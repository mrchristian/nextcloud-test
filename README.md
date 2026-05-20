# Nextcloud Test

A Quarto Book publication outputting HTML (GitHub Pages), PDF, and EPUB.

## Publication workflow

1. **Initialise Quarto Book project** — created `_quarto.yml` with `type: book`, output formats `html`, `pdf` (scrreprt), and `epub`; added GitHub Actions workflow (`.github/workflows/publish.yml`) to render and deploy to GitHub Pages on push to `main`.

2. **Convert DOCX source files to QMD** — used `quarto pandoc` with `-f docx -t markdown --wrap=none` to convert each `docx/` file to a Quarto Markdown chapter in `chapters/`.

3. **Extract embedded images** — re-ran conversions with `--extract-media=img/chNN` (executed from within `chapters/`) to produce relative image paths per chapter, avoiding path collisions between chapters sharing the same image filenames.

4. **Resolve duplicate heading IDs** — pandoc conversion preserved explicit `{#checklist}` and `{#faircare}` anchor IDs on recurring section headings. In a multi-chapter book these collide. Fixed by making each ID chapter-scoped (e.g. `{#checklist-ch01}`, `{#faircare-ch02}`).

5. **Add navigation aids** — added `downloads: [pdf, epub]`, `repo-url`, and `repo-actions: [edit, issue, source]` to `_quarto.yml` so the HTML site includes format download links and GitHub source links in the navbar.

## Render locally

```bash
quarto render
```

## Publish

The site is published to **https://mrchristian.github.io/nextcloud-test/** via GitHub Actions.

**How it works:**

- `.github/workflows/publish.yml` triggers on every push to `main`
- The `build` job runs `quarto render`, producing the book in `_book/`, then uploads it as a GitHub Pages artifact via `actions/upload-pages-artifact`
- The `deploy` job publishes the artifact to GitHub Pages via `actions/deploy-pages`

**Required repo setting (one-time):**  
Go to **Settings → Pages → Build and deployment → Source** and select **"GitHub Actions"**.

Push to `main` after that and the site deploys automatically.
