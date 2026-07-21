# Web specification

Technical companion to `context/styleguide.html`. The style guide says what the site looks like; this document says how it is built. The governing principle is the same as the design's: **boring, small, and durable beats clever.** Every decision below optimizes for a site that one person can fully understand, edit with a text editor, and still deploy without ceremony five years from now.

---

## 1. Architecture decision

**The site is hand-written static HTML and CSS. No framework, no build step, no bundler.**

Rationale: six pages plus a nested portfolio do not justify tooling. A build step is a dependency that can rot; plain HTML deployed to GitHub Pages cannot. The cost is that shared elements (nav, footer, `<head>`) are duplicated across pages — acceptable at this scale, and Section 9 defines the upgrade path if it stops being acceptable.

What this rules out:

- No React/Vue/Svelte, no client-side routing. Pages are documents.
- No CSS frameworks (Tailwind, Bootstrap). The entire visual system is ~150 lines of custom CSS.
- No JavaScript unless a specific interaction requires it, and then only vanilla JS in a single deferred file. The baseline site should work with JS disabled.
- No npm, no `node_modules`, no lockfiles in this repo.

---

## 2. Repository structure

```
/
├── index.html              # homepage hub
├── about/
│   └── index.html          # → /about/
├── projects/
│   └── index.html          # → /projects/
├── teaching/
│   └── index.html          # → /teaching/
├── cv/
│   └── index.html          # → /cv/
├── scicomm/                # existing portfolio, dropped in as-is
│   └── ...
├── assets/
│   ├── css/
│   │   └── main.css        # the one stylesheet
│   ├── js/                 # empty until genuinely needed
│   ├── img/                # images for the minimal site (not scicomm's)
│   └── files/
│       └── cv.pdf          # the downloadable CV
├── context/                # reference documents, unlinked from nav
│   ├── styleguide.html     # living design reference
│   ├── WEB_SPEC.md         # this document
│   └── BUILD_PROMPT.md     # original build prompt
├── README.md               # how to edit and deploy, 20 lines max
├── 404.html                # styled like everything else
└── CNAME                   # custom domain, if used
```

Conventions:

- **Folder-per-page with `index.html`** so URLs are clean (`/projects/`, not `/projects.html`). Internal links always use the trailing-slash form.
- **Root-relative URLs** (`/assets/css/main.css`, `/projects/`) everywhere in the minimal site, so links work from any depth. *Exception:* if deploying to `username.github.io/repo/` rather than a custom domain or user site, root-relative paths break — decide the final URL before writing links, and prefer a setup where the site lives at a domain root.
- **`/scicomm` is a tenant, not a citizen.** Its files keep their own structure and conventions; nothing in `/assets` styles it and nothing in it references `/assets`. The only contract: its internal asset paths must be relative (or `/scicomm/`-prefixed) so they resolve from the subfolder. Verify this on first deploy.
- **`/context` holds reference documents, not site pages.** Nothing in it is linked from the nav or built as a route; it exists for whoever maintains the site next.
- Lowercase, hyphenated filenames only (`long-now-atlas.jpg`). No spaces, no uppercase — GitHub Pages URLs are case-sensitive.

---

## 3. HTML conventions

- HTML5, `<!DOCTYPE html>`, `lang="en"` on `<html>`.
- **Semantic structure on every page:** one `<header>` (wordmark + nav on interior pages), one `<main>`, one `<footer>`, exactly one `<h1>`. Headings descend without skipping levels. Nav is `<nav><ul>` with a `aria-label="Main"`.
- A **skip link** (`<a href="#main" class="skip-link">Skip to content</a>`) is the first element in `<body>`, visually hidden until focused.
- Every page's `<head>` carries, in order: charset, viewport, `<title>` in the form `Page — Ricky Rojas` (homepage: name alone), meta description unique to the page, canonical URL, and Open Graph title/description/url. Keep the block identical in structure across pages so diffs are legible.
- The current page's nav link gets `aria-current="page"` (and the accent's non-hover style — it is not a link to nowhere).
- Images: always `width` and `height` attributes (prevents layout shift), always meaningful `alt` (or `alt=""` if decorative), `loading="lazy"` on anything below the fold.
- No inline styles, no inline event handlers, no `<div>` where a semantic element exists.
- Indent with 2 spaces. UTF-8, LF line endings (enforce with a `.editorconfig`).

---

## 4. CSS conventions

- **One file:** `assets/css/main.css`. At this scale, splitting CSS creates more navigation cost than it saves.
- **All design values are custom properties on `:root`,** matching the style guide exactly:

```css
:root {
  --paper: #FBF9F6;
  --ink: #22201B;
  --ink-soft: #3A362E;
  --muted: #756F63;
  --faint: #9A9081;
  --accent: #8C1515;
  --hairline: #E8E2D6;
  --measure: 34rem;
  --serif: 'Newsreader', Georgia, 'Times New Roman', serif;
  --mono: ui-monospace, 'SF Mono', Menlo, Consolas, monospace;
}
```

  **No raw hex values or font names appear anywhere below `:root`.** A color change is a one-line edit.
