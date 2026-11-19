# Software Onboarding

This page lists the software to install on your own computer. Cluster-side setup is covered in
[Computing on FARM](../Computing/GettingStartedWithFARM.md).

## Core tools

| Tool | What for | Get it |
|------|----------|--------|
| Terminal + SSH | Connecting to the cluster | Built in on macOS/Linux; Windows: [WSL](https://learn.microsoft.com/windows/wsl/) or Windows Terminal |
| Git | Version control | <https://git-scm.com> (macOS: `xcode-select --install`) |
| Python (via Miniforge) | Analysis and modeling | <https://github.com/conda-forge/miniforge> |
| VS Code | Code editor with remote-SSH support | <https://code.visualstudio.com> |
| Zotero | Reference management | <https://www.zotero.org> |
| Overleaf / LaTeX | Writing papers | <https://www.overleaf.com> |
| CDO and NCO | Processing NetCDF climate data from the command line | See [below](#cdo-and-nco) |
| Slack | Lab communication | <https://slack.com/downloads> |
| Box Drive | Lab shared files | <https://www.box.com/drive> |

The lab's own models, **GDSTEM** and **AquaCrop**, have a page of their own:
[Lab Models](LabModels.md).

## Python setup

Install Miniforge, then create a lab environment rather than installing packages into `base`:

```bash
mamba create -n gecl python=3.12 numpy pandas xarray netcdf4 dask matplotlib cartopy jupyterlab
mamba activate gecl
```

See [Python and Jupyter on FARM](../Computing/PythonAndJupyter.md) for using environments on the cluster.

## CDO and NCO

[CDO](https://code.mpimet.mpg.de/projects/cdo) (Climate Data Operators) and
[NCO](https://nco.sourceforge.net) (NetCDF Operators) are fast command-line tools for subsetting,
averaging, regridding, and combining NetCDF files. They are the quickest way to explore and
preprocess model output and climate datasets.

- **On FARM**: always use the modules; don't install CDO or NCO in a conda environment. Load each
  tool's module before using it (in an interactive session or in your job script):

  ```bash
  module load cdo        # CDO
  module load nco        # NCO (ncks, ncrcat, ncap2, ...)
  module load netcdf-c   # ncdump and ncgen
  ```

  ```{important}
  `ncdump` does **not** work on FARM until you run `module load netcdf-c`.
  ```

- **On your laptop**: `mamba install -c conda-forge cdo nco` (or `brew install cdo nco` on macOS).

A few common commands:

```bash
ncdump -h file.nc                          # show a file's header: dimensions, variables, attributes
cdo sinfon file.nc                         # summary of variables, grid, and time steps
cdo yearmean in.nc out_annual.nc           # annual means
cdo sellonlatbox,-125,-114,32,42 in.nc ca.nc   # subset a lon/lat box (here, California)
cdo fldmean in.nc out_fldmean.nc           # area-weighted spatial mean
ncks -v tas in.nc tas.nc                   # extract one variable
ncrcat file_19*.nc file_20*.nc merged.nc   # concatenate along time
```

Learn more: the [CDO user guide](https://code.mpimet.mpg.de/projects/cdo/wiki/Cdo#Documentation)
and the [NCO documentation](https://nco.sourceforge.net/nco.html).

## SSH keys

Create an SSH key once and use it for both GitHub and the cluster:

```bash
ssh-keygen -t ed25519 -C "your_email@ucdavis.edu"
cat ~/.ssh/id_ed25519.pub   # paste this into GitHub and HiPPO (for FARM)
```

## VS Code on the cluster

The *Remote – SSH* extension lets you edit files on the cluster as if they were local. Only use it
for editing; run heavy computations inside Slurm jobs, never on the login node.
