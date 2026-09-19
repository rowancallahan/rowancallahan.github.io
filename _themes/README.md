# Saved themes (not in use)

Folders starting with `_` are ignored by Jekyll, so nothing here is built or published.
The live site uses AcademicPages (repo root). The previous Minima site is git tag `minima-theme-2026-09`.

- `instinct/` — browser-default look (Times New Roman, 48em column) with a small picker in the
  top right: plain, dark, solarized light, solarized dark. Uses the Minima gem theme plus the
  overrides here (`_includes/theme-picker.html`, `_includes/font-picker.html`, `assets/main.scss`).
- `console/` — `jekyll-theme-console` gem (monospace, terminal style); only `_layouts/post.html`
  is overridden.

Each folder holds the theme's own `_config.yml`, `Gemfile`, layouts/includes and the page files
(`index.md`, `posts.md`, ...). Posts, images and PDFs are not duplicated; take them from the repo root.

## Switching to one

1. Copy the root `_posts/`, `pdfs/` and the post images (`assets/*.png`) somewhere safe.
2. Replace the repo root with the theme folder's contents, then put `_posts/`, `pdfs/` and the
   images back (images go in `assets/`).
3. `bundle install && bundle exec jekyll serve` to check, then commit.

Local previews of every variant: `~/website/variants/` (`build_all.sh`, gallery on port 4010).
