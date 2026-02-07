---
title: "Actually Static: When WordPress Stops Being the Enemy"
date: "2026-02-07T15:57:23+00:00"
lastmod: "2026-02-07T16:04:18+00:00"
draft: false
description: "This is a submission for the GitHub Copilot CLI Challenge What I Built I spent years avoiding WordPress for public-facing sites. Too slow, too heavy, too much maintenance. Then I..."
categories:
  - Uncategorized
author: githubcopilotchallenge
image: /images/1444/featured.webp
image_formats:
  - /images/1444/featured.webp
  - /images/1444/featured.avif
---

*This is a submission for the [GitHub Copilot CLI Challenge](https://dev.to/challenges/github-2026-01-21)*

## What I Built

I spent years avoiding WordPress for public-facing sites. Too slow, too heavy, too much maintenance.

Then I spent months tweaking Hugo themes, debugging build failures, and wrestling with image optimization pipelines.

Today, I use both. Not as competitors, but as partners.

Here’s the paradox I kept running into: **WordPress is probably the best writing environment ever built**. The interface is mature, the editor works, and you can focus on what matters — writing.

**Static site generators are probably the best deployment target ever built**. Fast, secure, cheap to host, and scalable by default.

So why do we keep choosing between them?

**In a comment on [one of my previous posts](https://dev.to/pascal_cescato_692b7a8a20/from-wordpress-to-astro-three-days-to-reclaim-control-5dn2)**, a reader captured this tension perfectly:

> “I spent quite a lot of time tweaking Hugo sites instead of writing, and I’m afraid I’ll do the same thing if I transfer to Astro.”

That’s the real problem. Not WordPress. Not Hugo. The friction between writing and deploying.

The problem with keeping WordPress public-facing isn’t the editor — it’s everything else.

Exposing a complete WordPress site to the public means decent hosting, heavy dependency on plugins — at minimum for security and SEO — and serious maintenance.

And by default, performance is variable, to say the least.

### The Search for an Alternative

This observation gradually led me to look for a different solution.

For some time now, I’ve been moving my content publishing to static sites.

But what I really wanted was simpler: keep WordPress as a writing environment while completely removing its public presence.

At a time when static sites can be deployed in seconds on almost any infrastructure, keeping WordPress as a frontend doesn’t always make much sense anymore — as long as you have a robust deployment solution.

Write normally. Publish automatically.

No manual export, no scripts to run, no friction.

### Existing Tools: Effective but Heavy

Having dozens of articles on my WordPress blogs, I developed a suite of Python tools capable of exporting a complete site to Hugo or Astro.

Functional, reliable, but based on a global export logic: complete site generation, transformation, then deployment.

An effective process, but heavy.

And especially unnatural in a daily writing workflow.

This search for a more fluid editorial workflow gave birth to the project.

**The solution**: A WordPress plugin that automatically syncs published posts to a GitHub repository, converts them to Markdown with Hugo-compatible front matter, optimizes images (WebP/AVIF), and triggers a GitHub Actions workflow for static site deployment.

**Tech Stack**:

- WordPress 6.9+ (PHP 8.1+)
- GitHub API (atomic commits via Trees API)
- Action Scheduler (async processing)
- Intervention Image (local optimization)
- Hugo (static site generator)
- GitHub Actions + GitHub Pages (deployment)

**Key Features**:
✅ Fully asynchronous sync (no admin blocking)
✅ Atomic commits (Markdown + all images in one commit)
✅ Native WordPress APIs only (WordPress.org compliant)
✅ Multi-format image optimization (AVIF → WebP → Original)
✅ Zero shell commands (100% GitHub API)
✅ HTTPS + Personal Access Token authentication
✅ WP-CLI commands for bulk operations

## Demo

The workflow is now operational.

Articles are written in WordPress, as before.

When publishing or updating, a dedicated plugin automatically triggers synchronization to a GitHub repository.

Each piece of content is converted to Markdown with Hugo-specific front matter, along with optimized images (WebP and AVIF).

Everything is sent in a single commit via the GitHub API.

A GitHub Actions workflow then takes over: static site generation, then deployment to GitHub Pages.

Concretely, publishing in WordPress is now enough to put a complete static version of the site online, without manual export or additional intervention.

### The Real-World Gauntlet

This wasn’t a smooth 48-hour sprint. The project survived several reality checks:

**Hugo Theme Version Hell**: The theme I wanted required Hugo 0.146.0 minimum. My local install was 0.139.0. GitHub Actions defaulted to 0.128.0. Each environment needed explicit version pinning, and debugging failures meant decoding cryptic TOML errors across three different build contexts.

**GitHub Pages URL Stuttering**: The deployed site initially rendered with broken internal links because Hugo’s `baseURL` configuration didn’t match GitHub Pages’ expectations. Pages built locally worked fine. CI builds deployed with relative paths pointing to void. Solution: hardcode the production URL in the workflow, accept that local previews would have slightly broken navigation.

**Image Pipeline Memory Limits**: Processing 10+ images per post with AVIF encoding pushed PHP’s memory limits on shared hosting. First attempt: fatal errors. Second attempt: disable AVIF, keep WebP. Final solution: increase `memory_limit` to 512M and batch-process images sequentially instead of in parallel.

**Action Scheduler Race Conditions**: Early versions created duplicate commits when saving a post multiple times quickly. WordPress’s `save_post` hook fires on autosaves, manual saves, and quick edits. Needed: debouncing logic, transient locks, and post meta flags to prevent redundant syncs.

None of this was in the initial specifications. All of it was mandatory to ship.

### The Time Investment Reality

Here’s what “48 hours” actually meant:

<table class="has-fixed-layout"><thead><tr><th>Task</th><th>Manual (estimated)</th><th>With Copilot CLI</th><th>Actual Savings</th></tr></thead><tbody><tr><td>Plugin boilerplate + WordPress standards</td><td>~4 hours</td><td>20 minutes</td><td>3h 40m</td></tr><tr><td>GitHub API integration (Trees, refs, commits)</td><td>~6 hours</td><td>1.5 hours</td><td>4h 30m</td></tr><tr><td>Image optimization pipeline</td><td>~5 hours</td><td>2 hours</td><td>3h</td></tr><tr><td>Async queue setup (Action Scheduler)</td><td>~3 hours</td><td>45 minutes</td><td>2h 15m</td></tr><tr><td>Admin UI + settings page</td><td>~4 hours</td><td>1 hour</td><td>3h</td></tr><tr><td>Hugo adapter (Markdown + front matter)</td><td>~2 hours</td><td>30 minutes</td><td>1h 30m</td></tr><tr><td>Debugging real-world issues</td><td>~8 hours</td><td>~8 hours</td><td>0h (no AI help here)</td></tr><tr><td>**Total**</td><td>**~32 hours**</td><td>**~14 hours**</td><td>**~18 hours**</td></tr></tbody></table>

Copilot accelerated structured implementation. It did nothing for architectural decisions, debugging environment-specific failures, or understanding WordPress.org compliance requirements.

### Why Local Image Optimization Matters

The plugin processes images **on the WordPress server before uploading to GitHub**. This is crucial:

**Without local optimization**:

- Upload 5MB original JPEGs to GitHub
- GitHub Actions must download, process (ImageMagick/Sharp), then deploy
- Build time: 2-3 minutes per post
- GitHub Actions runner minutes consumed: high
- Failed builds leave orphaned large files in Git history

**With local optimization** (current approach):

- WordPress generates AVIF (50-150KB) + WebP (100-300KB) + original
- Upload ~500KB total per post to GitHub
- GitHub Actions just copies files, no processing
- Build time: 15-30 seconds
- Clean Git history, minimal runner usage

The trade-off: PHP memory limits and processing time on the WordPress side. But WordPress is idle 99% of the time. GitHub Actions runners cost money per minute.

Processing locally shifts the bottleneck to where it’s free.

### Architecture

**What’s Currently Handled**:

✅ **Posts and Pages**: Both sync automatically with proper Hugo front matter
✅ **Deletions**: Trashing a post/page in WordPress triggers file deletion in GitHub
✅ **Updates**: Editing content re-syncs, overwriting existing files
✅ **Categories and Tags**: Converted to Hugo taxonomies in front matter
✅ **Featured Images**: Optimized and linked in front matter (`featured_image` field)
✅ **Custom Fields**: Basic fields map to front matter (extensible via adapter)

**Current Limitations** (MVP scope):

⚠️ **Draft Handling**: Drafts stay in WordPress, never sync (intentional)
⚠️ **Revisions**: Only published versions sync, revision history stays local
⚠️ **Complex Blocks**: Gutenberg blocks convert to HTML, then basic Markdown (no advanced block preservation)
⚠️ **Shortcodes**: Rendered to HTML before conversion (loses original shortcode)
⚠️ **ACF/Meta Boxes**: Only standard custom fields supported (ACF requires custom adapter extension)
⚠️ **Author Pages**: Not yet implemented (single-author blogs work fine)

**Deliberate Trade-offs**:

WordPress remains the source of truth. The plugin doesn’t sync bidirectionally. If you edit Markdown directly in GitHub, those changes won’t flow back to WordPress. This is intentional — simplicity over complexity.

**Future Refactoring Scenarios** (TODO):

One critical question remains unresolved: what happens when you need to change Hugo themes, restructure content directories, or migrate to a different SSG?

The current architecture binds tightly to Hugo’s conventions (`content/posts/`, specific front matter fields). A theme change requiring different front matter or file paths would need:

- Adapter refactoring
- Bulk re-sync of all posts
- Manual cleanup of old file structure in Git

This is the price of the current design. A more abstract adapter layer could mitigate this, but at the cost of initial complexity. For now, it’s easier to rebuild than to over-engineer upfront.

The repository contains:

- WordPress plugin (WordPress.org compliant)
- GitHub API integration (atomic commits)
- Asynchronous sync management (Action Scheduler)
- Hugo-compatible Markdown generation
- GitHub Actions workflow for deployment

### How It Works

The process in detail:

**1. Writing**: Standard WordPress interface, no change in the writing experience

![WordPress Editor](https://via.placeholder.com/800x400?text=WordPress+Editor)

**2. Automatic Commit**: The GitHub repository receives Markdown, optimized images, and front matter

![GitHub Commit](https://via.placeholder.com/800x400?text=GitHub+Atomic+Commit)

**3. Hugo Structure**: `content/posts/` structure automatically generated with correct naming

![Hugo File Tree](https://via.placeholder.com/800x400?text=Hugo+Content+Structure)

**4. Deployed Site**: Static version online via GitHub Pages, optimal performance

![Static Site](https://via.placeholder.com/800x400?text=Static+Site+Live)

The whole thing forms a simple publishing chain: write in WordPress, publish, and let the rest execute.

**Repository**: \[github.com/username/wp-jamstack-sync\] *(link to add)*

### Technical Highlights

**Atomic Commits via GitHub Trees API**:
Instead of multiple sequential commits (one per image, one for Markdown), the plugin uses GitHub’s Git Data API to create a single commit containing all files:

```
// Collect all files (Markdown + images)
$all_files = [
    'content/posts/2024-02-07-my-post.md' => $markdown_content,
    'static/images/photo.webp' => $webp_binary,
    'static/images/photo.avif' => $avif_binary,
];

// Single atomic commit
$git_api->create_atomic_commit($all_files, "Publish: My Post Title");
```

This approach is transactional: either everything commits or nothing does. No partial states, cleaner history.

**WordPress-Native Compliance**:
To meet WordPress.org requirements, the plugin uses exclusively native WordPress APIs:

- `wp_remote_post()` instead of curl
- `WP_Filesystem` instead of `file_put_contents()`
- `$wpdb` prepared statements
- No `exec()`, `shell_exec()`, or Git CLI

This makes it suitable for publication in the official WordPress plugin repository.

## My Experience with GitHub Copilot CLI

When I started this project, I wasn’t looking for a tool to code for me.

I was looking for a way to accelerate the execution of a project whose architecture was already clear.

Having already used GitHub Copilot CLI, Gemini CLI, and various LLMs on other projects, I knew these tools could produce code quickly.

But I also knew that without a precise framework, they mainly produce… code.

Not necessarily a coherent system.

**Note**: This isn’t the autocomplete in the editor, but a command-line tool capable of generating complete files from structured prompts.

### Specification First, Code Second

The first step wasn’t to code. The first step was to write specifications. Define the scope. Break down the project into functional blocks.

Identify non-negotiable constraints: WordPress native only, no shell execution, reliable async processing, atomic GitHub commits, WordPress.org compliance to publish the plugin in the official repository and benefit the community.

Then organize development into successive stages.

This work is very similar to what a technical project manager would do before entrusting implementation to a team. The difference here is that the “team” consisted of a tool capable of producing code very quickly — but only if the instructions were clear and precise.

So I didn’t write code in the traditional sense. I wrote functional and technical specifications, prompts, refined instructions, corrected trajectories.

Each step consisted of describing what needed to be built, verifying what was produced, then adjusting.

Sometimes Copilot proposed a relevant structure on the first try. Sometimes it required reworking, clarifying, constraining further.

### The Work Pattern

Very quickly, a work pattern emerged: specification → generation → verification → correction → iteration.

In this process, Copilot behaves less like a magic generator than like a fast executor.

It can structure an entire class in seconds, propose a coherent implementation, or refactor a complete block.

But it can also forget an essential hook, overwrite an existing method, or produce functional code that doesn’t comply with initial constraints.

**Real Examples of Issues**:

- Method replaced by an incomplete stub
- Hook not registered, causing silent failures
- File generated but not actually written to disk
- Fatal error on activation, typical of strict WordPress environments

Each incident required going back to fundamentals: verify, understand, correct, reformulate.

### What This Actually Means

This is probably the most interesting aspect of the experience.

Using Copilot effectively doesn’t mean writing one prompt and waiting for a result.

It’s much more like continuous piloting, where the quality of instructions directly conditions the quality of what’s produced.

In this context, the tool becomes particularly effective for accelerating everything that’s structured: class creation, file organization, repetitive function implementation, refactoring, documentation.

As soon as the objective is clearly defined, execution can become very fast.

But the responsibility for architecture, technical choices, and overall coherence remains entirely human.

### The Real Value

In the end, the experience is less like “AI-assisted development” than a form of assisted technical direction.

Code is produced quickly, but it must be thought out, supervised, and validated continuously.

This project was built in less than two days. Not because the tool replaces design work, but because once that work is done, execution can be considerably accelerated.

This is probably where GitHub Copilot CLI becomes most interesting: it’s not a substitute for development, but an accelerator for an already thought-out and structured project.

**What worked well**:
✅ Rapid scaffolding of classes following WordPress standards
✅ Boilerplate code generation (hooks, filters, nonces)
✅ Refactoring large blocks (sequential commits → atomic commits)
✅ Documentation generation from inline comments

**What required constant supervision**:
⚠️ Architecture decisions (adapter pattern, async queues)
⚠️ WordPress.org compliance verification
⚠️ Error handling and edge cases
⚠️ Integration testing across components

### Conclusion

GitHub Copilot CLI didn’t replace development.

It didn’t eliminate the need to think, architect, or decide.

But used as an execution partner rather than an automatic generator, it made it possible to quickly transform a clear idea into a functional system.

That’s perhaps where these tools really make sense: they don’t change the way we build, but they reduce the distance between what we imagine and what we put into production.

In this specific case, they made it possible to solve a real tension: write comfortably in WordPress while publishing to a high-performance static site.

No friction. No compromises.

Just a workflow that works.