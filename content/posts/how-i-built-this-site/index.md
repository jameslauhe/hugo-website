---
title: "How I Built This Site with Hugo, PaperMod, and Cloudflare Pages"
date: 2026-08-15
draft: false
tags: ["hugo", "cloudflare", "meta"]
summary: "A quick look at the stack behind this site: Hugo, the PaperMod theme, and Cloudflare Pages for hosting."
ShowToc: true
---

## The stack

This site is built with [Hugo](https://gohugo.io/), a static site generator written in Go, using the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme. It's hosted on [Cloudflare Pages](https://pages.cloudflare.com/), which builds and deploys the site automatically on every push.

I picked this combination because it's fast, simple to maintain, and doesn't need a database or server to run — just static files served from Cloudflare's edge network.

## Structure

The site has a few sections:

- **Writing** (`/posts/`) — blog posts like this one.
- **Creative** (`/projects/`) — a showcase of personal projects.
- A handful of static pages: Contact, Privacy, and Terms.

There's also a private, login-gated section that isn't linked from anywhere public — more on that below.

## The private section

Part of the goal of this site was to have somewhere to keep drafts and other content that isn't ready (or meant) for a public audience. Rather than build a custom login system, that section is gated at the edge using [Cloudflare Access](https://developers.cloudflare.com/cloudflare-one/policies/access/): a policy restricts the `/private/*` path to a single authorized email address, using a one-time login code. Hugo itself has no idea any of this is happening — it just builds the pages like any other; Cloudflare intercepts the request before it ever reaches them.

## What's next

Real content, mostly — this post and its sibling are placeholders to prove the structure works. Expect actual writing and projects to replace them soon.
