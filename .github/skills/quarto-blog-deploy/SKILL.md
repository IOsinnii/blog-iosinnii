---
name: quarto-blog-deploy
description: 'Build, customize, and deploy a Quarto personal blog or website to GitHub Pages. Use when: creating a Quarto personal site from a template or fork, personalizing _quarto.yml, selecting Bootstrap/Bootswatch themes or writing custom SCSS, adding Google Fonts, pushing to GitHub (including Windows SSH port-22 troubleshooting and GitHub CLI + PAT fallback), deploying via docs/ folder, quarto publish, or GitHub Actions. Covers all 25 Bootswatch themes with YAML examples and custom SCSS patterns.'
argument-hint: 'quarto blog task (e.g. style, deploy, push, troubleshoot ssh)'
---

# Quarto Blog: Build, Customize & Deploy

Complete workflow for a Quarto personal blog — from blank template to live GitHub Pages site.

---

## Prerequisites

| Tool | Notes |
|------|-------|
| R ≥ 4.0 | https://www.r-project.org |
| RStudio / Positron (latest) | https://posit.co |
| Quarto CLI ≥ 1.3 | https://quarto.org |
| Git | https://git-scm.com |
| GitHub account | https://github.com |

---

## Step 1 — Create the Project

**From template (new blog):**
```bash
# RStudio: File → New Project → New Directory → Quarto Website
# or CLI:
quarto create project website my-blog
cd my-blog
```

**From a forked repo:**
```bash
git clone https://github.com/{USERNAME}/{REPO}.git
cd {REPO}
```

Open the `.Rproj` file in RStudio/Positron.

---

## Step 2 — Personalize `_quarto.yml`

```yaml
project:
  type: website
  output-dir: docs            # Required for GitHub Pages deployment

website:
  title: "{YOUR NAME}"
  site-url: "https://{USERNAME}.github.io/{REPO}/"
  navbar:
    right:
      - about.qmd
      - icon: github
        href: https://github.com/{USERNAME}
      - icon: linkedin
        href: https://linkedin.com/in/{LINKEDIN_HANDLE}
  page-footer:
    center: "© {YEAR} {YOUR NAME}"

format:
  html:
    theme: cosmo              # See Style Catalog (Step 4)
    css: styles.css

execute:
  freeze: auto                # Cache R/Python output for GitHub Actions
```

**Always update:** `title`, `site-url`, navbar `href` values, footer copyright, `theme`.

---

## Step 3 — Personalize Content Pages

### `index.qmd` (landing page)

```yaml
---
image: images/profile.jpg
toc: false
about:
  template: jolla             # jolla | trestles | solana | marquee | broadside
  image-shape: round
  links:
    - icon: github
      text: GitHub
      href: https://github.com/{USERNAME}
    - icon: linkedin
      text: LinkedIn
      href: https://linkedin.com/in/{LINKEDIN_HANDLE}
---

Hi! I'm {YOUR NAME}. [Write your intro here.]
```

About page templates:

| Template | Layout |
|----------|--------|
| `jolla` | Centered photo, bio below |
| `trestles` | Photo left, content right |
| `solana` | Photo right, content left |
| `marquee` | Wide banner photo, nav links below |
| `broadside` | Two-column, photo one side |

### `about.qmd`

```yaml
---
title: "About {YOUR NAME}"
image: images/profile.jpg
about:
  template: broadside
  links:
    - icon: github
      text: GitHub
      href: https://github.com/{USERNAME}
title-block-banner: "#003559"
title-block-banner-color: "white"
---

[Your bio here]
```

### Blog post (`posts/my-post/index.qmd`)

```yaml
---
title: "My First Post"
author: "{YOUR NAME}"
date: "2026-01-01"
categories: [R, data science]
image: thumbnail.jpg
---

Post content here.
```

---

## Step 4 — Style the Site

See full **[Style Catalog](./references/themes.md)** — all 25 Bootswatch themes with previews, personality notes, and ready-to-paste `_quarto.yml` snippets.

### Quick picks for personal blogs

```yaml
# Clean & professional
format:
  html:
    theme: flatly

# Minimal & readable
format:
  html:
    theme: litera

# Dark & modern
format:
  html:
    theme: darkly

# Glassmorphism / frosted glass
format:
  html:
    theme: morph

# Bold & colourful
format:
  html:
    theme: pulse

# Light/dark toggle (user can switch)
format:
  html:
    theme:
      light: flatly
      dark: darkly
```

### Custom SCSS overlay (drop-in palette swap)

```yaml
# _quarto.yml
format:
  html:
    theme:
      - cosmo          # base Bootswatch theme
      - custom.scss    # your overrides on top
```

`custom.scss` (place in project root):

```scss
/*-- scss:defaults --*/

/* Google Font (optional — also add @import below) */
$font-family-sans-serif: 'Inter', sans-serif;
$font-size-root: 16px;

/* Colors */
$primary:    #2a6fa8;   /* navbar, buttons, links */
$body-bg:    #fdfbf7;   /* page background */
$body-color: #333333;   /* default text */
$link-color: #2a6fa8;

/* Navigation */
$navbar-bg: $primary;
$footer-bg: #f5f5f5;

/*-- scss:rules --*/

/* Import Google Font (add any Google Fonts URL here) */
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600&display=swap');
```

### CSS-only override (no SCSS, quickest)

