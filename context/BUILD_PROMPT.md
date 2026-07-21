# Build prompt for Claude Code

## How to use this

Place this file, `styleguide.html`, and `WEB_SPEC.md` together in a folder called `context/` at the root of your project directory. Then run:

```
claude
```

And paste or reference this prompt. Claude Code will read the two reference documents and build the full site from scratch.

---

## Prompt (copy everything below this line)

---

You are building a personal website from scratch according to two reference documents already in the `context/` directory: `context/WEB_SPEC.md` (architecture and code conventions) and `context/styleguide.html` (visual system). Read both documents in full before writing a single file. Everything you build must conform to them. Where this prompt and the documents agree, follow them. Where this prompt adds detail the documents don't cover, apply it. Do not invent conventions not present in either.

### What you are building

Six pages plus shared assets, deployed as a static GitHub Pages site — no framework, no build step, no npm.

```
/
├── index.html
├── about/index.html
├── projects/index.html
├── teaching/index.html
├── cv/index.html
├── 404.html
├── assets/
│   ├── css/main.css
│   ├── js/          (create the folder, leave it empty)
│   ├── img/         (create the folder, leave it empty)
│   └── files/       (create the folder, leave it empty)
├── context/
│   ├── BUILD_PROMPT.md
│   ├── WEB_SPEC.md
│   └── styleguide.html
├── .editorconfig
└── README.md
```

Do not create the `/scicomm/` folder — it will be dropped in separately later.

---

### Order of operations

Build in this order. Do not skip ahead.

1. `.editorconfig`
2. `assets/css/main.css`
3. `index.html`
4. `about/index.html`
5. `projects/index.html`
6. `teaching/index.html`
7. `cv/index.html`
8. `404.html`
9. `README.md`

---

### Step 1 — .editorconfig

```ini
root = true

[*]
indent_style = space
indent_size = 2
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
```

---

### Step 2 — assets/css/main.css

This is the only stylesheet. Every visual decision lives here. Build it in this exact top-to-bottom order with section comments as shown:

```
/* ---- tokens ---- */
/* ---- reset ---- */
/* ---- base ---- */
/* ---- layout ---- */
/* ---- nav ---- */
/* ---- components ---- */
/* ---- utilities ---- */
/* ---- media queries ---- */
```

**Tokens — use exactly these values, no others:**

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

No raw hex values or font names may appear below `:root`. Every color is a var(), every font is a var().

**Reset:** box-sizing border-box on *, zero margin/padding on body, nothing else. Do not import a reset library.

**Base element styles to include:**
- `body`: background var(--paper), color var(--ink-soft), font-family var(--serif), font-size 18px, line-height 1.7, -webkit-font-smoothing antialiased, font-optical-sizing auto
- `h1`: font-size 36px, font-weight 500, color var(--ink), line-height 1.15, letter-spacing -0.005em, margin 0
- `h2`: font-size 24px, font-weight 500, color var(--ink), line-height 1.2, margin 0
- `h3`: font-size 17px, font-weight 500, color var(--ink), margin 0
- `p`: margin 14px 0 0
- `a`: color var(--accent), text-decoration none
- `a:hover`: text-decoration underline, text-underline-offset 3px, text-decoration-thickness 1px
- `a:focus-visible`: outline 2px solid var(--accent), outline-offset 2px
- `img`: max-width 100%, height auto, display block

**Layout:**
- `.wrap`: max-width var(--measure), margin 0 auto, padding 64px 24px 120px

**Skip link:**
```css
.skip-link {
  position: absolute;
  top: -100%;
  left: 24px;
}
.skip-link:focus {
  top: 8px;
}
```

**Site header (interior pages):**
- `.site-header`: border-bottom 0.5px solid var(--hairline), padding 14px 0, margin-bottom 48px
- `.site-header .wrap`: padding-top 0, padding-bottom 0 — override the vertical padding here only
- `.header-inner`: display flex, justify-content space-between, align-items baseline

**Wordmark:**
- `.wordmark`: font-size 16px, font-weight 500, color var(--ink), text-decoration none
- `.wordmark:hover`: text-decoration none (the wordmark never underlines)

**Nav:**
- `.site-nav ul`: list-style none, margin 0, padding 0, display flex, flex-wrap wrap, gap 18px
- `.site-nav a`: font-size 14px, color var(--accent), text-decoration none
- `.site-nav a:hover`: text-decoration underline, text-underline-offset 3px, text-decoration-thickness 1px
- `.site-nav a[aria-current="page"]`: color var(--ink), pointer-events none, text-decoration none (current page is ink-colored and not a live link)

