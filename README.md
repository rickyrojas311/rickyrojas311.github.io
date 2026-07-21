# Ricky Rojas — personal site

Static site deployed via GitHub Pages at [rickyrojas311.github.io](https://rickyrojas311.github.io/).

## Editing content

- **Homepage intro:** `index.html`
- **About:** `about/index.html`
- **Projects:** `projects/index.html` and the featured rows in `index.html`. Each project also has its own landing page at `projects/<slug>/index.html` (short description + link to GitHub; add a demo there once one exists).
- **Teaching:** `teaching/index.html`
- **CV:** edit `cv/index.html`, then re-export `assets/files/cv.pdf` via print-to-PDF
- **SciComm:** `scicomm/index.html` is a short landing page that links out to the portfolio, hosted separately at [rickyrojas311.github.io/nsc-portfolio](https://rickyrojas311.github.io/nsc-portfolio/). The rest of `/scicomm/` (artifacts, images, PDFs) is legacy content from before the portfolio moved to its own repo — safe to delete once you've confirmed everything is live at `nsc-portfolio`.
- **Favicon:** `assets/img/favicon.svg` — same mark as the SciComm portfolio's, in the site accent red (`#8C1515`) instead of purple.

## Design reference

- Visual system: `context/styleguide.html` (open in a browser)
- Code conventions: `context/WEB_SPEC.md`
- Original build prompt: `context/BUILD_PROMPT.md`

## Hosting on GitHub Pages

This repo isn't connected to git yet. To get it live at `rickyrojas311.github.io`:

1. **Create the repo on GitHub.** It must be named exactly `rickyrojas311.github.io` (that exact name is what makes it a user site served at the domain root instead of a `/repo-name/` subpath). Create it empty — no README, no `.gitignore`.
2. **Push this folder to it:**
   ```sh
   git init
   git add .
   git commit -m "Initial site"
   git branch -M main
   git remote add origin https://github.com/rickyrojas311/rickyrojas311.github.io.git
   git push -u origin main
   ```
3. **Enable Pages.** In the repo, go to Settings → Pages. Under "Build and deployment," set Source to "Deploy from a branch," branch `main`, folder `/ (root)`. Save.
4. **Wait a minute, then visit** `https://rickyrojas311.github.io/`. GitHub shows a green checkmark under Settings → Pages once the build finishes.
5. Every future `git push` to `main` redeploys automatically — no other steps.

For the SciComm portfolio, repeat the same process in its own repo named `nsc-portfolio` (a project-site repo, not a user-site one — any name works for Pages, but it must be `nsc-portfolio` to land at `/nsc-portfolio/`). Same Settings → Pages steps there.

## Deploying

Push to `main`. GitHub Pages builds automatically.
No build step, no dependencies.