- File is organized top-to-bottom: tokens → reset (minimal: `box-sizing`, margin zeroing) → base element styles (`body`, headings, `p`, `a`) → layout (`.wrap`, header, footer) → components (teaser, project row) → utilities → media queries last.
- **Class naming:** simple, flat, kebab-case, component-scoped (`.project-row`, `.teaser`, `.site-nav`). No BEM ceremony, no utility-class soup, no IDs for styling. Specificity stays at one class deep; if you're writing `.a .b .c`, restructure instead.
- Mobile behavior via a single breakpoint (`max-width: 600px`) unless a real need proves otherwise. The single-column layout barely needs one.
- Honor `prefers-reduced-motion: reduce` by disabling all transitions/animations. (There should be almost none anyway — link underline on hover is the ceiling.)
- Comment *sections*, not lines: `/* ---- components ---- */`.

---

## 5. Typography loading

- Newsreader loads from Google Fonts with `<link rel="preconnect">` to both `fonts.googleapis.com` and `fonts.gstatic.com`, and `display=swap` in the CSS URL so text renders immediately in the Georgia fallback.
- Request **only** the weights/styles in use: 400, 500, italic 400, italic 500, with optical sizing. Nothing else. Audit this if the guide ever changes.
- If Google Fonts dependence ever chafes, the upgrade is self-hosting two woff2 files in `/assets/fonts/` with `@font-face` — a one-hour change, noted here so future-you knows it's available.

---

## 6. Accessibility baseline

Non-negotiable floor, checked before any deploy:

- All text/background pairs meet WCAG AA contrast. (The style guide palette passes: ink-soft on paper ≈ 9.8:1, muted on paper ≈ 4.6:1, accent on paper ≈ 6.4:1. If a token changes, re-check.)
- Keyboard: every interactive element reachable in a sensible order; visible `:focus-visible` outline (2px solid accent, offset 2px) — never `outline: none` without a replacement.
- Zoom to 200% breaks nothing (the rem-based measure handles this).
- Links make sense out of context — "See teaching →", never "click here".
- Run each page through a checker (axe DevTools or WAVE) once per structural change, not per content edit.

---

## 7. Performance & footprint

Targets, not aspirations — this architecture hits them by default:

- Each page **≤ 100 KB transferred** excluding fonts; fonts ≤ 60 KB.
- Zero JavaScript on the critical path. Zero third-party requests except Google Fonts. **No analytics** unless a specific question demands it, and then a privacy-respecting, script-light option (e.g., GoatCounter) — never Google Analytics.
- Images sized to their rendered dimensions, exported as JPEG/WebP for photos and SVG for anything line-based. Nothing over 200 KB without justification.
- Lighthouse ≥ 95 across the board is the smoke test; a score drop means something snuck in.

---

## 8. Content & data hygiene

- **Projects are the one growing dataset.** Each entry on `/projects/` is a self-contained block (title, one-line description, year, optional link) marked up identically. Keep entries in reverse-chronological order. When adding one, copy the previous block and edit — never restructure.
- The homepage shows **three or four** featured projects, chosen by hand. Featuring is an edit to `index.html`, deliberately manual: curation is the point.
- The CV exists in two forms with one source of truth: edit the HTML page at `/cv/`, then re-export `assets/files/cv.pdf` from it (print-to-PDF with a small `@media print` stylesheet is sufficient). Never let them drift; the README documents the export step.
- Dead links rot quietly — sweep annually with a link checker.

---

## 9. Future extensibility

Defined triggers, so growth is a decision rather than a drift:

- **If shared chrome duplication starts causing real mistakes** (nav edited on four pages, forgotten on the fifth), migrate to **Eleventy** with layouts and includes. It emits the exact same static HTML, deploys to Pages via a GitHub Action, and the CSS/HTML conventions above transfer unchanged. This is the sanctioned upgrade path; do not reach for a heavier framework.
- **If a blog/notes section is ever added,** it becomes `/notes/` with the same visual system, and is the moment the Eleventy migration pays for itself (markdown posts → templated pages). Design nothing for it until then.
- **If the SciComm portfolio is redesigned,** it stays in `/scicomm` under the same tenant contract — its look may change freely; its path may not, because inbound links will exist.
- Anything not covered here defaults to the governing principle: the smallest change that works, in plain HTML/CSS, with values drawn from the tokens.

---

## 10. Git & deployment

- Deploys from `main` via GitHub Pages; what is merged is what is live. No long-lived branches — small, complete commits directly to `main` are fine for a one-person site.
- Commit messages: imperative, specific — `Add Tidepool to projects`, `Fix nav wrap on mobile` — not `updates`.
- `.gitignore`: OS junk (`.DS_Store`, `Thumbs.db`) and editor droppings. Since there's no build, there are no artifacts to ignore.
- `404.html` at the root is served automatically by Pages for missing URLs; style it like every other page, with the nav, so a dead end is still a front door.
- After any structural change, verify on a real phone, not just a narrowed browser window.

---

## Checklist for every new page

1. Copy the `<head>` block from an existing page; update title, description, canonical.
2. `<header>` with wordmark + top-bar nav, `aria-current` on the right link.
3. One `<h1>`, content inside `<main id="main">`, footer with the standard links.
4. All values from tokens; no new colors, sizes, or fonts.
5. Check keyboard navigation and contrast; run Lighthouse.
6. Commit with a message that says what changed.
