# mragui.dev

Source for my personal blog, built with [Hugo](https://gohugo.io). There's no theme dependency: the layouts are in `layouts/` and the styles in `static/css/style.css`.

## Write locally

```bash
brew install hugo
hugo server -D        # http://localhost:1313  (-D also shows drafts)
hugo new posts/my-new-post.md
```

Posts in `content/posts/` stay hidden while they have `draft: true`. Set it to `false` to publish.

## First-time setup

1. **Create the repo.** Make a new **public** GitHub repo, for example `AguiMr/mragui.dev`, and push this folder to its `main` branch.
2. **Turn on Pages.** In the repo, go to **Settings → Pages → Build and deployment → Source: GitHub Actions**. The workflow in `.github/workflows/hugo.yml` then builds and deploys the site on every push.
3. **Custom domain.** Still on **Settings → Pages**, enter `mragui.dev` as the custom domain, wait for the DNS check to pass, then tick **Enforce HTTPS**. (`.dev` only works over HTTPS.)
4. **DNS.** Add these records at your registrar. If the domain is at Cloudflare, set them to **DNS only** (grey cloud) until GitHub has issued the certificate.

   | Type | Name | Value |
   |---|---|---|
   | A | @ | 185.199.108.153 |
   | A | @ | 185.199.109.153 |
   | A | @ | 185.199.110.153 |
   | A | @ | 185.199.111.153 |
   | AAAA | @ | 2606:50c0:8000::153 |
   | AAAA | @ | 2606:50c0:8001::153 |
   | AAAA | @ | 2606:50c0:8002::153 |
   | AAAA | @ | 2606:50c0:8003::153 |
   | CNAME | www | aguimr.github.io |

5. **Verify the domain (recommended).** Go to GitHub → your profile **Settings → Pages → Add a domain**. This stops anyone else from claiming mragui.dev on GitHub.

## Keep the alias separate from your real name

Commits are public. Before your first push, set the GitHub no-reply email:

```bash
git config user.name  "AguiMr"
git config user.email "<id>+AguiMr@users.noreply.github.com"   # find it under GitHub → Settings → Emails
```
