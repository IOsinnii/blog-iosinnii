# Quarto Style Catalog

All styling options for Quarto HTML websites, organized by CSS source.

---

## Source 1: Built-in Bootswatch Themes (25 themes)

Drop-in replacements — change one line in `_quarto.yml`. No extra files needed.

```yaml
format:
  html:
    theme: THEME_NAME    # replace with any name below
```

### Light themes — clean & professional

| Theme | Personality | Best for |
|-------|-------------|----------|
| `flatly` | Flat, modern, green accent | Personal sites, portfolios |
| `litera` | Clean, book-like, highly readable | Academic blogs, writing |
| `lux` | Elegant, serif headings | Premium/minimalist sites |
| `minty` | Fresh, mint-green accents | Health, lifestyle, casual |
| `sandstone` | Warm, earthy tan tones | Nature, travel, outdoors |
| `cosmo` | Crisp, default Bootstrap look | General purpose |
| `journal` | Newspaper-inspired | Blogs, newsletters |
| `cerulean` | Ocean blue, corporate | Tech, business |
| `united` | Ubuntu-orange accents | Open source projects |
| `simplex` | Ultra-minimal, no frills | Documentation |
| `spacelab` | Dark blue, NASA-ish | Data science, STEM |
| `lumen` | Bright white, high contrast | Data dashboards |
| `pulse` | Vivid purple, energetic | Creative portfolios |
| `materia` | Material Design–inspired | App-like layouts |
| `yeti` | Cool gray tones, clean | Consulting, enterprise |
| `zephyr` | Soft blue, modern | Finance, analytics |

### Dark themes

| Theme | Personality | Best for |
|-------|-------------|----------|
| `darkly` | Dark gray, white text | Developer blogs, tech |
| `cyborg` | True black, neon accents | Hacker/dev aesthetic |
| `slate` | Warm dark gray | Night-mode preferred |
| `solar` | Solarized-inspired dark | Terminal-themed |
| `superhero` | Dark blue + orange | Gaming, sci-fi |

### Special / distinctive themes

| Theme | Personality | Best for |
|-------|-------------|----------|
| `morph` | Glassmorphism / frosted glass | Modern, Apple-inspired |
| `quartz` | Light glass + purple | Premium, futuristic |
| `vapor` | Synthwave pastels, pink/purple | Creative, retro-future |
| `sketchy` | Hand-drawn / comic style | Playful, unique |

---

### Full YAML snippets for every theme

Paste any block into your `_quarto.yml` under `format: html:`:

```yaml
# LIGHT — Flat / Modern
theme: flatly

# LIGHT — Book-like / Readable
theme: litera

# LIGHT — Elegant / Serif
theme: lux

# LIGHT — Mint / Fresh
theme: minty

# LIGHT — Warm / Earthy
theme: sandstone

# LIGHT — Bootstrap Default
theme: cosmo

# LIGHT — Newspaper
theme: journal

# LIGHT — Ocean Blue
theme: cerulean

# LIGHT — Ubuntu Orange
theme: united

# LIGHT — Ultra-minimal
theme: simplex

# LIGHT — NASA / STEM
theme: spacelab

# LIGHT — Bright White
theme: lumen

# LIGHT — Material Design
theme: materia

# LIGHT — Vivid Purple
theme: pulse

# LIGHT — Cool Gray
theme: yeti

# LIGHT — Soft Blue
theme: zephyr

# DARK — Developer Gray
theme: darkly

# DARK — True Black / Neon
theme: cyborg

# DARK — Warm Dark
theme: slate

# DARK — Solarized
theme: solar

# DARK — Sci-Fi Dark Blue
theme: superhero

# SPECIAL — Glassmorphism
theme: morph

# SPECIAL — Purple Glass
theme: quartz

# SPECIAL — Synthwave Pastels
theme: vapor

# SPECIAL — Hand-drawn / Comic
theme: sketchy
```

Preview all themes at: https://bootswatch.com/

---

## Source 2: Light + Dark Mode Toggle

Give users a toggle button to switch between themes:

```yaml
format:
  html:
    theme:
      light: flatly
      dark: darkly
```

Popular pairings:

```yaml
# Classic
theme:
  light: flatly
  dark: darkly

# Minimal
theme:
  light: litera
  dark: slate

# STEM / Data
theme:
  light: spacelab
  dark: cyborg

# Elegant
theme:
  light: lux
  dark: solar

# Futuristic
theme:
  light: quartz
  dark: vapor
```

