---

title: "Why I Finally Stopped Fighting My Publishing Workflow"
date: 2026-02-08T13:25:01+00:00
cover:
  image: "images/1487/featured.avif"  # Utilisation de 1487 au lieu de why-i-finally-stopped-fighting-my-publishing-workflow
  alt: "Why I Finally Stopped Fighting My Publishing Workflow"

---

For years, I maintained two separate worlds.

**World One**: WordPress. Comfortable writing interface, familiar editor, everything just works. But slow page loads, constant security updates, plugin conflicts, and that nagging feeling that I’m running a Boeing 747 to deliver a postcard.

**World Two**: Static sites. Blazing fast, secure by default, costs pennies to host. But writing in Markdown files, committing to Git, running build commands, debugging deployment pipelines. I spent more time being a DevOps engineer than a writer.

The problem wasn’t technical. The problem was philosophical.

**I wanted to write like a human, but deploy like a machine.**

## The Breaking Point

Last month, I wrote three drafts. Two of them sat in WordPress, unpublished, because deploying to my Hugo site meant:

1. Copy content to a Markdown file
2. Format the front matter correctly
3. Download images from WordPress
4. Optimize them (WebP? AVIF? Both?)
5. Upload to the right directory
6. Commit everything to Git
7. Push and wait for CI/CD
8. Debug why the featured image didn’t show up
9. Fix it, commit again
10. Wait again

By step 5, I’d lost interest in publishing.

## The Realization

WordPress isn’t the enemy. Hugo isn’t the enemy.

**Friction is the enemy.**

What if I could write in WordPress and publish to Hugo without thinking about it?

What if hitting “Publish” in WordPress meant:

- Markdown automatically generated
- Images automatically optimized (WebP + AVIF)
- Front matter automatically filled
- Everything committed atomically to GitHub
- Site rebuilt and deployed in seconds

No export. No manual steps. No context switching.

## The Result

I’m writing this post in WordPress.

When I click “Publish”, you’ll read it on a static Hugo site hosted on GitHub Pages.

I didn’t copy a single line of Markdown. I didn’t touch Git. I didn’t run any build commands.

**I just wrote.**

And that’s exactly how it should be.

---

*This post was automatically synced from WordPress to Hugo using the [Atomic Jamstack Connector](https://github.com/pcescato/atomic-jamstack-connector). The entire workflow — conversion, optimization, commit, and deployment — happened in under 30 seconds.*

*Want to know how it works? Read the full story: [Actually Static: When WordPress Stops Being the Enemy](https://dev.to/pascal_cescato_692b7a8a20/actually-static-when-wordpress-stops-being-the-enemy-INSERT_POST_ID)*

## Technical Details (For the Curious)

If you’re wondering how this works under the hood:

- **Writing**: Standard WordPress editor (Gutenberg or Classic)
- **Conversion**: HTML → Markdown with customizable front matter
- **Images**: Local optimization (AVIF + WebP) before upload
- **Deployment**: Atomic Git commit via GitHub API
- **Build**: GitHub Actions triggers Hugo build
- **Hosting**: GitHub Pages (free, fast, global CDN)

The entire stack costs $0/month and handles thousands of visitors without breaking a sweat.

No WordPress security updates. No plugin conflicts. No performance optimization rabbit holes.

Just writing. Just publishing.

## Try It Yourself

Want to see this in action?

1. **Write a post** in the [demo WordPress](https://githubcopilotchallenge.tsw.ovh/wp-admin/) (login: `tester` / `Github~Challenge/2k26`)
2. **Hit “Publish”**
3. **Watch it appear** on the [live Hugo site](https://pcescato.github.io/hugodemo/) in seconds

**See the commits**: Check out [the Hugo repository](https://github.com/pcescato/hugodemo) to see the Markdown files, optimized images, and GitHub Actions workflow in action.

**Get the plugin**: [github.com/pcescato/atomic-jamstack-connector](https://github.com/pcescato/atomic-jamstack-connector)

---

**The best publishing workflow is the one you don’t notice.**

This is mine now.