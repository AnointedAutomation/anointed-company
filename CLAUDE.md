# Anointed Company (anointed.company)

## What this is
Static one-page umbrella landing site for Anointed Online Store (formerly Anointed Attire & Apparel; "Anointed Attire" now means only the Amazon brand) and Anointed Automation. Plain HTML, no build
step, no package manifest, no tests. Custom domain pinned by `CNAME`.

## Inputs
- Direct requests from the owner. No PROJECT_WORK.md in this repo. `docs/` is gitignored here (local plans only).

## Process
- Edit `index.html` (or `404.html`), open it directly in a browser to preview.
- Every file starts with the Anointed copyright header (see `CONTRIBUTING.md`); README carries "Jesus is King" under the title.
- Keep `llms.txt`, `llms.json`, `sitemap.xml`, `robots.txt` in sync with page changes.

## Outputs
- Served by GitHub Pages. README says "commit and push to `main`". Which branch Pages publishes: Unknown, verify in repo Pages settings.
- Work on `develop`; never push `main` without the owner's explicit instruction.

## Verification
- Open `index.html` in a browser at desktop and phone width; click both venture links.
- Check `llms.json` parses: `python3 -c "import json;json.load(open('llms.json'))"`.

## Read only when needed
| Doing | Read |
|---|---|
| File header rule | `CONTRIBUTING.md` |
| File list and editing flow | `README.md` |
| AI-facing site description | `llms.txt`, `llms.json` |

## Landmines
- `docs/` is ignored by git, so notes placed there never commit.
- Do not remove `CNAME`; it pins the custom domain.
- Avoid the word "creator" in copy; use "built by", "founder", "made by".
