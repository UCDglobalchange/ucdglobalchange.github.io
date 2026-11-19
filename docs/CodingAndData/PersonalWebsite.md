# Your Personal Website

A simple personal website helps other researchers, collaborators, and future employers find you
and your work. This beginner-friendly tutorial shows you how to build one and host it for free on
**GitHub Pages** at:

```text
https://<yourusername>.github.io
```

**You'll need:** a GitHub account, a web browser, and the ability to edit text files.

## Step 1: Create your repository

1. Go to <https://github.com/new>.
2. Name the repository exactly `<yourusername>.github.io`, replacing `<yourusername>` with your
   GitHub username. Create it in your **personal** account, not the lab organization.
3. Set it to **Public**.
4. Tick **Add a README file**.
5. Click **Create repository**.

Official guide:
[Creating a GitHub Pages site](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site).

## Step 2: Create your homepage

In the repository, click **Add file → Create new file**, name it `index.html`, paste this
starter code, edit it with your own information, and commit:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Your Name</title>

  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 800px;
      margin: 40px auto;
      padding: 0 20px;
      line-height: 1.6;
      color: #222;
    }

    h1 { margin-bottom: 0.2em; }

    .subtitle {
      color: #666;
      margin-top: 0;
    }

    .card {
      margin-top: 2rem;
      padding: 1rem 1.25rem;
      border: 1px solid #ddd;
      border-radius: 10px;
    }

    a {
      color: #0b57d0;
      text-decoration: none;
    }

    a:hover {
      text-decoration: underline;
    }
  </style>
</head>

<body>
  <h1>Your Name</h1>
  <p class="subtitle">Global Environmental Change Lab · UC Davis</p>

  <div class="card">
    <h2>About Me</h2>
    <p>
      Welcome to my personal website. This is where I share my work,
      publications, and projects.
    </p>
  </div>

  <div class="card">
    <h2>Projects</h2>
    <p>Coming soon.</p>
  </div>

  <div class="card">
    <h2>Contact</h2>
    <p>Email: your.email@ucdavis.edu</p>
  </div>
</body>
</html>
```

## Step 3: Turn on GitHub Pages

1. In your repository, click **Settings**.
2. Click **Pages** in the left sidebar.
3. Under **Build and deployment**, set:
   - **Source**: *Deploy from a branch*
   - **Branch**: `main`
   - **Folder**: `/ (root)`
4. Click **Save**.

Docs: [GitHub Pages quickstart](https://docs.github.com/en/pages/getting-started-with-github-pages/quickstart).

## Step 4: View your website

After a minute or so, your site is live at `https://<yourusername>.github.io`.

## Step 5: Organize your files

As the site grows, split it into several pages and a shared stylesheet:

```text
<yourusername>.github.io/
├── index.html
├── about.html
├── projects.html
├── style.css
└── images/
```

Move the CSS from `index.html` into `style.css`, for example:

```css
body {
  font-family: Arial, sans-serif;
  max-width: 900px;
  margin: 40px auto;
  padding: 0 20px;
}

.card {
  margin-top: 24px;
  padding: 16px;
  border: 1px solid #ddd;
  border-radius: 10px;
}
```

Then replace the `<style>…</style>` block in each page's `<head>` with:

```html
<link rel="stylesheet" href="style.css">
```

## Step 6: Add navigation

Put the same menu at the top of every page's `<body>`:

```html
<nav>
  <a href="index.html">Home</a>
  <a href="about.html">About</a>
  <a href="projects.html">Projects</a>
</nav>
```

## What to include

- A photo and a short bio, mentioning that you're in the
  **Global Environmental Change Lab at UC Davis**.
- Your research interests and projects.
- Your CV (PDF) and publications.
- Teaching materials, if any.
- Links to GitHub, Google Scholar, ORCID, and LinkedIn.
- Contact information.

## Optional extras

- **Jekyll**: GitHub Pages can build more advanced sites with templates and blog posts using
  [Jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll). Plain HTML
  is simpler for beginners.
- **Custom domain**: you can use your own domain, like `www.yourname.com`; see
  [Configuring a custom domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site).

## Troubleshooting

**404 error**

- The repository name isn't exactly `<yourusername>.github.io`.
- `index.html` is missing (check the spelling and that it's in the root folder).
- GitHub Pages isn't enabled, or the wrong branch is selected.

More: [Troubleshooting 404 errors](https://docs.github.com/en/pages/troubleshooting-pages-sites/troubleshooting-404-errors-for-github-pages-sites).

**Styles not showing**

- Check the spelling of `style.css` and the path in the `<link>` tag.
- Make sure the file is committed.

**Site not updating**

- Wait about a minute, then refresh (a hard refresh, Cmd/Ctrl + Shift + R, bypasses the cache).
- Check the Pages settings.
