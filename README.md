# Ivan Osinnii's Blog

A Quarto-powered personal blog and portfolio, ready to fork and rebrand.

## Quick Start

**Prerequisites:** R, Quarto CLI, Git

**Local preview:**
```bash
quarto preview
```

**Build for deployment:**
```bash
quarto render
```

## How to Update

### Add a new post

1. Create a folder under `posts/`:
   ```
   posts/my-new-post/
   ├── index.qmd          # Post content
   └── images/            # (optional) Post-specific images
   ```

2. In `posts/my-new-post/index.qmd`, add YAML frontmatter:
   ```yaml
   ---
   title: "Post Title"
   author: "Ivan Osinnii"
   date: "2026-01-15"
   categories: [tag1, tag2]
   image: "thumbnail.jpg"  # (optional)
   ---

   Your post content here.
   ```

3. Render and push:
   ```bash
   quarto render
   git add docs
   git commit -m "Add new post"
   git push
   ```

### Update existing post

Edit the `.qmd` file in the post folder, then render and push.

### Update About page

Edit `about.qmd` — update bio, links, and profile image path.

### Update site metadata

Edit `_quarto.yml` for:
- Site title and URL
- Navigation links
- Footer copyright
- Theme and styling

## Styling

**Current:** Navy blue (`#003559`) theme applied via `styles.css`.

**Alternative themes** (commented in `_quarto.yml`):
- Flatly, Litera, Morph, Light/Dark toggle, or custom SCSS overlay

Uncomment the desired option in `_quarto.yml` and run `quarto render`.

## Compliance with Standard Structure

This project follows the [Quarto blog deployment standard](https://quarto.org), making it ideal for:
- ✓ Forking and rebranding as your own blog
- ✓ Using the same workflow for GitHub Pages deployment
- ✓ Leveraging the same customization patterns (themes, fonts, CSS overrides)

**To fork and rebrand:**
1. Clone/fork this repo
2. Update `_quarto.yml`: title, site-url, navbar links, footer
3. Update `about.qmd`: bio, image, links
4. Customize `styles.css` or swap theme in `_quarto.yml`
5. Replace posts in `posts/`
6. Deploy to GitHub Pages (Settings → Pages → Branch: main / Folder: /docs)

---

Built with [Quarto](https://quarto.org)
