# ucdglobalchange.github.io

UC Davis Global Environmental Change Lab Manual — an onboarding site for new lab members,
built with [Jupyter Book](https://jupyterbook.org) and published at
<https://ucdglobalchange.github.io/>.

- Content lives in `docs/` as Markdown; the sidebar order is set in `docs/_toc.yml`.
- Every push to `main` rebuilds and deploys the site via `.github/workflows/deploy-book.yml`.

## Build locally

```bash
python3 -m venv .venv && source .venv/bin/activate
pip install -r docs/requirements.txt
jupyter-book build docs/
open docs/_build/html/index.html
```

See `docs/Contributing/Instructions.md` for how to add or edit pages.
