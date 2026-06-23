# WizardML7 — site deploy & theme kit

Everything to (1) put the new landing page live on **wizardml7.github.io**, and
(2) give your other project pages (Cuckoo-Watchtower, QtFuzzer, AudioCovert, …)
the same dark theme, color switcher, and starfield.

## What's in this folder

| File | What it is |
|---|---|
| `index.html` | The **landing page**, fully self-contained (one file, works offline). Drop it at the root of your `WizardML7.github.io` repo. |
| `wizard-theme.js` | Drop-in theme engine for your *other* repos: injects the color switcher, the twinkling starfield, and the `--accent` CSS variables. Persists the choice. |
| `wizard-theme.css` | Design tokens + a light baseline + helper classes (`.wz-btn`, `.wz-eyebrow`, `.wz-glow`, …). Pair with the JS. |
| `demo.html` | A worked example: a themed "Cuckoo Watchtower" page built from `wizard-theme.css` + `wizard-theme.js`. Use it as the pattern for every project page. |

## How the shared theme works (important)

Every page under `https://wizardml7.github.io/*` is the **same browser origin**, so they
share `localStorage`. Both the landing page and the kit read/write the same key
(`wzml7_theme`). Result: a visitor who picks **Spellbound Violet** on your homepage
sees violet on Cuckoo-Watchtower too — automatically, no extra wiring.
(`byuan.github.io/...` is a different origin, so it's correctly left untouched.)

---

## Part 1 — Deploy the landing page

Your `WizardML7.github.io` repo serves its root `index.html` as the site.

1. Copy `index.html` from this folder to the **root** of the repo (replace the old one).
2. Commit & push to the default branch (`main`). GitHub Pages redeploys in ~1 min.
3. Visit https://wizardml7.github.io/ — done.

If you keep your old page, name the new one `home.html` instead and link to it — but
root `index.html` is what GitHub Pages serves by default.

> The landing page is one self-contained file (HTML + JS + the icon, ~940 KB). It pulls
> Google Fonts, Lucide, and tsParticles from their CDNs at runtime, so it needs a network
> connection like any normal web page.

## Part 2 — Theme your other project repos

For each repo you control (e.g. `Cuckoo-Watchtower`):

1. Copy **`wizard-theme.js`** and **`wizard-theme.css`** into the repo (root is fine).
2. In that repo's `index.html`:
   - In `<head>`: `<link rel="stylesheet" href="wizard-theme.css">`
   - Add `class="wz"` to `<body>` for the dark baseline (optional but recommended).
   - Just before `</body>`: `<script src="wizard-theme.js" defer></script>`
3. Style the page's own elements with the tokens — `var(--accent)`, the `.wz-*`
   helper classes, `var(--wz-card)`, etc. See `demo.html` for the full pattern.

That's the whole integration. The switcher + starfield appear automatically; the
accent recolors everything that references `var(--accent)`.

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

---

## Paste-into-Claude-Code prompt

You said Claude Code is ready in the `WizardML7.github.io` repo. Paste this:

> I have a folder of design handoff files (`index.html`, `wizard-theme.js`,
> `wizard-theme.css`, `demo.html`). 
> 1) Replace this repo's root `index.html` with the provided `index.html` and commit.
> 2) Then, for my other project repos that I control (Cuckoo-Watchtower, QtFuzzer,
>    AudioCovertChannel, metadataAnalyzer, ChatCSEC — NOT byuan.github.io), copy
>    `wizard-theme.js` + `wizard-theme.css` into each, link them in `index.html`
>    (`<link>` in head, `<script defer>` before `</body>`, `class="wz"` on body), and
>    restyle each page's existing content to use the theme tokens (`var(--accent)`,
>    `.wz-btn`, `.wz-eyebrow`, `var(--wz-card)`), following `demo.html` as the pattern.
>    Keep each page's actual content; only restyle. Open a PR per repo.

(For repos other than this one, point Claude Code at each repo or open them locally —
each is its own GitHub Pages project.)
