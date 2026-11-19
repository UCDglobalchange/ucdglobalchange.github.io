# Git and GitHub

All lab code lives under version control in the lab GitHub organization,
**[UCDglobalchange](https://github.com/UCDglobalchange)**. This page summarizes the lab workflow;
the full step-by-step tutorial is in the
[lab GitHub repo](https://github.com/UCDglobalchange/GitHub).

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

## 4. Create or clone a repository

Create new repositories **inside the UCDglobalchange organization** (choose Private or Public, and
tick *Add a README*). Then:

```bash
git clone git@github.com:UCDglobalchange/<repo>.git
cd <repo>
```

To connect an existing folder of code instead:

```bash
git init
git remote add origin git@github.com:UCDglobalchange/<repo>.git
```

## 5. Daily workflow

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
- **Never commit large data or model output.** Add a `.gitignore` (start from GitHub's
  [Python template](https://github.com/github/gitignore/blob/main/Python.gitignore)) with entries
  like `*.nc`, `*.log`, `__pycache__/`.
- Add a `LICENSE` (MIT is recommended for academic code).
- Commit early and often, with clear messages.

## Learning resources

- Software Carpentry, *Version Control with Git*: <https://swcarpentry.github.io/git-novice/>
- GitHub Docs: <https://docs.github.com>
- *Pro Git* (free book): <https://git-scm.com/book>