**Homepage nav (placed below the intro, not in a header bar):**
- `.home-nav ul`: same as .site-nav ul
- `.home-nav a`: same as .site-nav a

**Components:**

Section label:
```css
.section-label {
  font-size: 14px;
  font-style: italic;
  color: var(--faint);
  margin: 38px 0 14px;
}
```

Section divider (between homepage teaser sections — not after the last one):
```css
.section-divider {
  border: none;
  border-top: 0.5px solid var(--hairline);
  margin: 38px 0 0;
}
```

Project row:
```css
.project-row {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  gap: 16px;
  padding: 7px 0;
}
.project-title {
  font-size: 17px;
  font-weight: 500;
  color: var(--ink);
}
.project-desc {
  font-size: 17px;
  color: var(--muted);
}
.project-year {
  font-size: 14px;
  color: var(--faint);
  font-family: var(--mono);
  white-space: nowrap;
}
```

Teaser block (teaching, scicomm):
```css
.teaser p {
  margin: 0;
  font-size: 17px;
  color: var(--ink-soft);
}
```

More link (→ links):
```css
.more {
  font-size: 15px;
  color: var(--accent);
  margin-top: 10px;
  display: block;
}
.more:hover {
  text-decoration: underline;
  text-underline-offset: 3px;
  text-decoration-thickness: 1px;
}
```

Footer:
```css
.site-footer {
  border-top: 0.5px solid var(--hairline);
  margin-top: 64px;
  padding-top: 20px;
}
.site-footer ul {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  flex-wrap: wrap;
  gap: 18px;
}
.site-footer a {
  font-size: 14px;
  color: var(--accent);
}
```

CV page — basic definition list styling:
```css
.cv-section {
  margin-top: 40px;
}
.cv-section h2 {
  border-bottom: 0.5px solid var(--hairline);
  padding-bottom: 8px;
  margin-bottom: 20px;
}
.cv-entry {
  display: grid;
  grid-template-columns: 6rem 1fr;
  gap: 0 20px;
  margin-bottom: 14px;
  font-size: 17px;
}
.cv-year {
  color: var(--faint);
  font-family: var(--mono);
  font-size: 14px;
  padding-top: 2px;
}
.cv-detail p {
  margin: 4px 0 0;
  font-size: 15px;
  color: var(--muted);
}
```

**Utilities:**
```css
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0,0,0,0);
  white-space: nowrap;
  border: 0;
}
```

**Media queries:**
```css
@media (max-width: 600px) {
  .wrap {
    padding: 44px 20px 80px;
  }
  h1 {
    font-size: 30px;
  }
  .cv-entry {
    grid-template-columns: 1fr;
    gap: 2px;
  }
}

@media (prefers-reduced-motion: reduce) {
  * {
    transition: none !important;
    animation: none !important;
  }
}
```

---

### The shared `<head>` block

Every page uses this structure. Update title, description, and canonical per page:

```html
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title><!-- Page — Your Name, or just Your Name on the homepage --></title>
  <meta name="description" content="<!-- Unique one-sentence description -->">
  <link rel="canonical" href="<!-- https://yourdomain.com/page/ -->">
  <meta property="og:title" content="<!-- Same as title -->">
  <meta property="og:description" content="<!-- Same as meta description -->">
  <meta property="og:url" content="<!-- Same as canonical -->">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Newsreader:ital,opsz,wght@0,6..72,400;0,6..72,500;1,6..72,400;1,6..72,500&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="/assets/css/main.css">
</head>
```

Use root-relative paths for the stylesheet (`/assets/css/main.css`). If deploying to a GitHub Pages project site at a subpath (e.g. `username.github.io/repo/`), this needs adjusting — leave a TODO comment noting this.

---

### Step 3 — index.html (homepage)

