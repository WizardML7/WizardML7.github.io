# THEMING.md — how to give a WizardML7 project page the site theme

This is the repeatable playbook for theming a `WizardML7` project repo so its GitHub Pages
site matches the landing page at https://wizardml7.github.io/ — dark surface, twinkling
starfield, live accent-color switcher, and matching typography/buttons.

**Audience:** a future Claude Code session (or Domenic) doing this from any machine.
The theme kit lives in [`theme-kit/`](theme-kit/) in this repo, so it's available anywhere
the repo is cloned. The kit's own reference is [`theme-kit/README.md`](theme-kit/README.md).

---

## The mental model (read this first)

- **One origin, one theme.** Every page on `https://wizardml7.github.io/*` shares the
  browser origin, so they share `localStorage`. The landing page and the kit both use the
  key **`wzml7_theme`** (values: `teal` `azure` `arcane` `emerald` `gold` `magenta`). A
  visitor's color choice therefore carries across all pages automatically — you don't wire
  anything up; just include the kit.
- **The landing page is separate.** The root `index.html` of *this* repo is a large
  (~985 KB) self-contained "bundler" page; its content is base64-encoded inside
  `<script type="__bundler/*">` blocks and unpacked at runtime into `#dc-root`. Treat that
  payload as opaque — only its outer `<head>` is normal HTML. It has its **own** built-in
  switcher (not the kit), but it speaks the same `wzml7_theme` protocol, so it stays in sync
  with kit pages for free. **You do not touch the landing page when theming a project repo.**
- **Each project repo is its own GitHub Pages project**, served from its own `main` branch.
  You clone it, edit it, and push it independently.

## Scope — which repos get themed

Themed and live: `Cuckoo-Watchtower`, `QtFuzzer`, `AudioCovertChannel`, `metadataAnalyzer`,
`ChatCSEC`.

**Do NOT theme `MalwareBenchmarkDataset`** — it's a former professor's research under
`byuan.github.io` (a different origin) and is intentionally not shown on this site.

The right set to theme is "whatever the landing page links to under `wizardml7.github.io/...`".
To re-derive it from the live landing page:
```bash
grep -oE 'wizardml7\.github\.io/[A-Za-z0-9_.-]+' index.html | sort -u
```
(`.pdf` links are hosted files, not project pages — skip those. `github.com/WizardML7/...`
links point at repos, not Pages sites — skip those too.)

## Deploy preference

Domenic prefers **pushing straight to `main`** (no PRs), replacing files in place (git
history is the backup). Still **preview each page in a browser before pushing** — that part
matters. Pages redeploys each repo in ~1–3 min.

---

## Step-by-step: theme a new repo `<REPO>`

### 1. Clone into a scratch workspace
```bash
WORK=/tmp/wzml7-theme && mkdir -p "$WORK"
git clone git@github.com:WizardML7/<REPO>.git "$WORK/<REPO>"
```

### 2. Read the existing page — preserve its content
`Read` `"$WORK/<REPO>/index.html"`. Capture: the title, the section headings/anchors, every
paragraph/list, and any assets (`<img src>`, code blocks). **You are restyling, not
rewriting** — keep all the real content. Note any images so you can confirm they exist:
```bash
ls "$WORK/<REPO>/"   # check that referenced images (e.g. example.png) are present
```

### 3. Copy the kit in
```bash
cp theme-kit/wizard-theme.css "$WORK/<REPO>/wizard-theme.css"
cp theme-kit/wizard-theme.js  "$WORK/<REPO>/wizard-theme.js"
```
(Run from this repo's root, where `theme-kit/` lives. If you're working elsewhere, copy the
two kit files from a fresh clone of `WizardML7.github.io`.)

