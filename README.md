# hugo-website

Personal portfolio and blog, built with [Hugo](https://gohugo.io/) and the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme, hosted on [Cloudflare Pages](https://pages.cloudflare.com/).

## Local development

Requires Hugo **extended**, version 0.146.0 or newer (PaperMod's minimum).

```bash
git clone --recurse-submodules <repo-url>
cd hugo-website
hugo server -D    # includes drafts
```

If you cloned without `--recurse-submodules`, fetch the theme with:

```bash
git submodule update --init --recursive
```

Visit `http://localhost:1313`.

## Adding content

**A new blog post:**

```bash
hugo new content posts/<slug>/index.md
```

Fill in the front matter (`title`, `date`, `tags`, `summary`), write the post, then set `draft: false` when it's ready to publish.

**A new project:**

```bash
hugo new content projects/<slug>/index.md
```

Same pattern — fill in front matter, set `draft: false` to publish.

**Updating the PaperMod theme:**

```bash
git submodule update --remote --merge
```

## Deployment (Cloudflare Workers, static assets)

This repo deploys as a Cloudflare Worker serving static assets, configured entirely in-repo:

- **`wrangler.jsonc`** — Worker config: project name, compatibility date, build command, and the assets directory (`./public`, Hugo's output), with `404-page` handling (PaperMod ships a `404.html` layout).
- **`build.sh`** — installs pinned versions of Hugo (plus Dart Sass/Go/Node.js, only if this project ever needs them), initializes the `themes/PaperMod` git submodule, and runs `hugo build --gc --minify`.

To deploy: connect this repo in the Cloudflare dashboard (Workers & Pages → Create → connect to Git) so Workers Builds runs `build.sh` on every push, or deploy manually with `npx wrangler deploy` after running `./build.sh` locally.

Once a custom domain is attached, update `baseURL` in `hugo.toml` and `name` in `wrangler.jsonc` to match.

## Private section (`/private/`)

The `/private/` section is a placeholder for future unpublished drafts. It isn't linked from anywhere on the public site and its pages are excluded from search, RSS, and the sitemap (see the front matter on `content/private/*.md`) — but none of that is real access control. Access is enforced separately, at the edge, using **Cloudflare Access**:

1. In the Cloudflare dashboard, go to **Zero Trust** (the free plan covers a single user).
2. **Zero Trust → Settings → Authentication**: confirm **One-Time PIN** login is enabled (on by default).
3. **Zero Trust → Access → Applications → Add an application → Self-hosted**.
4. Set the **application domain** to the site's hostname (the `*.pages.dev` domain and/or custom domain), with **path** `/private*`.
5. Set a reasonable **session duration** (e.g. 24h).
6. Add a policy: Action **Allow**, Include → **Emails** → the single authorized email address, Identity provider: **One-time PIN**.
7. Save. Visiting `/private/` now prompts for an email code before the page is served; anyone not on the allow-list is rejected.

This configuration lives entirely in Cloudflare's dashboard — there's nothing to check into this repo for it to work.