Respect the user's OS preference:

```yaml
format:
  html:
    theme:
      light: flatly
      dark: darkly
    respect-user-color-scheme: true
```

---

## Source 3: Custom SCSS Overlay

Layer your overrides on top of any Bootswatch base:

```yaml
format:
  html:
    theme:
      - cosmo          # base theme (any of the 25 above)
      - custom.scss    # your overrides
```

### Minimal `custom.scss` template

```scss
/*-- scss:defaults --*/

/* ── Fonts ──────────────────────────────────────── */
$font-family-sans-serif: 'Inter', sans-serif;
$font-size-root:         16px;
$line-height-base:       1.7;

/* ── Palette ─────────────────────────────────────── */
$primary:    #2a6fa8;        /* navbar, buttons, active links */
$body-bg:    #fdfbf7;        /* page background */
$body-color: #2e2e2e;        /* default text */
$link-color: $primary;

/* ── Navigation ──────────────────────────────────── */
$navbar-bg:  $primary;
$navbar-fg:  #ffffff;
$footer-bg:  #f5f5f5;
$footer-fg:  #666666;

/* ── Headings ────────────────────────────────────── */
$headings-font-family: 'Josefin Sans', sans-serif;
$headings-font-weight: 600;
$h1-font-size: 2.2rem;
$h2-font-size: 1.6rem;

/* ── Code blocks ─────────────────────────────────── */
$code-block-border-left:       true;
$code-block-border-left-style: solid;
$code-block-bg:                #f8f8f8;

/*-- scss:rules --*/

/* ── Google Fonts import ─────────────────────────── */
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600&family=Josefin+Sans:wght@400;600&display=swap');

/* ── Custom rules ────────────────────────────────── */
.navbar-brand {
  font-weight: 700;
  letter-spacing: 0.03em;
}

.title-block-banner {
  padding: 2rem 0;
}
```

---

## Source 4: Bootstrap Sass Variables Only (no `.scss` file)

Quick tweaks directly in `_quarto.yml` — no separate file needed:

```yaml
format:
  html:
    theme: cosmo
    mainfont: "Georgia, serif"
    fontsize: "1.05em"
    linestretch: 1.7
    backgroundcolor: "#fdfbf7"
    fontcolor: "#2e2e2e"
    linkcolor: "#2a6fa8"
    max-width: "900px"
```

All supported inline options:

| Key | What it controls |
|-----|-----------------|
| `mainfont` | Body font-family |
| `monofont` | Code font-family |
| `fontsize` | Base font size |
| `fontcolor` | Default text color |
| `linkcolor` | Hyperlink color |
| `backgroundcolor` | Page background |
| `linestretch` | Line height (e.g. `1.6`) |
| `max-width` | Content width cap |
| `margin-left/right/top/bottom` | Body margins |
| `monobackgroundcolor` | Inline code background |

---

## Source 5: Plain CSS Override (`styles.css`)

Lightest option — no SCSS compilation, works alongside any theme:

```yaml
format:
  html:
    theme: cosmo
    css: styles.css
```

Example `styles.css`:

```css
/* Google Font */
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap');

body {
  font-family: 'Inter', sans-serif;
  background-color: #fdfbf7;
  color: #2e2e2e;
}

.navbar {
  background-color: #2a6fa8 !important;
}

h1, h2, h3 {
  font-weight: 600;
  color: #1a1a1a;
}

a {
  color: #2a6fa8;
}

/* Center profile image on landing page */
.about-image {
  object-fit: cover;
  object-position: 50% 20%;
}
```

---

## Source 6: No Framework (`theme: none`)

Strip all Bootstrap. Bring your own CSS entirely:

```yaml
format:
  html:
    theme: none
    css: styles.css
```

Use this when importing Tailwind, Pico CSS, or any other framework externally. Add the CDN link in `styles.css`:

```css
/* Example: Pico CSS (classless, minimal) */
@import url('https://cdn.jsdelivr.net/npm/@picocss/pico@2/css/pico.min.css');
```

---

## Decision Guide

| You want | Use |
|----------|-----|
| Fastest results, no config | Built-in theme (Source 1) |
| Light + dark toggle | `theme: light/dark` pair (Source 2) |
| Custom brand colors + font | SCSS overlay (Source 3) |
| Minor tweaks, no file | Inline Bootstrap vars (Source 4) |
| Simple CSS rules only | `css: styles.css` (Source 5) |
| Full custom framework | `theme: none` + custom CSS (Source 6) |
