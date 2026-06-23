# CLAUDE.md — WizardML7.github.io

Domenic's personal portfolio site (GitHub Pages, served from `main`).

## Layout
- `index.html` — the landing page. It's a large (~985 KB) self-contained **bundler** page:
  content is base64-encoded inside `<script type="__bundler/*">` blocks and unpacked at
  runtime into `#dc-root`. Treat the payload as opaque — only the outer `<head>` is editable
  as normal HTML. Don't try to hand-edit its content or its theme switcher.
- `theme-kit/` — the portable dark theme + color switcher + starfield kit shared by all the
  project pages. `wizard-theme.css` + `wizard-theme.js` are the source of truth.
- `about.html`, `services.html`, `style.css` — older standalone pages (not part of the bundle).

## Theming a project repo
The five project pages (`Cuckoo-Watchtower`, `QtFuzzer`, `AudioCovertChannel`,
`metadataAnalyzer`, `ChatCSEC`) are themed to match the landing page using the kit. To theme
a new repo, or to edit the theme, **follow [`THEMING.md`](THEMING.md)** — it has the full
clone → restyle → preview → push process and the known gotchas (heading-level mapping, button
contrast). Do **not** theme `MalwareBenchmarkDataset` (it's a former professor's repo under
`byuan.github.io`).

All pages share the `localStorage` key `wzml7_theme`, so a visitor's accent-color choice
carries across every page on the `wizardml7.github.io` origin.

## Working conventions
- Deploy by pushing straight to `main` (no PRs); replace files in place — git history is the
  backup. **But always preview rendered pages in a browser before pushing.**
- Each project page lives in its own GitHub Pages repo, cloned/edited/pushed independently.
- Don't commit: `.claude/`, `*.zip` download artifacts, or build/scratch files.
