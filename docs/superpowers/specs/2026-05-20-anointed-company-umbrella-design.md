# anointed.company — Umbrella Landing Page

**Date:** 2026-05-20
**Owner:** Alexander Fields
**Status:** Design — pending user review

## Purpose

A single static page at `anointed.company` that serves as the public umbrella for two ventures:

- **Anointed Attire & Apparel** (`anointedattireapparel.com`) — clothing brand
- **Anointed Automation** (`anointedautomation.net`) — software consulting

No backend. No CMS. No build step. Just an `index.html` and static assets.

## Goals

1. Communicate "Anointed" as a parent brand with two distinct ventures.
2. Direct every visitor to either `anointedattireapparel.com` or `anointedautomation.net`.
3. Look polished enough to inspire confidence in either venture without overshadowing them.

## Non-Goals

- No contact form, mailing list, or about page on this domain.
- No analytics in v1.
- No third venture or expandable grid — just the two.
- No SEO content marketing (the child sites carry their own SEO).

## Visual Design

Approved through v6 of the brainstorming mockups. Key elements:

- **Palette:** Black/near-black background (`radial-gradient(ellipse at top, #1a1a1f, #0a0a0d)`); gold accent (`#c9a96e`); cream text (`#f4e9d2` / `#a89878`).
- **Typography:** Georgia serif for the wordmark "Anointed" (60px, light weight, slightly tightened letter-spacing). System sans-serif for body and labels.
- **Layout:** Centered "Anointed" wordmark + thin gold rule, then two side-by-side venture cards.
- **Venture card (×2):** Logo at top (140px max height), small letter-spaced category label in gold ("CLOTHING" / "SOFTWARE"), serif venture name, one-line description, outlined gold link button, domain text below.
- **Responsive:** Cards stack on narrow viewports (<700px).

### Final copy

| Element | Text |
|---|---|
| Page wordmark | `Anointed` |
| Card 1 label | `CLOTHING` |
| Card 1 name | `Attire & Apparel` |
| Card 1 description | `Faith-rooted streetwear, made to order. Hats, hoodies, tees.` |
| Card 1 button | `VISIT SHOP →` |
| Card 1 domain | `anointedattireapparel.com` |
| Card 2 label | `SOFTWARE` |
| Card 2 name | `Automation` |
| Card 2 description | `Custom integrations, automations, and development consulting for small businesses.` |
| Card 2 button | `VISIT SITE →` |
| Card 2 domain | `anointedautomation.net` |

### Assets

- `attire-mark.png` — sourced from `C:\Users\Roku6\OneDrive\Pictures\apple-touch-icon.png` (THUG praying-hands graphic on black square).
- `automation-mark.png` — sourced from `/home/roku674/Anointed Automation LLC/Website/Frontend/public/favicon.png` (gold cross).
- Favicon: re-use `automation-mark.png` via `<link rel="icon" type="image/png" href="automation-mark.png">` — no separate `.ico` file (saves an image-conversion step).

Marks intentionally remain at different on-screen sizes — they're different graphic types (illustration vs. icon) and forcing equal size distorts the cross.

## Architecture

Static files at the repo root. No build tooling. Edits are pushed directly to `main`.

```
anointed-company/
├── index.html          # the page (uses automation-mark.png as <link rel="icon">)
├── attire-mark.png     # 1024×1024 graphic, on-black
├── automation-mark.png # gold cross icon — also serves as favicon
├── CNAME               # contains: anointed.company
├── 404.html            # minimal styled 404 → links back to /
├── robots.txt          # allow all
└── README.md           # one-paragraph: what this repo is, how to edit
```

### Why static / no framework

- The page changes maybe once a quarter (when copy or links change).
- A framework adds a build step, a node_modules folder, and version-pinning headaches for ~50 lines of HTML.
- A static page on GitHub Pages serves in <100ms globally and never breaks.

## Hosting & Deployment

- **Repo:** `github.com/AnointedAutomation/anointed-company` (public).
- **Host:** GitHub Pages, served from `main` branch root.
- **Custom domain:** `anointed.company` via `CNAME` file + DNS at registrar.
- **HTTPS:** GitHub-provisioned Let's Encrypt cert (auto-renewed).
- **Deploy cycle:** `git push` → live in ~30 seconds.

### DNS records the user adds at the registrar

Apex `anointed.company` → four A records:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

`www.anointed.company` → CNAME:

```
anointedautomation.github.io
```

**Registrar: Squarespace Domains.** DNS managed at Squarespace → Settings → Domains → `anointed.company` → DNS Settings → Custom Records. Squarespace's default A records and any auto-configured `www` CNAME will need to be removed / replaced with the GitHub Pages records above.

## What I Will Do With the PAT

Authenticated as `roku674` (admin on AnointedAutomation org):

1. `gh repo create AnointedAutomation/anointed-company --public --clone --description "Umbrella landing page for anointed.company"` into `/home/roku674/Anointed Automation LLC/Anointed Company/`.
2. Add `index.html`, both marks, `favicon.ico`, `CNAME`, `404.html`, `robots.txt`, `README.md`.
3. Commit and push to `main`.
4. Enable Pages via `gh api -X POST repos/AnointedAutomation/anointed-company/pages -f source[branch]=main -f source[path]=/`.
5. Set the custom domain via `gh api -X PUT repos/AnointedAutomation/anointed-company/pages -f cname=anointed.company`.
6. Hand the user the four A records and CNAME for their registrar.

## Testing

- Local: open `index.html` directly in browser; verify layout at 1440px, 1024px, 768px, 400px widths.
- After deploy: confirm `https://anointedautomation.github.io/anointed-company/` renders before DNS cuts over.
- After DNS propagates: confirm `https://anointed.company` and `https://www.anointed.company` both serve the page over HTTPS.

## Risks / Open Items

- Squarespace may have pre-existing A records or a `www` CNAME pointing to Squarespace's parking page. Those must be removed before the GitHub Pages records take effect; if both exist, the domain may flicker between the parking page and our Pages site during propagation. Plan to delete-then-add in one sitting.
- `apple-touch-icon.png` is a 1024×1024 graphic with black background; on a dark page it blends seamlessly but on light browser tabs it's a black square with art inside. That's fine for the in-page mark but means we use the gold cross (not this) as the favicon.
