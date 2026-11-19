# Data Transfer and Storage

## Moving data to and from FARM

FARM accepts SSH keys only, so every transfer tool must use the same private key you use to log in.
**Run these commands on your laptop**, not on FARM (outbound `scp` from FARM is disabled).

```bash
# laptop → FARM
scp -r local-directory <you>@farm.hpc.ucdavis.edu:/quobyte/emoniergrp/<you>/

# FARM → laptop
scp -r <you>@farm.hpc.ucdavis.edu:/quobyte/emoniergrp/<you>/results ./

# rsync: resumable — if the connection drops, rerun the same command
rsync -aP <you>@farm.hpc.ucdavis.edu:/quobyte/emoniergrp/<you>/results/ ./results/
```

With `rsync`, a trailing slash on the source (`results/`) means "the contents of" the folder.
Avoid `--delete` unless you deliberately want a mirror: it permanently removes files on the
destination.

**Graphical tools:** [Cyberduck](https://cyberduck.io) (macOS/Windows),
[FileZilla](https://filezilla-project.org), [WinSCP](https://winscp.net) (Windows).

**Large transfers:** use [Globus](https://docs.hpc.ucdavis.edu/data-transfer/) (FARM collection:
*UC Davis Farm home*).

**Box:** `module load rclone` and configure a Box remote; instructions are at
<https://docs.hpc.ucdavis.edu/data-transfer/>.

**Downloading public datasets** (CMIP, ERA5, etc.): do it from an interactive or batch job, not the
login node.

## Where to store data

```{important}
Your **home directory is limited to 20 GB**. Use it only for configuration files (`.bashrc`,
`.ssh`, `.condarc`). Scripts, conda environments, and data go on the lab's group storage,
`/quobyte/emoniergrp/`.
```

- **Your own work** goes in a folder named after your username: `/quobyte/emoniergrp/<username>/`.
  Keep your scripts, conda environments, and user-specific data there. Create it on your first login
  with `mkdir /quobyte/emoniergrp/$USER`.
- **Shared data** that other lab members may use goes in a **dedicated folder** directly under
  `/quobyte/emoniergrp/`, e.g. `/quobyte/emoniergrp/ERA5/`.

```text
/quobyte/emoniergrp/
├── README              # catalog of shared datasets (keep it updated!)
├── ERA5/               # example shared dataset (one dedicated folder per dataset)
├── CMIP6/              # example shared dataset
├── <username-1>/       # personal folders: scripts, envs, user-specific data
└── <username-2>/
```

When you add a shared dataset, document it in the `README` file in `/quobyte/emoniergrp/`:

- dataset name and **version**,
- link to the source repository/website,
- temporal and spatial **resolution**, spatial **extent**,
- **variables** included,
- who downloaded it and when.

Let the lab know about it too.

Check space before large writes:

```bash
qinfo quota /quobyte/emoniergrp      # group quota and usage
du -sh /quobyte/emoniergrp/$USER     # your usage
du -sh ~                             # home usage (20 GB limit)
```

```{warning}
`/quobyte` has a **one-writer-per-file rule**: never let two jobs or array tasks write to the same
file at the same time. Put `%j` (or `%A_%a`) in log file names and give each task its own output file.
```

Make shared files readable by lab mates: `chmod -R g+rX <dir>` (never `chmod 777`).

## Backups and long-term storage

```{danger}
FARM has **no backups**, and storage hardware past its five-year life is unsupported.
```

- **Code**: always in GitHub ([Git and GitHub](../CodingAndData/GitAndGithub.md)).
- **Irreplaceable data** (processed results, model configurations, key inputs that can't be
  downloaded again): keep a second copy in the **lab Box folder**, using `rclone` from FARM (see
  [below](#backing-up-to-box)).
- **Published data**: deposit in a public repository with a DOI (Zenodo, Dryad); see
  [Data Management](../CodingAndData/DataManagement.md).
- Compress before archiving (e.g., NetCDF4 compression or `tar czf`).
- When you leave the lab, hand over or archive your `/quobyte/emoniergrp/<username>/` folder with the PI.

## Backing up to Box

The lab Box folder is the long-term home for copies of data that can't be regenerated. To copy
data from FARM to Box with `rclone`:

1. **Set up once.** Configure a Box remote following the HPC instructions at
   <https://docs.hpc.ucdavis.edu/data-transfer/>. Box sign-in needs a web browser, so the
   instructions use an Open OnDemand desktop session. Name the remote `box`.
2. **Copy** (run it in an interactive job, `srun ... --pty /bin/bash -l`, for anything large):

   ```bash
   module load rclone
   rclone copy /quobyte/emoniergrp/<you>/results  box:<lab-folder>/<you>/results --progress
   ```

3. **Check** what's there: `rclone ls box:<lab-folder>/<you>/results`.

<!-- TODO: name the lab Box folder and its folder structure. -->

`rclone copy` only adds and updates files. Avoid `rclone sync` unless you mean to make the
destination an exact mirror: it **deletes** files on Box that aren't on FARM.

Compress large collections of small files before uploading (`tar czf results.tar.gz results/`):
Box handles a few large files much better than many small ones.

