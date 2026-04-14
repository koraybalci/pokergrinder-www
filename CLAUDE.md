# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is the marketing/landing page website for the **Poker Grinder** mobile app (https://pokergrinder.app), a bankroll and preflop action tracker. It's a Jekyll 4.3.3 static site with a Gulp-based frontend asset pipeline.

## Commands

### Jekyll (site content/structure)
```bash
bundle exec jekyll serve       # Local dev server with live reload
bundle exec jekyll build       # Build static site to _site/
```

### Gulp (CSS/JS asset pipeline)
```bash
gulp                           # Build CSS+JS then watch for changes (default)
gulp build                     # One-time build of CSS and JS
gulp sass                      # Compile SCSS → CSS only
gulp js                        # Bundle and minify JS only
npm run dev                    # gulp production: clean dist/, copy files, create zip
```

Run `gulp build` (or `gulp`) before `jekyll serve` if assets are stale.

## Architecture

### Asset Pipeline (Gulp)
Gulp manages the frontend assets — Jekyll just uses the output files directly:

1. **CSS**: `assets/scss/*.scss` → compiled → `assets/css/style.min.css`, then merged with vendor CSS (Bootstrap 4, AOS, Owl Carousel from `node_modules`) → **`assets/css/app.min.css`** (what the layout loads)
2. **JS**: vendor libs from `node_modules` + `assets/js/app.js` concatenated → **`assets/js/build.min.js`** (what the layout loads)

The `<!-- gulp:css -->` / `<!-- endgulp -->` comments in `_layouts/default.html` mark the injection points.

### Jekyll Structure
- **`_layouts/`** — `default.html` is the base template; `post.html`, `page.html`, `blog.html`, `category.html`, `author.html`, `privacy.html` extend it
- **`_includes/`** — modular HTML fragments included in layouts/pages (header, footer, navigation, section components like `services.html`, `apps_area.html`, etc.)
- **`_data/`** — YAML-driven content: `services.yml` (the 3 app feature cards on the homepage), `features.yml` (unused placeholder data)
- **`_authors/`** — author collection pages
- **`_posts/`** — blog posts (currently all placeholder content, not actively used)
- **`_site/`** — Jekyll build output, not committed

### Homepage
`index.html` uses `layout: default` and includes only `services.html`, which renders the three key app features from `_data/services.yml`. Most other sections (video, apps store, testimonials, pricing, blog area) are commented out.

### Unused Files
Several HTML files at the root are suffixed `-unused` (about, authors, blog, contact) — they exist as templates but are not part of the active site navigation.

### Plugins
- `jekyll-paginate-v2` + `jekyll-archives` — configured in `_config.yml` for potential blog use
- `jekyll-feed` — generates RSS at `/index.xml`
- `jekyll-seo-tag` — SEO meta tags
