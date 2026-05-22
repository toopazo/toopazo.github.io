# CLAUDE.md — toopazo.github.io

Personal blog / portfolio of Tomás Opazo. Built with **Jekyll 4.2** and hosted on GitHub Pages.

## Stack

- **Jekyll 4.2** (Ruby, via Bundler)
- **Sass** (compiled by Jekyll) — source lives in `_sass/`, compiled output lands in `assets/css/`
- **KaTeX** (bundled in `assets/katex/`) for math rendering in posts
- **Font Awesome** icons (bundled in `assets/fontawesome/` and `_data/font-awesome/`)
- Language: Spanish (`lang: "es"` in `_config.yml`)

## Running locally

```bash
bundle exec jekyll serve --livereload
```

Site is served at `http://127.0.0.1:4000`. Rebuilt on file save when `--livereload` is active.

## Key directories

| Path | Purpose |
|---|---|
| `_posts/` | Blog posts — filename format `YYYY-MM-DD-slug.md` |
| `_layouts/` | Page templates (`default.html`, `page.html`, `post.html`) |
| `_includes/` | Reusable partials (header, sidebar, menu, embed helpers) |
| `_sass/` | Sass source files (`index.sass`, `layout.sass`, `basic.sass`, `classes.sass`, `font.sass`) |
| `assets/` | Static assets: CSS output, images, docs (PDFs), fonts, KaTeX, Font Awesome |
| `_config.yml` | Site-wide configuration (title, author, navigation, plugins, layout flags) |
| `_data/` | Structured data (Font Awesome icon registry) |
| `excluded/` | Draft/archived posts and old template — excluded from Jekyll build |

## Adding a post

Create `_posts/YYYY-MM-DD-slug.md` with front matter:

```yaml
---
title: "Post title"
layout: post
---
```

Optional front matter fields: `mathjax: true` (enables KaTeX), `categories`.

## Layout flags (`_config.yml`)

- `show_excerpts` — show article excerpts on home page
- `show_frame` — adds gray frame (uses `frame.css`); if false uses `index.css`
- `show_sidebar` — toggle sidebar instead of header
- `minimal` — dark header variant

## Sass compilation

Jekyll compiles `_sass/*.sass` automatically. The entry files in `assets/css/*.sass` import from `_sass/`. Do not edit files under `assets/css/` directly — edit `_sass/` source.

## Scope / permissions for Claude Code

- Full CRUD inside this repo: edit posts, layouts, includes, Sass, config, assets.
- `git`, `bundle exec jekyll *`, `cat`, `grep`, `find` are all fair game.
- Do **not** touch anything outside this repo without asking.
- No `sudo`, no `/etc`, no global gem installs, no system-level changes.