### 4. Rewrite `index.html` using the standard structure
Match the existing themed pages exactly. Skeleton (fill in the project's real content):

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title><Project> — <tagline> · WizardML7</title>
    <meta name="description" content="<one-sentence description>">
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&family=Source+Code+Pro:wght@400;500&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="wizard-theme.css">
</head>
<body class="wz">
    <nav class="wz-nav">
        <a class="wz-nav-brand" href="https://wizardml7.github.io/"><span class="wz-dot"></span>WizardML7</a>
        <ul class="wz-nav-links">
            <!-- one <li><a href="#anchor">Label</a></li> per section, reusing the page's existing anchors -->
        </ul>
    </nav>

    <main class="wz-main">
        <header class="wz-hero">
            <div class="wz-eyebrow">// <CATEGORY · CATEGORY></div>
            <h1><Project></h1>
            <p class="wz-lede"><1–2 sentence summary, from the existing overview></p>
            <div class="wz-hero-actions">
                <a class="wz-btn" href="https://github.com/WizardML7/<REPO>" target="_blank" rel="noopener">View on GitHub</a>
                <a class="wz-btn-ghost" href="https://wizardml7.github.io/">← Back to portfolio</a>
            </div>
        </header>

        <section class="wz-prose">
            <!-- The page's real content. Map the OLD structure like this:
                 old <h2> title       -> becomes the <h1> in the hero (don't repeat it here)
                 old <h3 id="x">Sec</h3> -> <h2 id="x">Sec</h2>   (kit styles h2 as the accent mono eyebrow heading)
                 old <h4>Subsec</h4>     -> keep as <h4>
                 <p> / <ul> / <ol> / <pre><code> / <img> -> keep as-is, the kit themes them
                 inline code -> wrap in <code>…</code> -->
        </section>
    </main>

    <footer class="wz-footer">
        <a href="https://github.com/WizardML7">GitHub Profile</a> &nbsp;•&nbsp;
        <a href="https://linkedin.com/in/domenic-loiacono" target="_blank" rel="noopener">LinkedIn</a> &nbsp;•&nbsp;
        <a href="https://wizardml7.github.io/">Portfolio</a>
    </footer>

    <script src="wizard-theme.js" defer></script>
</body>
</html>
```

Heading-level mapping is the one non-obvious bit: the kit renders `.wz-prose h2` as the
small uppercase **accent mono "eyebrow" heading** and `.wz-prose h3` as the large white
section title. The old pages used `<h3 id="...">` for section titles, so those become
`<h2 id="...">` here (keeping the same `id` anchors the nav links point at).

### 5. Preview in a browser before pushing (required)
The Playwright MCP server blocks `file://`, so serve over HTTP:
```bash
( cd "$WORK/<REPO>" && python3 -m http.server 8750 --bind 127.0.0.1 >/tmp/wz.log 2>&1 & )
```
Then navigate to `http://127.0.0.1:8750/index.html` and:
- Full-page screenshot — confirm hero, sections, footer, and any images render.
- Confirm the kit attached: `#wz-switcher`, `#wz-stars`, and 6 `.wz-sw` swatches exist.
- **Check button legibility on the `gold` theme** (worst case). In the page console:
  `window.setWizardTheme('gold')` then verify `.wz-btn` text is near-black, not the fill color.
- The only acceptable console error is `favicon.ico 404` from the local server.

Kill the server (`pkill -f "http.server 8750"`) and delete any screenshots when done.

### 6. Commit + push to main
```bash
git -C "$WORK/<REPO>" add index.html wizard-theme.css wizard-theme.js
git -C "$WORK/<REPO>" commit -m "Apply WizardML7 dark theme to project page"
git -C "$WORK/<REPO>" push origin HEAD:main
```

### 7. Verify live (Pages takes ~1–3 min)
```bash
curl -s "https://wizardml7.github.io/<REPO>/wizard-theme.css?cb=$RANDOM" | grep -q 'wz-nav' && echo LIVE
```

---

## Editing the theme itself (colors, button styles, etc.)

The source of truth is **`theme-kit/wizard-theme.css`** and **`theme-kit/wizard-theme.js`**.
After editing either, **re-copy it into every themed repo** and push each (the per-repo
copies are independent files — there is no shared include). Then update this repo's
`theme-kit/` copy too so it stays canonical.

### Known gotcha: button text contrast
`.wz-btn` / `.wz-btn-ghost` are usually `<a>` elements. The dark baseline has
`body.wz a { color: var(--accent) }`, whose specificity (0,1,1) beats `.wz-btn` (0,1,0) — so
button text would render the same color as the accent fill and be invisible until hover.
The kit fixes this with higher-specificity rules:
```css
body.wz a.wz-btn, body.wz a.wz-btn:hover { color: #0D0D0D; }
body.wz a.wz-btn-ghost, body.wz a.wz-btn-ghost:hover { color: var(--wz-fg); }
```
Keep these. If you add new button variants, give them the same `body.wz a.<class>` override.

## Adding a new project to the landing page
The landing page is the opaque bundle described above — its project cards are baked into the
encoded payload, so you can't hand-edit them here. To add/remove a project card, regenerate
the landing-page bundle in the design tool that produced it and replace the root
`index.html`. Theming the new project's *own* repo page (this playbook) is independent of
that and can be done anytime.