```yaml
# _quarto.yml
format:
  html:
    theme: cosmo
    css: styles.css    # plain CSS file — fastest option
```

### Bootstrap Sass variables inline (no separate file needed)

```yaml
format:
  html:
    theme: cosmo
    backgroundcolor: "#fdfbf7"
    fontcolor: "#333333"
    linkcolor: "#2a6fa8"
    mainfont: "Georgia, serif"
    fontsize: "1.05em"
    linestretch: 1.7
```

---

## Step 5 — Initialize Git & First Commit

```bash
cd /path/to/my-blog

# Create .nojekyll (required for GitHub Pages)
# Windows PowerShell:
copy NUL .nojekyll
# Mac/Linux:
touch .nojekyll

git init
git add .
git commit -m "Initial commit: Quarto blog setup"
```

Create a new repo at https://github.com/new (match `{REPO}` to `output-dir: docs` expectations), then:

```bash
git remote add origin https://github.com/{USERNAME}/{REPO}.git
git branch -M main
```

---

## Step 6 — Push to GitHub

### Option A: SSH (preferred — if port 22 is accessible)

```bash
# 1. Generate SSH key (one-time)
ssh-keygen -t ed25519 -C "your@email.com" -f ~/.ssh/id_ed25519 -N ""

# 2. Copy public key
cat ~/.ssh/id_ed25519.pub
# → go to github.com/settings/ssh/new → paste → save

# 3. Test connection
ssh -T git@github.com
# Expected: "Hi {USERNAME}! You've successfully authenticated..."

# 4. Push
git remote set-url origin git@github.com:{USERNAME}/{REPO}.git
git push -u origin main
```

If `ssh -T` fails → see Troubleshooting below.

### Option B: GitHub CLI + Personal Access Token (Windows fallback)

Use this when SSH is blocked — it always works regardless of network restrictions.

```bash
# 1. Verify GitHub CLI is installed
gh --version
# If missing: winget install GitHub.cli

# 2. Create a token:
#    github.com/settings/tokens → Generate new token (classic) → check 'repo' → Generate
#    Copy the token (ghp_xxxx...) immediately

# 3. Authenticate
echo YOUR_TOKEN_HERE | gh auth login --with-token

# 4. Push via HTTPS
git remote set-url origin https://github.com/{USERNAME}/{REPO}.git
git push -u origin main
```

---

## Step 7 — Deploy to GitHub Pages

### Method A: Render to `docs/` (simplest, recommended)

1. Confirm `_quarto.yml` has `output-dir: docs`
2. Render and push:
   ```bash
   quarto render
   git add docs .nojekyll
   git commit -m "Publish site to docs/"
   git push
   ```
3. On GitHub: **Settings → Pages → Branch: `main` / Folder: `/docs` → Save**

Site live at: `https://{USERNAME}.github.io/{REPO}/`

### Method B: `quarto publish` (one-command)

```bash
quarto publish gh-pages
```

Automatically creates a `gh-pages` branch and deploys. No manual GitHub settings needed for project repos.

> For user sites (`{USERNAME}.github.io`): after first publish, go to Settings → Pages → set Source branch to `gh-pages`.

### Method C: GitHub Actions (auto-deploy on every push)

First run `quarto publish gh-pages` once locally. Then add `.github/workflows/publish.yml`:

```yaml
on:
  workflow_dispatch:
  push:
    branches: main

name: Quarto Publish

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v4
      - uses: quarto-dev/quarto-actions/setup@v2
      - uses: quarto-dev/quarto-actions/publish@v2
        with:
          target: gh-pages
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

Enable write permissions: **GitHub repo → Settings → Actions → Workflow permissions → Read and write**.

Commit `_freeze/` directory alongside your source files.

---

## Troubleshooting

### SSH: `connect to host github.com port 22: Permission denied`

Port 22 is blocked at the ISP or network level. **Disabling Windows Defender / Firewall does NOT fix ISP-level port blocking.**

Solutions (in order):

1. **Use GitHub CLI + PAT** (Step 6 Option B) — always works
2. **Try SSH over HTTPS port 443** — add to `~/.ssh/config`:
   ```
   Host github.com
     HostName ssh.github.com
     Port 443
     User git
     IdentityFile ~/.ssh/id_ed25519
   ```
   Then test: `ssh -T git@github.com`
3. **Mobile hotspot** — temporarily bypasses ISP port blocking

### `fatal: Authentication failed` on HTTPS push

GitHub removed password authentication. You must use a Personal Access Token as the password. See Step 6 Option B.

### `gh auth login` fails (stale token in keyring)

```bash
gh auth logout -u {USERNAME}
# then re-authenticate with token:
echo YOUR_TOKEN | gh auth login --with-token
```

### `_site/` committed to git by accident

Add to `.gitignore`:
```
/_site/
/.quarto/
```
Then remove from tracking:
```bash
git rm -r --cached _site
git commit -m "Remove _site from tracking"
```

### Post dates showing incorrectly

Set explicit `date:` in each post's YAML:
```yaml
date: "2026-01-15"
```

---

## Quick Reference

| Task | Command |
|------|---------|
| Preview locally | `quarto preview` |
| Render site | `quarto render` |
| One-command deploy | `quarto publish gh-pages` |
| Test SSH | `ssh -T git@github.com` |
| Check remote URL | `git remote -v` |
| GitHub CLI auth status | `gh auth status` |
| Re-authenticate CLI | `gh auth logout -u {USER}` then re-login |
