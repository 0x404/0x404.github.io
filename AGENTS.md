# AGENTS.md

## Cursor Cloud specific instructions

This is an **al-folio** Jekyll-based academic portfolio/website. It is a single-service static site generator (not a monorepo).

### Key services

| Service | Command | Port | Notes |
|---------|---------|------|-------|
| Jekyll dev server | `bundle exec jekyll serve --port=8080 --host=0.0.0.0 --livereload` | 8080 (+ 35729 for LiveReload) | Set `JEKYLL_ENV=development` and `EXECJS_RUNTIME=Node` |

### System dependencies (pre-installed in snapshot)

Ruby 3.2, ImageMagick, inotify-tools, Node.js, Python 3 + nbconvert. These are installed via `apt` and `pip` — not managed by the update script.

### Developing

- **Gems** are installed to `vendor/bundle` (via `bundle config set --local path 'vendor/bundle'`). This avoids needing `sudo`.
- **Lint**: `npx prettier --check "**/*.{html,liquid,js,css,scss,json,yml,yaml,md}"` — uses Prettier with the Shopify Liquid plugin.
- **Build/serve**: `bundle exec jekyll serve --port=8080 --host=0.0.0.0 --livereload` (see also `bin/entry_point.sh` for the Docker-based approach).
- The first build takes ~2-3 minutes because ImageMagick generates responsive WebP images for all images in `assets/img/`. Subsequent rebuilds with `--watch` are fast.
- There is no automated test suite in this repo. Validation is done via Prettier formatting checks and manual browser testing.

### Gotchas

- The `jekyll-terser` gem is sourced from a **git repo** (`https://github.com/RobertoJBeltran/jekyll-terser.git`), not RubyGems. `bundle install` needs network access to fetch it.
- `nbconvert` is installed via `pip install --break-system-packages nbconvert` (user-level) because the system Python is managed by Ubuntu. The binary lands in `~/.local/bin` — ensure this is on `PATH`.
- The site's available pages depend on files in `_pages/`. The default setup includes: `/` (about), `/publications/`, `/news/`, `/repositories/`, and `/assets/pdf/CV.pdf` (CV). Blog and projects pages may not exist unless added.
