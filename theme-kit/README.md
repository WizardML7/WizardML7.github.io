# WizardML7 — theme kit

The portable dark theme, color switcher, and starfield shared by every page on
**wizardml7.github.io**. Drop it into a project repo to make that project's page
match the landing page.

> Looking for the full step-by-step process to theme a new repo (clone → restyle →
> verify → push)? See **[`/THEMING.md`](../THEMING.md)** at the repo root. This file is
> just the kit reference.

## What's in this folder

| File | What it is |
|---|---|
| `wizard-theme.css` | Design tokens, dark baseline, helper classes (`.wz-btn`, `.wz-eyebrow`, …) **and** the project-page layout classes (`.wz-nav`, `.wz-main`, `.wz-hero`, `.wz-prose`, `.wz-footer`). This is the source of truth — re-copy it to each repo after editing. |
| `wizard-theme.js` | Drop-in theme engine: injects the color switcher, the twinkling starfield, and the live `--accent` CSS variables. Persists the choice to `localStorage`. |
| `demo.html` | A minimal worked example built from the kit. |

## How the shared theme works (important)

Every page under `https://wizardml7.github.io/*` is the **same browser origin**, so they
share `localStorage`. Both the landing page and this kit read/write the same key
(`wzml7_theme`). Result: a visitor who picks **Spellbound Violet** on the homepage sees
violet on every project page too — automatically, no extra wiring.
(`byuan.github.io/...` is a different origin, so it's correctly left untouched.)

## Integration (the short version)

1. Copy `wizard-theme.css` + `wizard-theme.js` into the repo (root is fine).
2. In that repo's `index.html`:
   - In `<head>`: `<link rel="stylesheet" href="wizard-theme.css">`
   - Add `class="wz"` to `<body>` for the dark baseline.
   - Just before `</body>`: `<script src="wizard-theme.js" defer></script>`
3. Build the page with the layout classes (see the structure below) and style anything
   custom with the tokens — `var(--accent)`, `var(--wz-card)`, `.wz-*` helpers.

The switcher + starfield appear automatically; the accent recolors everything that
references `var(--accent)`.

### Page structure used by the live project pages
```
<nav class="wz-nav"> brand + section links </nav>
<main class="wz-main">
  <header class="wz-hero"> .wz-eyebrow · h1 · .wz-lede · .wz-hero-actions (.wz-btn / .wz-btn-ghost) </header>
  <section class="wz-prose"> h2/h3/h4, ul/ol, p, code, pre, img — all themed </section>
</main>
<footer class="wz-footer"> profile links </footer>
<script src="wizard-theme.js" defer></script>
```

### Buttons
`.wz-btn` (filled accent) and `.wz-btn-ghost` (outline). Button text is forced near-black
via `body.wz a.wz-btn` so it stays legible on every palette (the generic `body.wz a`
accent-color rule would otherwise win by specificity and hide the text). Don't remove that
rule.

### Don't want the starfield or switcher on a given page?
Set options *before* the script tag:
```html
<script>window.WizardTheme = { stars: false, switcher: true, default: 'teal' };</script>
<script src="wizard-theme.js" defer></script>
```

### Palette
`teal` (default) · `azure` · `arcane` (violet) · `emerald` · `gold` · `magenta`.
Edit the `THEMES` map at the top of `wizard-theme.js` to add/tweak colors — change it
once and re-copy to each repo.

## Already themed & live (each on its repo's `main`)
- https://wizardml7.github.io/Cuckoo-Watchtower/
- https://wizardml7.github.io/QtFuzzer/
- https://wizardml7.github.io/AudioCovertChannel/
- https://wizardml7.github.io/metadataAnalyzer/
- https://wizardml7.github.io/ChatCSEC/

Intentionally **not** themed: `MalwareBenchmarkDataset` (owned by a former professor under
`byuan.github.io`, not shown on this site).
