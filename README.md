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

Push to `main`. GitHub Actions renders and deploys to the `gh-pages` branch automatically.
