# latechlean.github.io

Course website for **Math 490/587 — Interactive Theorem Proving with Lean** at
Louisiana Tech University.

Live at <https://latechlean.github.io>. Course materials live in a separate
repo: [LATechLean/ITPLxS25](https://github.com/LATechLean/ITPLxS25).

## Running it locally

Requires Ruby and Bundler.

```bash
bundle install
```

```bash
bundle exec jekyll serve
```

The site is then at <http://127.0.0.1:4000>. Pass `--port 4001` if that port is
busy. Edits to posts, pages, layouts and styles rebuild automatically;
**changes to `_config.yml` require restarting the server.**

Note: the Gemfile includes `logger` explicitly. It left Ruby's default gems in
Ruby 4.0, and Jekyll 4.4 still expects it — without it the server won't boot on
Ruby 4.x.

## Adding a post

Create `_posts/YYYY-MM-DD-slug.markdown` with front matter:

```yaml
---
layout: post
title: "Homework 3"
date: 2025-04-15 10:00:00 -0600
categories: homework
---
```

The first entry in `categories` is shown as a tag on the homepage list and on
the post itself. Existing categories are `welcome`, `homework` and `projects`.
Posts appear newest-first automatically; nothing needs registering.

For a standalone page, add a file with `layout: page`, a `title`, and a
`permalink`. To have it appear in the nav, add its path to `header_pages` in
`_config.yml` — the nav is an explicit list, not everything in the repo.

## Layout of the repo

This site does **not** use a gem theme. It was originally stock
[minima](https://github.com/jekyll/minima), but the brand system fought minima's
defaults at every turn, so the theme was replaced with a small one in-repo.
Everything that renders a page is here and editable.

```
_layouts/     default, home, page, post
_includes/    head, header, footer, hero, lockup, gator-lockup
_sass/        latechlean.scss (the whole design system), syntax.scss (code blocks)
assets/       main.scss (entry point) + image assets
_posts/       announcements
```

`assets/main.scss` is the stylesheet entry point; it pulls in the two partials
with `@use`. That requires Dart Sass, which matters for CI — see below.

## The marks

The wordmark is "LA Tech Lean" with two letter substitutions: the **A of LA is
the logical ∀**, the **E of Tech is ∃**.

Both glyphs are real Archivo Bold letters transformed in SVG — the ∀ is an `A`
flipped vertically, the ∃ an `E` mirrored horizontally, each in a box matching
the real letter's advance width so spacing holds at any size. **Do not swap
these for the Unicode characters ∀ (U+2200) and ∃ (U+2203)** — they fall back to
a math font and won't match the surrounding type.

The markup lives in `_includes/lockup.html` and is used at three sizes, set by
the class passed in: `ll-word--header`, `ll-word--hero`, `ll-word--footer`.
Change the include and every instance follows.

The nav also carries the **gator lockup** (`_includes/gator-lockup.html`): an
alligator silhouette with Lean's turnstile `⊢` running into its closed snout —
the gator taking the goal off the board. It is roughly 6:1, so it belongs in
headers, never in a square, and holds down to 34px tall. The gator must paint
*over* the bar; the bar is drawn long on purpose so the snout swallows its end.

Square uses (favicon, avatar) take the plain ∀ instead.

### Tokens

| Token | Value |
| --- | --- |
| Ink / text | `#201e1d` |
| Accent red | `#ec3013` |
| Accent red, text-safe on light | `#b8250f` |
| Page ground | `#f3f2f2` |
| Font | Archivo (400/500/700) |
| Divider | 2px solid ink |
| Corner radius | 0 everywhere |

The system is Modernist: flat, flush left, zero radius, 2px rules, red used
sparingly.

## Deployment

Pushes to `main` build and deploy via GitHub Actions
(`.github/workflows/build-deploy.yml`). Pull requests build too, but publish
nothing.

The workflow builds from this repo's `Gemfile.lock` rather than using
`actions/jekyll-build-pages`. That is deliberate: the prebuilt action uses the
`github-pages` gem (Jekyll 3.x with LibSass), which does not support the `@use`
rules in `assets/main.scss` and ignores the Jekyll 4.4 pin. Building from the
lockfile keeps CI and local identical.

This requires the repo's **Pages source to be set to "GitHub Actions"** under
Settings → Pages, not "Deploy from a branch".

## Credits and caveats

- Alligator silhouette: [PhyloPic](https://www.phylopic.org), CC0, by Ananth
  Srinivas. It is a raster at 1536px — fine on screen, but trace it to a vector
  before printing large.
- Louisiana outline (`assets/avatar-la-forall.png`, used for the
  apple-touch-icon and social image; `favicon.png` is the plain ∀): Natural
  Earth, public domain. Note the current brand sheet supersedes this mark in
  favour of the gator, so those two `head.html` tags are worth revisiting.
- Archivo is served from Google Fonts.

Louisiana Tech's own institutional mark is a **trademark**. The marks here are
deliberately distinct — a ∀ rather than an outlined T, a different palette —
but clear them with the university before any official use.