The homepage layout is: skip link → `<main>` containing name as `<h1>`, intro paragraph, homepage nav below it, then teaser sections, then footer. **There is no `<header>` element on the homepage.** The `<header>` with wordmark and top-bar nav only appears on interior pages.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <!-- shared head block, title = "Your Name", canonical = domain root -->
</head>
<body>
  <a href="#main" class="skip-link">Skip to content</a>
  <main id="main">
    <div class="wrap">

      <h1><!-- CONTENT NEEDED: full name --></h1>
      <p><!-- CONTENT NEEDED: intro paragraph. Placeholder: "I'm a researcher and educator working where science meets public understanding. I build tools, teach, and write about making complex ideas legible." --></p>

      <nav class="home-nav" aria-label="Main">
        <ul>
          <li><a href="/about/">About</a></li>
          <li><a href="/projects/">Projects</a></li>
          <li><a href="/teaching/">Teaching</a></li>
          <li><a href="/scicomm/">SciComm</a></li>
          <li><a href="/cv/">CV</a></li>
        </ul>
      </nav>

      <p class="section-label">Selected work</p>
      <!-- Three to four project rows. Placeholder entries: -->
      <div class="project-row">
        <div>
          <span class="project-title"><!-- CONTENT NEEDED: project title --></span><span class="project-desc"> — <!-- CONTENT NEEDED: one-line description --></span>
        </div>
        <span class="project-year"><!-- YYYY --></span>
      </div>
      <!-- repeat for 2–3 more projects -->
      <a href="/projects/" class="more">See all projects →</a>

      <hr class="section-divider">

      <p class="section-label">Teaching</p>
      <div class="teaser">
        <p><!-- CONTENT NEEDED: one to two lines. Placeholder: "Five years teaching undergraduate science writing and quantitative reasoning. Courses, materials, and student work." --></p>
      </div>
      <a href="/teaching/" class="more">See teaching →</a>

      <hr class="section-divider">

      <p class="section-label">Science communication</p>
      <div class="teaser">
        <p><!-- CONTENT NEEDED: one to two lines. Placeholder: "A separate home for my public-facing work — essays, visualizations, and talks for non-specialist audiences." --></p>
      </div>
      <a href="/scicomm/" class="more">Visit the portfolio →</a>

    </div>
  </main>

  <footer class="site-footer">
    <div class="wrap" style="padding-top:0;padding-bottom:40px">
      <ul>
        <li><a href="mailto:<!-- CONTENT NEEDED: email -->">Email</a></li>
        <li><a href="<!-- CONTENT NEEDED: GitHub URL -->">GitHub</a></li>
        <li><a href="<!-- CONTENT NEEDED: Bluesky or other URL -->">Bluesky</a></li>
        <li><a href="/assets/files/cv.pdf">CV (PDF)</a></li>
      </ul>
    </div>
  </footer>

</body>
</html>
```

---

### Step 4 — about/index.html

Uses the interior page shell (header with wordmark + top-bar nav). `<h1>About</h1>`, then two or three short first-person paragraphs: background, current focus, and a pointer to the CV and footer links for the fuller history and contact. This is prose, not a list — no `.cv-entry` blocks here. Use placeholder paragraphs with a `<!-- CONTENT NEEDED -->` comment above them. Mark this page's nav link with `aria-current="page"`.

---

### Step 5 — projects/index.html

Uses the interior page shell (header with wordmark + top-bar nav). Full project list, newest first. Mark this page's nav link with `aria-current="page"`.

Include a minimum of four placeholder project entries using the `.project-row` pattern. Each entry needs a title, one-line description, year, and an optional link on the title. Add a `<!-- CONTENT NEEDED -->` comment above the project list reminding that these are placeholders.

---

### Step 6 — teaching/index.html

Interior page shell. `<h1>Teaching</h1>`, then a short introductory paragraph, then a structured list of courses or roles as `.cv-entry` blocks (year in left column, title + institution + brief description in right column). Use placeholder entries with `<!-- CONTENT NEEDED -->` comments. Mark nav link with `aria-current="page"`.

---

### Step 7 — cv/index.html

Interior page shell. The page title is `<h1>CV</h1>` followed by a prominent download link:

```html
<p><a href="/assets/files/cv.pdf">Download PDF ↓</a></p>
```

Then CV sections using `.cv-section` and `.cv-entry` blocks: Education, Positions (or Experience), Publications (or Projects), Teaching, and any other relevant sections. Use placeholder entries. Add a comment at the top of the CV content:

```html
<!-- CONTENT NEEDED: replace all placeholder entries below with real CV content. -->
<!-- When content is updated here, re-export assets/files/cv.pdf via print-to-PDF. -->
```

Mark nav link with `aria-current="page"`.

---

### Step 8 — 404.html

Uses the interior page shell. Simple, warm, still useful:

```html
<h1>Page not found</h1>
<p>That URL doesn't exist. Try the <a href="/">homepage</a>.</p>
```

No `aria-current` — this page is not in the nav. The nav still appears so a visitor can navigate anywhere.

---

### Step 9 — README.md

Keep it to the essentials a returning maintainer needs:

```markdown
# [Your name] — personal site

