# Avoid Git Merge Nightmare When Tracking `docs/`

## The Problem

When you render a Quarto site on two different branches (`main` and `dev`), both branches produce their own version of `docs/` — including compiled files like `docs/site_libs/bootstrap/bootstrap.min.css`. These files differ between branches because they encode your theme and SCSS variables.

Git then sees both versions as "real" changes and refuses to merge, switch branches, or stash cleanly. You end up in a loop where every operation fails with:

```
error: Your local changes to the following files would be overwritten by merge:
        docs/site_libs/bootstrap/bootstrap.min.css
```

---

## The Root Cause

`docs/` is **build output**, not source code. It should never be committed to a development branch. The moment you run `quarto render` on `dev` and commit the result, you've created a divergence that Git cannot auto-resolve.

---

## The Fix: Never Commit `docs/` to `dev`

### Step 1 — Remove `docs/` from `dev` branch tracking

On your `dev` branch, untrack `docs/` but keep it locally:

```bash
git checkout dev
git rm -r --cached docs/
git commit -m "Stop tracking docs/ on dev"
```

### Step 2 — Add `docs/` to `.gitignore` on `dev`

Create or edit `.gitignore` on `dev` to include:

```
docs/
_site/
_freeze/
.quarto/
```

Commit it:
```bash
git add .gitignore
git commit -m "Ignore docs/ on dev"
git push
```

---

## The Correct Workflow Going Forward

### Developing on `dev`

```bash
git checkout dev
# edit .qmd, .scss, _quarto.yml
quarto preview          # test locally — does NOT commit docs/
git add _quarto.yml custom.scss about.qmd   # source files only
git commit -m "Styling updates"
git push origin dev
```

### Deploying to `main`

```bash
git checkout main
Remove-Item -Recurse -Force docs    # wipe any leftover build output
git merge dev                        # merge source files only — no docs/ conflict
quarto render                        # rebuild docs/ fresh on main
git add docs/
git commit -m "Deploy: render after merge"
git push
```

---

## Why `docs/` Must Be Committed on `main` (But Not `dev`)

GitHub Pages reads from `main/docs/`. So:

| Branch | `docs/` committed? | Why |
|--------|-------------------|-----|
| `main` | ✓ Yes | GitHub Pages needs it |
| `dev`  | ✗ No  | Only source files, no build output |

---

## Quick Recovery If You Get Stuck Again

```bash
# Nuclear option — always works
git merge --abort          # cancel any in-progress merge
Remove-Item -Recurse -Force docs
git merge dev
quarto render
git add docs/
git commit -m "Deploy"
git push
```
