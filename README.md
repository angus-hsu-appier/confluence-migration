# Team Knowledge Base

Markdown-based knowledge base, built with [MkDocs Material](https://squidfunk.github.io/mkdocs-material/) and deployed to GitHub Pages.

**Live site:** https://angus-hsu-appier.github.io/confluence-migration/

## Local preview

```bash
pip install -r requirements.txt
mkdocs serve
# open http://127.0.0.1:8000
```

## Editing

- **Quick edits:** click the pencil icon on any page → GitHub web editor → commit to `main`. Site rebuilds automatically.
- **Larger edits:** open a Pull Request as usual.

## Adding a new page

1. Create `docs/<section>/<page-title>.md` with frontmatter:
   ```yaml
   ---
   title: Page Title
   ---
   ```
2. Push to `main`. Navigation auto-generates from folder structure.

## Migration metadata

Pages migrated from Confluence include frontmatter pointing to the original source:

```yaml
---
title: ...
source: https://appier.atlassian.net/wiki/...
confluence_id: 1234567890
space: IDASH
last_modified: 2026-03-03
author: ...
migrated_at: 2026-06-03
---
```

## Deployment

`.github/workflows/deploy.yml` runs `mkdocs gh-deploy` on every push to `main` that touches docs. Output goes to the `gh-pages` branch, which GitHub Pages serves.

**One-time setup** (after first push):
1. Repo Settings → Pages → Source: **Deploy from a branch** → Branch: `gh-pages` / `/ (root)`.
