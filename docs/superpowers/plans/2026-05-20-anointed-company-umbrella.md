# anointed.company — Umbrella Landing Page Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Ship a single static HTML page at `https://anointed.company` linking to `anointedattireapparel.com` and `anointedautomation.net`.

**Architecture:** Plain `index.html` + static assets at the root of a new public GitHub repo `AnointedAutomation/anointed-company`. Served by GitHub Pages from `main` branch root. Custom domain via `CNAME` file + DNS records at Squarespace (user-applied). No build tooling.

**Tech Stack:** HTML5, vanilla CSS, GitHub Pages, Squarespace DNS.

**Repo path on disk:** `/home/roku674/Anointed Automation LLC/Anointed Company/`

---

## File Structure

```
anointed-company/
├── index.html          # the page — markup + inline <style>
├── attire-mark.png     # praying-hands graphic for Attire & Apparel
├── automation-mark.png # gold cross for Automation
├── CNAME               # single line: anointed.company
├── 404.html            # minimal styled 404 → links back to /
├── robots.txt          # allow all
├── README.md           # one paragraph: what this is, how to edit
└── docs/               # spec + plan already exist here from brainstorming
```

Inline CSS in `index.html` (no separate `.css`) — the page is short enough that splitting hurts more than it helps. Same for inline `<style>` over a CSS file.

---

## Task 1: Create GitHub repo and clone locally

**Files:**
- Create: `/home/roku674/Anointed Automation LLC/Anointed Company/` (becomes git repo)

- [ ] **Step 1: Verify the empty target directory has no leftover state**

```bash
ls -la "/home/roku674/Anointed Automation LLC/Anointed Company/"
```

Expected: shows `.superpowers/` (brainstorm artifacts) and `docs/` (spec + plan). No `.git/` directory. If `.git/` exists, stop and ask the user before continuing.

- [ ] **Step 2: Create the public repo on GitHub and clone into a temp location**

`gh repo create --clone` always creates a subdirectory matching the repo name; we'll clone next to the target and move the `.git/` in.

```bash
cd /tmp
gh repo create AnointedAutomation/anointed-company --public \
  --description "Umbrella landing page for anointed.company" \
  --clone
ls -la /tmp/anointed-company/
```

Expected: `gh` prints the new repo URL; `/tmp/anointed-company/` exists and contains only `.git/` (empty repo).

- [ ] **Step 3: Move git metadata into the existing project folder**

```bash
mv /tmp/anointed-company/.git "/home/roku674/Anointed Automation LLC/Anointed Company/.git"
rmdir /tmp/anointed-company
cd "/home/roku674/Anointed Automation LLC/Anointed Company"
git status
```

Expected: `git status` reports `On branch main` and lists `.superpowers/` and `docs/` as untracked.

- [ ] **Step 4: Add a `.gitignore` to keep brainstorm artifacts out of the repo**

Create `/home/roku674/Anointed Automation LLC/Anointed Company/.gitignore`:

```
.superpowers/
.DS_Store
*.log
```

- [ ] **Step 5: Commit the spec, plan, and gitignore**

```bash
cd "/home/roku674/Anointed Automation LLC/Anointed Company"
git add .gitignore docs/
git status
git commit -m "Add design spec and implementation plan"
```

Expected: commit succeeds; `git log --oneline` shows one commit.

---

## Task 2: Add static image assets

**Files:**
- Create: `attire-mark.png`
- Create: `automation-mark.png`

- [ ] **Step 1: Copy the Attire mark from OneDrive**

```bash
cd "/home/roku674/Anointed Automation LLC/Anointed Company"
cp "/mnt/c/Users/Roku6/OneDrive/Pictures/apple-touch-icon.png" attire-mark.png
file attire-mark.png
```

Expected: `file` reports PNG image data, 1024 x 1024.

- [ ] **Step 2: Copy the Automation mark (gold cross) from the Frontend repo**

```bash
cp "/home/roku674/Anointed Automation LLC/Website/Frontend/public/favicon.png" automation-mark.png
file automation-mark.png
```

