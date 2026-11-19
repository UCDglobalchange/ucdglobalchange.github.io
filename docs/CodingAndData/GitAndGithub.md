# Git and GitHub

All lab code lives under version control in the lab GitHub organization,
**[UCDglobalchange](https://github.com/UCDglobalchange)**. This tutorial walks you through setting
up a GitHub account and Git, creating a repository, contributing code, and staying in sync when
collaborating, whether you work on FARM or on your own computer.

## 1. Create an account and join the organization

1. Create a GitHub account at <https://github.com> (if you don't have one).
2. Send your GitHub username to the PI or lab manager to be invited to **UCDglobalchange**.
3. Accept the invitation by email or at <https://github.com/notifications>.

## 2. Set up Git

Git is already installed on FARM. On your laptop: `brew install git` (macOS) or
`sudo apt install git` (Linux).

```bash
git config --global user.name "Your Name"
git config --global user.email "your_email@ucdavis.edu"
```

## 3. Connect FARM (and your laptop) to GitHub with SSH

```bash
ls ~/.ssh                                  # look for id_ed25519(.pub)
ssh-keygen -t ed25519 -C "your_email@ucdavis.edu"   # only if you don't have one
cat ~/.ssh/id_ed25519.pub                  # copy this
```

Add it at <https://github.com/settings/keys> (**New SSH key**, name it e.g. "FARM"), then test:

```bash
ssh -T git@github.com    # "Hi <username>! You've successfully authenticated..."
```

## 4. Create a repository on GitHub

Create new project repositories **inside the UCDglobalchange organization**:

1. Log in to <https://github.com> and go to the
   [UCDglobalchange](https://github.com/UCDglobalchange) organization.
2. Click **New** (or the **+** icon in the upper right → **New repository**) and make sure the
   **Owner** is `UCDglobalchange`.
3. Fill in the details:
   - **Repository name**, e.g. `my-project` (short, lowercase, hyphens);
   - a short **description**;
   - **Private** or **Public**, depending on who needs access (discuss with the PI);
   - ✅ **Add a README file**.
4. Click **Create repository**.

## 5. Clone the repository to FARM (or your computer)

```bash
git clone git@github.com:UCDglobalchange/my-project.git
cd my-project
```

To add code you already have, copy it into the repository folder:

```bash
cp -r /path/to/your/code/* .
```

Or, to connect an existing folder that isn't a Git repository yet:

```bash
cd /path/to/your/code
git init
git remote add origin git@github.com:UCDglobalchange/my-project.git
git pull origin main          # bring in the README created on GitHub
```

Then commit and push your code for the first time:

```bash
git add .
git commit -m "Initial commit of preprocessing scripts"
git push origin main
```

## 6. Daily workflow

Which workflow you use depends on who works in the repository:

| Repository | Workflow |
|------------|----------|
| **Your own** repositories (your project or paper code) | Commit and push directly to `main`, as below. |
| **Shared** repositories (e.g., the [GDSTEM and AquaCrop](../Onboarding/LabModels.md) repos) | Work on a **branch** and open a **pull request** for review by the PI or the repository owner ([below](#shared-repositories-branches-and-pull-requests)). Never push directly to `main`. |

In your own repositories:

```bash
git pull origin main --rebase      # 1. get the latest (start of every session, and before pushing)
# 2. ...edit files...
git add <files>                    # 3. stage
git commit -m "Short description of what changed and why"   # 4. commit
git push origin main               # 5. push to GitHub
```

```{tip}
Pull at the start of each work session and always before pushing. This avoids most conflicts
when you work from both FARM and your laptop or with collaborators.
```

## Shared repositories: branches and pull requests

In repositories shared by several people, every change goes through a pull request (PR), so that
`main` always works and someone else reviews each change.

```bash
git checkout main
git pull origin main --rebase      # start from the latest main
git checkout -b short-description  # create a branch for your change
# ...edit, add, and commit as usual...
git push -u origin short-description
```

Then on GitHub:

1. Click **Compare & pull request**.
2. Describe what changed, why, and how you tested it.
3. Request a review from the PI or the repository owner.
4. Address review comments by pushing more commits to the same branch.
5. Once approved, the PR is merged into `main`. Delete the branch afterwards.

Keep PRs small and focused: one fix or feature each. If `main` changed while you were working,
update your branch with `git pull origin main --rebase` and push again (use
`git push --force-with-lease` after a rebase).

You can also use branches in your own repositories to try out experimental changes.

## When things go wrong

**Push rejected** ("Updates were rejected because the remote contains work that you do not have"):
someone else pushed first. Run `git pull origin main --rebase`, then push again.

**Merge conflict**: Git marks the conflicting lines:

```text
<<<<<<< HEAD
your local changes
=======
changes from GitHub
>>>>>>> origin/main
```

Edit the file to keep what you want, then `git add <file>` and `git rebase --continue` (or
`git commit` if you were merging).

**Start over to match GitHub exactly** (⚠️ permanently deletes uncommitted local changes):

```bash
git fetch origin
git reset --hard origin/main
git clean -fd
```

## Lab conventions

- One repository per project or paper, in the UCDglobalchange organization.
- Push directly to `main` only in your own repositories; use pull requests in shared ones.
- Use the standard [project layout](PaperRepositories.md) and a `README.md`.
- **Never commit large data or model output** (see `.gitignore` below).
- Add a `LICENSE` (MIT is recommended for academic code; see below).
- Commit early and often, with clear messages.

## Add a `.gitignore`

A `.gitignore` file lists files Git should not track, such as large data, model output, compiled
code, temporary logs, and machine-specific settings. Start from GitHub's Python template:

```bash
curl https://raw.githubusercontent.com/github/gitignore/main/Python.gitignore -o .gitignore
```

Then add entries for your project, for example:

```text
*.nc
*.log
__pycache__/
temp/
results/
```

Commit it like any other file:

```bash
git add .gitignore
git commit -m "Add .gitignore"
git push origin main
```

## Add a license

A license tells others how they may reuse your code. MIT is recommended for academic code. The
easiest way to add one is on GitHub: **Add file → Create new file**, name it `LICENSE`, click
**Choose a license template**, pick **MIT License**, and commit. Then run `git pull` to get it
locally. You can also choose a license when you create the repository.

## Learning resources

- Software Carpentry, *Version Control with Git*: <https://swcarpentry.github.io/git-novice/>
- GitHub Docs: <https://docs.github.com>
- *Pro Git* (free book): <https://git-scm.com/book>
