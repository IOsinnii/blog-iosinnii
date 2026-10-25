# How to Style a Quarto Blog from Default

A practical tutorial based on customizing a Quarto blog with the **Morph** theme and a custom navy blue (`#003559`) brand palette using `custom.scss`.

------------------------------------------------------------------------

## 1. Activate Custom SCSS Instead of Plain CSS

By default, `_quarto.yml` uses `css: styles.css`. For serious styling, switch to an SCSS overlay — it lets you override Bootstrap/Bootswatch Sass variables *before* the theme compiles, giving much deeper control.

**`_quarto.yml`:**

``` yaml
format:
  html:
    theme:
      - morph        # base Bootswatch theme
      - custom.scss  # your overrides on top
```

Do **not** mix `css:` and `theme: [morph, custom.scss]` — use one approach only.

------------------------------------------------------------------------

## 2. Set Brand Colors as SCSS Variables

In the `/*-- scss:defaults --*/` section of `custom.scss`, override Bootstrap Sass variables. These apply *globally* before the theme renders — cleaner and more reliable than fighting CSS specificity later.

``` scss
/*-- scss:defaults --*/

$primary:    #003559;   /* navbar, buttons, active links */
$body-color: #003559;   /* default body text */
$link-color: #003559;

$navbar-bg:  $primary;
$navbar-fg:  #ffffff;
$footer-bg:  #003559;
$footer-fg:  #ffffff;
```

> **Why not `$body-bg`?** If your theme has a special background (Morph uses a light blue glassmorphism bg), setting `$body-bg` overwrites it. Only set it if you want a plain color background.

------------------------------------------------------------------------

## 3. Force Text Color in the Body

SCSS variable `$body-color` sets the default, but some elements don't inherit it cleanly. Add explicit rules in `/*-- scss:rules --*/`:

``` scss
/*-- scss:rules --*/

body {
  color: $body-color !important;
}

.content, .page-content, main {
  color: $body-color !important;
}

p, li, td, span {
  color: $body-color !important;
}
```

------------------------------------------------------------------------

## 4. Keep Navbar and Banner Text White

The body color override will bleed into the navbar and title banners unless you explicitly re-whitify them:

``` scss
/* Navbar */
.navbar {
  background-color: $primary !important;
}

.navbar a,
.nav-link,
.navbar-brand {
  color: #ffffff !important;
}

.nav-link:hover {
  color: #e8e8e8 !important;
}

/* Title banner */
.title-block-banner,
.quarto-title-block,
.quarto-title-block-banner {
  background-color: $primary !important;
  color: #ffffff !important;
}

.title-block-banner h1,
.title-block-banner h2,
.title-block-banner p,
.title-block-banner a {
  color: #ffffff !important;
}
```

------------------------------------------------------------------------

## 5. Reduce Title Banner Height

The correct selector for the rendered Quarto banner is `.quarto-title-banner` — **not** `.title-block-banner` alone. Use `padding-top` and `padding-bottom` to control height:

``` scss
.quarto-title-banner {
  padding-top: 10px !important;
  padding-bottom: 10px !important;
  margin-bottom: 0 !important;
}
```

------------------------------------------------------------------------

## 6. Remove Gap Between Banner and Content

The gap below the banner before `H1` text comes from two sources: the content container padding and the `H1` top margin. Target both:

``` scss
div.about-contents {
  padding-top: 0 !important;
}

.about-contents h1:first-child,
main h1:first-child {
  margin-top: 0 !important;
}
```

------------------------------------------------------------------------

## 7. Style the Footer

The `$footer-bg` SCSS variable sets the background. For reliable text color, also add CSS rules:

``` scss
.nav-footer {
  background-color: #003559 !important;
  color: #ffffff !important;
}

.nav-footer a,
.nav-footer p,
.nav-footer span,
.nav-footer div {
  color: #ffffff !important;
}
```

**Footer content** is set in `_quarto.yml`, not in CSS:

``` yaml
page-footer:
  center: "© 2026 Your Name &nbsp;|&nbsp; Made with [Quarto](https://quarto.org)"
```

------------------------------------------------------------------------

## Pitfalls to Avoid

Based on hard-won lessons from this session:

1.  **Wrong selector for banner height** — `.title-block-banner` alone does not control the rendered height; use `.quarto-title-banner`. Using the wrong selector adds a *second* padding on top of the theme default, making it taller.

2.  **Setting `$body-bg` kills theme backgrounds** — Morph and other themed backgrounds are overwritten by `$body-bg`. Omit it to preserve the theme's native background.

3.  **`css:` is ignored when using SCSS overlay** — If `_quarto.yml` uses `theme: [morph, custom.scss]`, any `css: styles.css` line is silently ignored. All CSS rules must live in `custom.scss`.

4.  **Broad `div { color: ... !important }` breaks navbar/footer** — Targeting `div` globally will override navbar and banner text colors. Scope selectors to `main`, `.content`, or `.page-content`.

5.  **Hard refresh is required** — `quarto preview` caches stylesheets. Always use **Ctrl+Shift+R** after changing `.scss` files, or restart the preview server.

6.  **Padding rules cascade unexpectedly** — Adding padding to the wrong parent (e.g., `.quarto-title-meta`, `.title-metadata-section`) stacks on top of existing padding rather than replacing it. Inspect with browser DevTools first to identify the exact element before adding rules.
