# Contributing to This Manual

This manual is a living document. Everyone in the lab is encouraged to improve it.

The site is built with [Jupyter Book](https://jupyterbook.org) from Markdown files in the
[`docs/`](https://github.com/UCDglobalchange/ucdglobalchange.github.io/tree/main/docs) folder of
the [lab manual repository](https://github.com/UCDglobalchange/ucdglobalchange.github.io).
Every push to `main` rebuilds and publishes <https://ucdglobalchange.github.io> automatically
within a few minutes.

## Quick edits

Click the **pencil (edit)** icon at the top of any page, edit the Markdown on GitHub, and
commit (or open a pull request).

## Larger edits

```bash
git clone git@github.com:UCDglobalchange/ucdglobalchange.github.io.git
cd ucdglobalchange.github.io
python3 -m venv .venv && source .venv/bin/activate
pip install -r docs/requirements.txt
jupyter-book build docs/              # then open docs/_build/html/index.html
```

When you're happy, commit and push (or open a pull request):

```bash
git add docs/
git commit -m "Add page on <topic>"
git push
```

## Adding a page

1. Create a Markdown file in the right section folder, e.g. `docs/Computing/MyNewTopic.md`,
   starting with a `# Title` heading.
2. Add it to `docs/_toc.yml` under the right `caption`:

   ```yaml
   - caption: Computing on FARM
     chapters:
       - file: Computing/MyNewTopic.md
   ```

3. Put images in `docs/Images/` and reference them with `![alt text](../Images/figure.png)`.

## Adding a section

Add a new folder under `docs/` and a new `caption:` block in `docs/_toc.yml`.

## Formatting tips

Pages use [MyST Markdown](https://mystmd.org/guide), which supports admonitions:

````md
```{note}
This is a note.
```
````

Notebooks (`.ipynb`) can also be added as pages; they are displayed but not re-executed.
