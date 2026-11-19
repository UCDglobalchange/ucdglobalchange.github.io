# Your Personal Website

A simple personal website helps other researchers, collaborators, and future employers find you
and your work. You can host one for free on **GitHub Pages** at `https://<yourusername>.github.io`.

The lab's step-by-step tutorial is in the
**[GitHub-website repository](https://github.com/UCDglobalchange/GitHub-website)**. Here is an
overview.

## Steps

1. **Create a repository** named exactly `<yourusername>.github.io` in your **personal** GitHub
   account (not the lab organization). Make it **Public** and tick *Add a README file*.
2. **Add a homepage**: create `index.html`. The tutorial has starter HTML you can paste in and edit.
3. **Turn on GitHub Pages**: *Settings → Pages → Build and deployment*, choose *Deploy from a
   branch*, branch `main`, folder `/ (root)`, and save.
4. **View it** at `https://<yourusername>.github.io` after a minute or so.
5. **Grow it**: move styles into `style.css`, add `about.html` and `projects.html`, and add a
   navigation bar.

```text
<yourusername>.github.io/
├── index.html
├── about.html
├── projects.html
├── style.css
└── images/
```

## What to include

- A photo and a short bio, mentioning that you're in the
  **Global Environmental Change Lab at UC Davis**.
- Your research interests and projects.
- Your CV (PDF) and publications.
- Links to GitHub, Google Scholar, ORCID, and LinkedIn.
- Contact information.

## Troubleshooting

- **404 error**: check that the repository name is exactly `<yourusername>.github.io`, that
  `index.html` exists, and that Pages is enabled on the right branch.
- **Styles not showing**: check the filename and the path in `<link rel="stylesheet" href="style.css">`,
  and make sure the file is committed.
- **Changes not appearing**: wait a minute and refresh.

For more advanced sites, GitHub Pages also supports [Jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll)
and custom domains. See the [tutorial](https://github.com/UCDglobalchange/GitHub-website) for links.
