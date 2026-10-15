# mragui.dev

Source for [mragui.dev](https://mragui.dev), a blog about hardware tinkering, firmware and side projects. Built with [Hugo](https://gohugo.io) and deployed to GitHub Pages on every push to `main`.

## Run locally

```bash
brew install hugo
hugo server -D    # http://localhost:1313 (-D shows drafts)
```

## New post

```bash
hugo new posts/my-post.md
```

Set `draft: false` when it's ready, then push.

Give every post a `description` (about 150–160 characters) and a few `tags`. The description is used for the search snippet, the social preview, the RSS feed and `llms.txt`.

## What the build generates

- **Social preview images**: a 1200×630 card per page (logo plus title), made at build time by `layouts/_partials/og-image.html`.
- **SEO**: canonical URLs, Open Graph/Twitter tags, JSON-LD (`WebSite`, `Person`, `BlogPosting`, breadcrumbs), a sitemap with git-based `lastmod`, and a full-content RSS feed.
- **For LLMs and AI search**: `/llms.txt` (index), `/llms-full.txt` (every post in one file), and a Markdown copy of each page at `<page>/index.md`. `robots.txt` explicitly allows AI crawlers.

Logo assets live in `static/images/brand/`, the favicons and app icons in `static/`. The heading font is Source Serif 4 (SIL OFL, see `static/fonts/OFL.txt`).
