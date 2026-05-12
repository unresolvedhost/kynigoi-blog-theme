# Kynigoi Jekyll Theme

A dark, technical Jekyll theme built for security researchers and detection engineers. Inspired by Elastic Security Labs.

![Jekyll](https://img.shields.io/badge/Jekyll-4.x-red)
![License](https://img.shields.io/badge/License-MIT-green)

## Features

- Dark / light theme toggle (respects OS preference, persists via localStorage)
- Bento-style featured posts layout
- Horizontal scrolling category strips — auto-generated from post front matter
- Per-category pages via `jekyll-archives`
- Sticky TOC sidebar on post pages with scroll tracking
- Animated wire wave SVG background
- Syntax highlighting via Rouge (GitHub-style token colours)
- Fully responsive
- Social links and favicon managed from `_config.yml`
- GitHub Actions deploy workflow included

## Quick Start

### Use as a template

Click **Use this template** on GitHub to create your own repo.

### Run locally

```bash
gem install bundler
bundle install
bundle exec jekyll serve
```

Visit `http://localhost:4000`

## Configuration

Edit `_config.yml` to set your site details:

```yaml
title: Your Blog Title
tagline: Your tagline
description: Your description
url: "https://yourusername.github.io"

social:
  email: you@example.com
  linkedin: your-linkedin
  github: your-github
  x: your-x-handle

favicon:
  ico:              /assets/img/favicons/favicon.ico
  apple_touch_icon: /assets/img/favicons/apple-touch-icon.png
  favicon_32:       /assets/img/favicons/favicon-32x32.png
  favicon_16:       /assets/img/favicons/favicon-16x16.png
```

## Writing Posts

Create a new file in `_posts/` following the naming convention `YYYY-MM-DD-title.md`:

```yaml
---
title: "Your Post Title"
date: 2025-05-07 09:00:00 +0800
categories: [Detection Engineering]   # Detection Engineering | Malware Analysis | Tools
tags: [windows, eql, elastic]
author: yourname
description: >
  A short description shown on post cards.
severity: HIGH                         # HIGH | MED | LOW (shown on post page)
mitre:
  - "T1218 — Signed Binary Proxy Execution"
read_time: 10
featured: true                         # shows in Featured bento (max 3)
---

Your content here...
```

### Categories

Posts are grouped into strips on the home page automatically. Add any category to a post's front matter and it will appear. No config changes needed.

### Featured Posts

Mark up to 3 posts with `featured: true`. They appear in the bento layout at the top of the home page. If more than 3 are marked, only the 3 most recent are shown.

### Callout Boxes

```html
<div class="callout">
  <div class="callout-label">// note</div>
  Your note here.
</div>

<div class="callout warn">
  <div class="callout-label">⚠ warning</div>
  Your warning here.
</div>

<div class="callout danger">
  <div class="callout-label">✕ danger</div>
  Your danger note here.
</div>
```

## Favicon

Generate a favicon set at [realfavicongenerator.net](https://realfavicongenerator.net) and drop the files into `assets/img/favicons/`.

## Deploy to GitHub Pages

1. Push to GitHub
2. Go to **Settings → Pages → Source → GitHub Actions**
3. Every push to `main` triggers an automatic build and deploy

## Folder Structure

```
├── _config.yml
├── _layouts/
│   ├── default.html
│   ├── home.html
│   ├── post.html
│   ├── page.html
│   ├── category.html
│   └── tags.html
├── _includes/
│   ├── head.html
│   ├── nav.html
│   ├── footer.html
│   ├── background.html
│   └── post-card.html
├── _sass/
│   ├── kynigoi.scss
│   ├── _variables.scss
│   ├── _base.scss
│   ├── _nav.scss
│   ├── _home.scss
│   ├── _post.scss
│   ├── _strip.scss
│   └── _syntax.scss
├── _posts/
├── _pages/          # optional — not needed if using jekyll-archives
├── assets/
│   ├── css/
│   │   └── main.scss
│   └── img/
│       └── favicons/
├── index.html
├── about.md
├── tags.md
└── .github/
    └── workflows/
        └── deploy.yml
```

## License

MIT — free to use and modify.