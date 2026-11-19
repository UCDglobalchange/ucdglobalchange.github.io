# Getting Started with FARM

Most lab computing (climate model runs, large-ensemble analysis, big NetCDF datasets) happens on
**FARM**, the Linux supercomputing cluster run by the College of Agricultural and Environmental
Sciences and supported by UC Davis HPC Core Facility (HPCCF).

```{admonition} Lab quick reference
:class: tip
| | |
|--|--|
| Login | `ssh <your-kerberos-id>@farm.hpc.ucdavis.edu` |
| Lab group storage | `/quobyte/emoniergrp/` (your folder: `/quobyte/emoniergrp/<username>/`) |
| Slurm account | `--account=moniergrp` |
| Partitions | `low` by default · `high` when a job needs high priority · `bml` for very large memory |
| Web interface | [Open OnDemand](https://ondemand.farm.hpc.ucdavis.edu) |
| Help | `farm-hpc@ucdavis.edu` · <https://docs.hpc.ucdavis.edu/farm/> |
```

## What is a cluster?

A cluster is many computers (*nodes*) shared by many users. You log into a shared **login node**,
and the **Slurm** scheduler runs your work on **compute nodes**.

```{warning}
**Never run computations on the login node.** Use it only to edit files, move data, and submit
jobs. Anything that takes more than a few seconds of CPU (analysis, compiling, large `conda`
installs) must run in a Slurm job ([Submitting Jobs](SubmittingJobs.md)). Jobs found running on
the login node are killed and your account can be suspended.
```

## 1. Create an SSH key

FARM only accepts **SSH keys** (no passwords). On your laptop:

```bash
ssh-keygen -t ed25519 -C "your_email@ucdavis.edu"
cat ~/.ssh/id_ed25519.pub        # this is the PUBLIC key you will paste into HiPPO
```

Protect the private key (`~/.ssh/id_ed25519`) with a passphrase and never share it. If you lose it
or forget the passphrase, it cannot be recovered: generate a new pair and update HiPPO. More:
<https://docs.hpc.ucdavis.edu>.

## 2. Request an account

1. Go to **HiPPO**: <https://hippo.ucdavis.edu/Farm>.
2. Choose the lab's group/PI as your sponsor and paste your **public** key.
3. The PI approves the request; you'll get an email when your account is ready, listing the
   partitions you can use.

## 3. Log in

You need a terminal program that can use SSH keys:

- **macOS**: the built-in [Terminal](https://support.apple.com/guide/terminal/welcome/mac) or
  [iTerm2](https://iterm2.com).
- **Windows**: [Windows Terminal](https://aka.ms/terminal) with OpenSSH or
  [WSL](https://learn.microsoft.com/windows/wsl/install) (then follow the Linux/macOS steps), or
  [MobaXterm](https://mobaxterm.mobatek.net), an all-in-one terminal that can also edit files
  locally and upload changes back to FARM. [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/)
  also works.
- **Linux**: any terminal.

FARM supports **one key per user**. To log in from several computers (e.g., a desktop and a
laptop), copy your private key to each one rather than creating new keys.

```bash
ssh <your-kerberos-id>@farm.hpc.ucdavis.edu
```

Add a shortcut to `~/.ssh/config` on your laptop so that `ssh farm` just works:

```text
Host farm
    HostName farm.hpc.ucdavis.edu
    User <your-kerberos-id>
    IdentityFile ~/.ssh/id_ed25519
```

At login, FARM prints a table of your Slurm accounts, partitions, and limits. To see it again:

```bash
sacctmgr show assoc user=$USER format=account%20,partition%20,qos%40
```

## 4. Know where to put things

| Location | Size | Use it for |
|----------|------|-----------|
| `~` (home) | **20 GB only** | Configuration files (`.bashrc`, `.ssh`, `.condarc`). **Not** scripts, data, or conda environments. |
| `/quobyte/emoniergrp/<username>/` | Lab quota | **Your** folder: your scripts, conda environments, and user-specific data. Create it on your first login. |
| `/quobyte/emoniergrp/<dataset>/` | Lab quota | **Shared** datasets used by several people, each in its own dedicated folder. |
| `/scratch` / `$TMPDIR` on a compute node | ~1.8 TB per node | Fast temporary space during a job; **deleted when the job ends**. |

On your first login, create your personal folder:

```bash
mkdir /quobyte/emoniergrp/$USER
```

```{warning}
`/quobyte` has a **one-writer-per-file rule**: never let two jobs (or two array tasks) write to the
same file at the same time. It can hang compute nodes and get accounts locked. Always put the job
ID in log names (`%j`, or `%A_%a` for arrays) and give every task its own output file.
```

```{danger}
**Nothing on FARM is backed up.** Keep code in GitHub and keep a copy of anything irreplaceable
elsewhere (see [Data Transfer and Storage](DataTransferAndStorage.md)).
```

## 5. Software modules

Most software on FARM is available as **environment modules**. Loading a module adjusts your
environment (e.g., your `PATH`) so the software and its dependencies work, and unloading it undoes
that, which keeps environments reproducible and avoids conflicts.

```bash
module avail                 # list ALL available modules
module avail netcdf          # search for a module
module load conda            # load a module (default version)
module load cdo/<version>    # load a specific version
module list                  # what you have loaded
module whatis <name>         # short description
module help <name>           # more detailed help
module unload <name>         # unload one module
module purge                 # unload everything
```

Several versions of a package are often available; `module load <name>` loads the default, and
`module load <name>/<version>` picks a specific one. Load as few modules as you need at a time, and
unload a module when you're done with it, to avoid incompatibilities. Full help: `man module`.

If you find a broken module, email `farm-hpc@ucdavis.edu` with your username, the cluster, the
directory, the exact command, and the full error output.

If something you need is missing, email `farm-hpc@ucdavis.edu` with the software name, a link to
it, and your username.

## Next steps

- [Submitting Jobs on FARM](SubmittingJobs.md)
- [Data Transfer and Storage](DataTransferAndStorage.md)
- [Python and Jupyter on FARM](PythonAndJupyter.md)
- New to the command line? [Introduction to Linux](IntroToLinux.md)
