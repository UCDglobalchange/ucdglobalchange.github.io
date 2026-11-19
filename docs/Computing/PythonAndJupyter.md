# Python and Jupyter on FARM

## Conda environments

FARM provides a central conda installation. Create your **own** environment rather than
installing into the shared one, and keep environments **out of your 20 GB home directory**: put
them in your folder on group storage, `/quobyte/emoniergrp/<you>/envs/`.

```bash
# run inside an interactive job, not on the login node:
srun --account=moniergrp --partition=high --time=1:00:00 --cpus-per-task=2 --mem=8G --pty /bin/bash -l

module load conda
mamba create -p /quobyte/emoniergrp/<you>/envs/<your-env> -c conda-forge \
      python=3.12 numpy pandas xarray netcdf4 dask matplotlib cartopy ipykernel
mamba activate /quobyte/emoniergrp/<you>/envs/<your-env>
```

`mamba` does everything `conda` does but much faster.

Put the package cache on group storage too, so it doesn't fill your home. In `~/.condarc`:

```yaml
pkgs_dirs:
  - /quobyte/emoniergrp/<you>/conda-pkgs
```

```{note}
Never `pip install` into a central environment. Activate your own environment first, then use
`mamba install` (preferred) or `pip install`.
```

Save your environment with your project so others can recreate it:

```bash
mamba env export --from-history > environment.yml
```

## JupyterLab through Open OnDemand

[Open OnDemand](https://ondemand.farm.hpc.ucdavis.edu) lets you use FARM from a web browser: browse
files, and launch **JupyterLab** or **RStudio** as Slurm jobs. Apps keep running when you close the
browser, and you can reconnect later. Docs: <https://docs.hpc.ucdavis.edu/software/ondemand/>.

To use your own environment in OnDemand's JupyterLab, register it as a kernel once:

```bash
module load conda
mamba activate /quobyte/emoniergrp/<you>/envs/<your-env>
python -m ipykernel install --user --name <your-env>
```

Then, to start a session:

1. Log in at <https://ondemand.farm.hpc.ucdavis.edu> with your UC Davis credentials.
2. Choose **Interactive Apps → JupyterLab**.
3. Fill in the form: account **`moniergrp`**, a partition (`low` by default, `high` if you can't
   afford to be interrupted, `bml` for very large memory), CPUs, memory, and hours.
4. Click **Launch**, wait for the job to start, then **Connect**.
5. Pick your environment's kernel. Packages you add to that environment will be available.

When you're finished, **delete the session** in OnDemand so you don't hold resources. RStudio works
the same way.

## The climate-data Python stack

| Package | Use |
|---------|-----|
| `xarray`, `netCDF4`, `h5netcdf` | Labeled N-D arrays and NetCDF I/O |
| `dask` | Parallel / out-of-core computation with xarray |
| `cartopy`, `matplotlib` | Maps and figures |
| `xesmf` | Regridding |
| `cftime`, `pandas` | Model calendars and time series |
| `intake-esm` | Catalogs of CMIP-style data |

Command-line tools are available as modules: `module load cdo` for **CDO**, `module load nco` for **NCO**, and `module load netcdf-c` for **`ncdump`** (which doesn't work without it).

<!-- TODO: link to a shared lab environment.yml. -->
