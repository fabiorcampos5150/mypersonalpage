# Fábio Campos · personal page

Personal site and writing about AI agent evaluation, built with [Astro](https://astro.build) and deployed to GitHub Pages.

## Add an article

Create a Markdown file in `src/pages/writing/` with this front matter, then push to `main`:

```markdown
---
layout: ../../layouts/Post.astro
title: Your title
description: One sentence for the list and for link previews.
date: 2026-12-01
readingTime: 5 min read
---
```

## Run locally

```bash
npm install
npm run dev
```

## Custom domain

To use your own domain, add it in Settings → Pages, then in `astro.config.mjs` set `site` to that domain and remove `base`.