Expected: `file` reports PNG image data, 1024 x 1024 (it's the same size).

- [ ] **Step 3: Commit the assets**

```bash
git add attire-mark.png automation-mark.png
git commit -m "Add brand marks for Attire and Automation"
```

---

## Task 3: Write `index.html`

**Files:**
- Create: `index.html`

This is the page. Single file with inline `<style>`. Mobile-friendly via media query that stacks the cards under 700px.

- [ ] **Step 1: Write `index.html`**

Create `/home/roku674/Anointed Automation LLC/Anointed Company/index.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Anointed</title>
  <meta name="description" content="Anointed — two ventures by Alexander Fields. Anointed Attire & Apparel (faith-rooted streetwear) and Anointed Automation (custom software consulting).">
  <link rel="icon" type="image/png" href="automation-mark.png">
  <style>
    :root {
      --bg-1: #1a1a1f;
      --bg-2: #0a0a0d;
      --gold: #c9a96e;
      --cream: #f4e9d2;
      --cream-dim: #a89878;
      --cream-muted: #7a6b56;
      --rule: #c9a96e44;
      --rule-faint: #c9a96e22;
    }
    * { box-sizing: border-box; }
    html, body { margin: 0; padding: 0; }
    body {
      min-height: 100vh;
      background: radial-gradient(ellipse at top, var(--bg-1), var(--bg-2));
      color: var(--cream);
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
      -webkit-font-smoothing: antialiased;
    }
    .wordmark {
      text-align: center;
      padding: 64px 24px 0;
    }
    .wordmark h1 {
      font-family: Georgia, "Times New Roman", serif;
      font-size: 60px;
      font-weight: 300;
      color: var(--cream);
      line-height: 1;
      margin: 0;
      letter-spacing: -1px;
    }
    .wordmark .rule {
      width: 48px;
      height: 1px;
      background: var(--gold);
      margin: 24px auto 40px;
    }
    .ventures {
      display: flex;
      gap: 1px;
      background: var(--rule-faint);
      padding: 0 24px 64px;
      max-width: 1100px;
      margin: 0 auto;
    }
    .venture {
      flex: 1;
      background: var(--bg-2);
      padding: 40px 32px;
      border: 1px solid var(--rule);
      text-align: center;
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    .venture .mark {
      height: 140px;
      display: flex;
      align-items: center;
      justify-content: center;
      margin-bottom: 20px;
    }
    .venture .mark img { max-height: 140px; max-width: 100%; object-fit: contain; }
    .venture[data-kind="automation"] .mark img { max-height: 120px; }
    .venture .label {
      font-size: 10px;
      letter-spacing: 3px;
      color: var(--gold);
      margin-bottom: 12px;
    }
    .venture h2 {
      font-family: Georgia, "Times New Roman", serif;
      font-size: 26px;
      color: var(--cream);
      line-height: 1.1;
      margin: 0 0 14px;
      font-weight: 400;
    }
    .venture p {
      font-size: 14px;
      color: var(--cream-dim);
      line-height: 1.5;
      margin: 0 0 28px;
      max-width: 320px;
    }
    .cta {
      display: inline-block;
      border: 1px solid var(--gold);
      color: var(--cream);
      padding: 12px 26px;
      font-size: 11px;
      letter-spacing: 2px;
      text-decoration: none;
      transition: background-color 150ms ease, color 150ms ease;
    }
    .cta:hover, .cta:focus { background: var(--gold); color: var(--bg-2); }
    .venture .domain {
      font-size: 10px;
      color: var(--cream-muted);
      margin-top: 16px;
      letter-spacing: 1px;
      font-family: ui-monospace, "SF Mono", Menlo, monospace;
    }
    @media (max-width: 700px) {
      .wordmark { padding-top: 48px; }
      .wordmark h1 { font-size: 48px; }
      .ventures { flex-direction: column; padding: 0 16px 48px; }
      .venture { padding: 32px 24px; }
    }
  </style>
</head>
<body>
  <header class="wordmark">
    <h1>Anointed</h1>
    <div class="rule" aria-hidden="true"></div>
  </header>
  <main class="ventures">
    <section class="venture" data-kind="attire">
      <div class="mark"><img src="attire-mark.png" alt=""></div>
      <div class="label">CLOTHING</div>
      <h2>Attire &amp; Apparel</h2>
      <p>Faith-rooted streetwear, made to order. Hats, hoodies, tees.</p>
      <a class="cta" href="https://anointedattireapparel.com">VISIT SHOP &rarr;</a>
      <div class="domain">anointedattireapparel.com</div>
    </section>
    <section class="venture" data-kind="automation">
      <div class="mark"><img src="automation-mark.png" alt=""></div>
      <div class="label">SOFTWARE</div>
      <h2>Automation</h2>
      <p>Custom integrations, automations, and development consulting for small businesses.</p>
      <a class="cta" href="https://anointedautomation.net">VISIT SITE &rarr;</a>
      <div class="domain">anointedautomation.net</div>
    </section>
  </main>
</body>
</html>
```

- [ ] **Step 2: Smoke-test by opening in a browser**

```bash
cd "/home/roku674/Anointed Automation LLC/Anointed Company"
explorer.exe index.html
```

(In WSL, `explorer.exe <file>` opens it in the Windows default browser. On other systems use `xdg-open` or `open`.)

Expected: page loads showing "Anointed" wordmark, gold rule, then two cards side-by-side. The Attire card shows the praying-hands graphic; the Automation card shows the gold cross.

- [ ] **Step 3: Resize the browser to verify responsive stacking**

Drag the window narrower than 700px wide. Expected: cards stack vertically, wordmark shrinks to 48px.

- [ ] **Step 4: Click both CTA links to confirm they open the right sites**

Expected: "VISIT SHOP" opens `anointedattireapparel.com`; "VISIT SITE" opens `anointedautomation.net`.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "Add index.html with two-venture layout"
```

---

## Task 4: Add supporting files (`CNAME`, `404.html`, `robots.txt`, `README.md`)

**Files:**
- Create: `CNAME`
- Create: `404.html`
- Create: `robots.txt`
- Create: `README.md`

- [ ] **Step 1: Write `CNAME` (one line, no trailing newline issues)**

Create `/home/roku674/Anointed Automation LLC/Anointed Company/CNAME` with exactly this content:

```
anointed.company
```

Verify:

```bash
cat -A "/home/roku674/Anointed Automation LLC/Anointed Company/CNAME"
```

Expected: `anointed.company$` (the `$` is `cat -A`'s end-of-line marker).

- [ ] **Step 2: Write `404.html`**

Minimal styled 404 that shares the page's look and links home.

Create `/home/roku674/Anointed Automation LLC/Anointed Company/404.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Not found — Anointed</title>
  <link rel="icon" type="image/png" href="automation-mark.png">
  <style>
    html, body { margin: 0; padding: 0; }
    body {
      min-height: 100vh;
      background: radial-gradient(ellipse at top, #1a1a1f, #0a0a0d);
      color: #f4e9d2;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      text-align: center;
      padding: 24px;
    }
    h1 {
      font-family: Georgia, serif;
      font-size: 48px;
      font-weight: 300;
      margin: 0 0 8px;
      letter-spacing: -1px;
    }
    .rule { width: 48px; height: 1px; background: #c9a96e; margin: 20px auto; }
    p { color: #a89878; font-size: 14px; margin: 0 0 28px; }
    a {
      display: inline-block;
      border: 1px solid #c9a96e;
      color: #f4e9d2;
      padding: 12px 26px;
      font-size: 11px;
      letter-spacing: 2px;
      text-decoration: none;
    }
    a:hover { background: #c9a96e; color: #0a0a0d; }
  </style>
</head>
<body>
  <h1>404</h1>
  <div class="rule"></div>
  <p>That page doesn't live here.</p>
  <a href="/">RETURN HOME &rarr;</a>
</body>
</html>
```

- [ ] **Step 3: Write `robots.txt`**

Create `/home/roku674/Anointed Automation LLC/Anointed Company/robots.txt`:

```
User-agent: *
Allow: /
```

- [ ] **Step 4: Write `README.md`**

Create `/home/roku674/Anointed Automation LLC/Anointed Company/README.md`:

```markdown
# anointed.company

Umbrella landing page for the two ventures of Alexander Fields:

- [Anointed Attire & Apparel](https://anointedattireapparel.com) — clothing
- [Anointed Automation](https://anointedautomation.net) — software consulting

## Editing

This is a static HTML page. To make changes:

1. Edit `index.html`.
2. Open it directly in a browser to preview.
3. Commit and push to `main` — GitHub Pages serves the changes within ~30 seconds.

## Files

- `index.html` — the page
- `attire-mark.png` / `automation-mark.png` — the two venture marks
- `CNAME` — pins the custom domain `anointed.company`
- `404.html` — error page
- `robots.txt` — search crawler policy
- `docs/superpowers/` — spec and implementation plan from initial build
```

- [ ] **Step 5: Smoke-test the 404 page**

```bash
cd "/home/roku674/Anointed Automation LLC/Anointed Company"
explorer.exe 404.html
```

Expected: shows "404", gold rule, "That page doesn't live here.", and a "RETURN HOME →" button.

- [ ] **Step 6: Commit**

```bash
git add CNAME 404.html robots.txt README.md
git commit -m "Add CNAME, 404 page, robots.txt, README"
```

---

## Task 5: Push to GitHub

**Files:** (none — git operation)

- [ ] **Step 1: Push `main` to origin**

```bash
cd "/home/roku674/Anointed Automation LLC/Anointed Company"
git log --oneline
git push -u origin main
```

Expected: push succeeds; the commits from Tasks 1-4 are now on GitHub.

- [ ] **Step 2: Verify on GitHub via API**

```bash
gh api repos/AnointedAutomation/anointed-company --jq '.default_branch,.html_url,.visibility'
```

Expected: `main`, `https://github.com/AnointedAutomation/anointed-company`, `public`.

- [ ] **Step 3: Confirm files are present in the remote tree**

```bash
gh api repos/AnointedAutomation/anointed-company/contents --jq '.[].name' | sort
```

Expected output includes: `404.html`, `CNAME`, `README.md`, `attire-mark.png`, `automation-mark.png`, `docs`, `index.html`, `robots.txt`.

---

## Task 6: Enable GitHub Pages and set custom domain

**Files:** (none — GitHub API operations)

- [ ] **Step 1: Enable Pages, serving from `main` branch root**

```bash
gh api -X POST repos/AnointedAutomation/anointed-company/pages \
  -f 'source[branch]=main' \
  -f 'source[path]=/'
```

Expected: HTTP 201 response with a JSON body containing a `url` like `https://anointedautomation.github.io/anointed-company/`.

If the response says Pages is already enabled, that's fine — proceed.

- [ ] **Step 2: Set the custom domain to `anointed.company`**

Note: the `CNAME` file in the repo (Task 4) already declares the domain, but we still set it via the API to make sure the Pages settings UI reflects it and the HTTPS provisioning kicks off.

```bash
gh api -X PUT repos/AnointedAutomation/anointed-company/pages \
  -f cname=anointed.company
```

Expected: HTTP 204 (no content) on success.

- [ ] **Step 3: Verify the Pages configuration**

```bash
gh api repos/AnointedAutomation/anointed-company/pages \
  --jq '{cname, html_url, source, https_enforced, status}'
```

Expected:

```json
{
  "cname": "anointed.company",
  "html_url": "https://anointed.company/",
  "source": {"branch": "main", "path": "/"},
  "https_enforced": true,
  "status": "building"
}
```

`status` may be `building` initially; it transitions to `built` once the first deploy finishes (usually under a minute).

- [ ] **Step 4: Wait for first build to finish and verify the temporary `*.github.io` URL serves the page**

```bash
until [ "$(gh api repos/AnointedAutomation/anointed-company/pages --jq '.status')" = "built" ]; do sleep 10; done
curl -sI https://anointedautomation.github.io/anointed-company/ | head -3
```

Expected: status reaches `built`; curl returns `HTTP/2 200`.

If curl returns 404 even after `built`, wait another minute and retry — Pages CDN propagation can lag the build status briefly.

---

## Task 7: User-applied DNS at Squarespace

**Files:** (none — manual user action)

This is the only step the user has to do. Hand them these instructions verbatim.

- [ ] **Step 1: User opens Squarespace DNS settings**

Squarespace dashboard → Settings → Domains → click `anointed.company` → DNS Settings → scroll to "Custom Records".

- [ ] **Step 2: User removes any pre-existing records that conflict**

Look for and delete:
- Any existing A records on the apex (`@`) that point to Squarespace IPs.
- Any existing CNAME on `www` that points to a `*.squarespace.com` address.

Keep MX records (email), TXT records (verification), and the SOA/NS records (Squarespace nameservers) untouched.

- [ ] **Step 3: User adds the four GitHub Pages A records on the apex**

For each of the following, click "Add Record" → Type: `A` → Host: `@` → Data:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

- [ ] **Step 4: User adds the `www` CNAME**

Click "Add Record" → Type: `CNAME` → Host: `www` → Data: `anointedautomation.github.io`

- [ ] **Step 5: User saves and waits**

Save in Squarespace. DNS typically propagates within 10-30 minutes; can take up to a few hours.

- [ ] **Step 6: Verify DNS from your side**

```bash
dig +short anointed.company
dig +short www.anointed.company
```

Expected:
- `anointed.company` resolves to the four GitHub Pages IPs.
- `www.anointed.company` resolves through `anointedautomation.github.io.` to its A records.

- [ ] **Step 7: Trigger / confirm HTTPS cert provisioning at GitHub**

Once DNS resolves, GitHub auto-provisions a Let's Encrypt cert. To force-check:

```bash
gh api -X POST repos/AnointedAutomation/anointed-company/pages/https_certificate 2>&1 | head -5
gh api repos/AnointedAutomation/anointed-company/pages --jq '.https_certificate.state,.https_enforced'
```

Expected: `state` reaches `approved` (may show `provisioning` first; takes 5-30 minutes after DNS resolves). `https_enforced` should be `true`.

- [ ] **Step 8: Final verification**

```bash
curl -sI https://anointed.company/ | head -3
curl -sI https://www.anointed.company/ | head -3
```

Expected: both return `HTTP/2 200`.

Open `https://anointed.company` in a browser. Confirm:
- Page loads over HTTPS (lock icon, no warnings).
- Both CTA links open the correct child sites.
- Page renders correctly on a phone (narrow viewport, cards stack).

---

## Done

After Task 7, the umbrella site is live. To make changes later: edit `index.html`, commit, push — Pages re-deploys automatically within ~30 seconds.