Static site deployed via GitHub Pages.

## Editing content

- **Homepage intro:** `index.html`
- **About:** `about/index.html`
- **Projects:** `projects/index.html` and the featured rows in `index.html`
- **Teaching:** `teaching/index.html`
- **CV:** edit `cv/index.html`, then re-export `assets/files/cv.pdf` via print-to-PDF
- **SciComm portfolio:** `/scicomm/` (maintained separately)

## Design reference

- Visual system: `context/styleguide.html` (open in a browser)
- Code conventions: `context/WEB_SPEC.md`
- Original build prompt: `context/BUILD_PROMPT.md`

## Deploying

Push to `main`. GitHub Pages builds automatically.
No build step, no dependencies.
```

---

### Shared interior page shell

Every page except `index.html` and `404.html` uses this outer wrapper. The wordmark links to `/`. The nav link for the current page gets `aria-current="page"`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <!-- shared head block -->
</head>
<body>
  <a href="#main" class="skip-link">Skip to content</a>

  <header class="site-header">
    <div class="wrap">
      <div class="header-inner">
        <a href="/" class="wordmark"><!-- Your name --></a>
        <nav class="site-nav" aria-label="Main">
          <ul>
            <li><a href="/about/" <!-- aria-current="page" if on about -->>About</a></li>
            <li><a href="/projects/" <!-- aria-current="page" if on projects -->>Projects</a></li>
            <li><a href="/teaching/" <!-- aria-current="page" if on teaching -->>Teaching</a></li>
            <li><a href="/scicomm/">SciComm</a></li>
            <li><a href="/cv/" <!-- aria-current="page" if on cv -->>CV</a></li>
          </ul>
        </nav>
      </div>
    </div>
  </header>

  <main id="main">
    <div class="wrap">
      <!-- page content -->
    </div>
  </main>

  <footer class="site-footer">
    <div class="wrap" style="padding-top:0;padding-bottom:40px">
      <ul>
        <li><a href="mailto:<!-- email -->">Email</a></li>
        <li><a href="<!-- GitHub URL -->">GitHub</a></li>
        <li><a href="<!-- Bluesky URL -->">Bluesky</a></li>
        <li><a href="/assets/files/cv.pdf">CV (PDF)</a></li>
      </ul>
    </div>
  </footer>

</body>
</html>
```

---

### Hard constraints

These are non-negotiable. Check each before finishing:

- No framework, no bundler, no npm, no node_modules.
- No `<link>` or `<script>` tags for any library.
- No inline `style="..."` attributes on any element except where explicitly shown in this prompt (the footer wrap padding override). All styling is in `main.css`.
- No raw hex values in CSS below `:root`. No raw font family names below `:root`.
- No `<div>` where `<header>`, `<main>`, `<footer>`, `<nav>`, `<ul>`, or `<h1>`–`<h3>` is correct.
- No `outline: none` without a replacement focus style.
- Exactly one `<h1>` per page.
- All images (if any are added later) need `width`, `height`, and `alt` attributes.
- Filenames: lowercase, hyphenated, no spaces.

---

### Verification checklist

After building every file, check each of these:

- [ ] Validate `index.html`, `about/index.html`, `projects/index.html`, `teaching/index.html`, `cv/index.html`, and `404.html` with the W3C HTML validator (or `vnu` locally). Zero errors, zero warnings.
- [ ] Open each page in a browser. Confirm Newsreader loads and the cream paper color renders. Confirm the layout is single-column and centered.
- [ ] Click every internal navigation link. Confirm the current page's nav link is ink-colored and non-clickable.
- [ ] Confirm the homepage has no `<header>` element and nav appears below the intro paragraph.
- [ ] Confirm every interior page has wordmark + top-bar nav.
- [ ] Tab through the homepage. Confirm the skip link appears on first Tab press. Confirm all nav links are reachable and visibly focused.
- [ ] Narrow the browser to 375px. Confirm nothing overflows, nav wraps cleanly, and the layout is readable.
- [ ] Check that every `<!-- CONTENT NEEDED -->` comment is present and nothing has been silently omitted.
- [ ] Confirm `assets/css/main.css` has no raw hex values outside `:root`.

---

### Content placeholders

Wherever real content is missing, use the placeholder text shown in this prompt and leave a `<!-- CONTENT NEEDED: [description] -->` comment immediately before it. Do not invent specific project titles, real names, real email addresses, or real URLs — leave those as obvious placeholders. The site should build and render correctly with placeholders in place; the owner will replace them.
