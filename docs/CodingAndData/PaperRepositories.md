# Project and Paper Repositories

Every paper should be reproducible from its GitHub repository: someone (including future you)
should be able to regenerate every figure and table.

## Standard layout

```text
project-name/
├── README.md        # what this is, how to reproduce results, where the data live
├── environment.yml  # conda environment (mamba env export --from-history)
├── LICENSE          # MIT recommended
├── .gitignore       # exclude data, output, logs
├── data/            # small inputs only; large data documented in README, not committed
├── scripts/         # processing and analysis code (numbered in run order helps: 01_, 02_, …)
├── slurm/           # job scripts used on FARM
├── notebooks/       # exploratory notebooks
├── figures/         # generated figures (usually not committed)
├── results/         # generated output (not committed)
└── docs/            # notes and documentation
```

Adjust to your project, but be consistent and document it.

## The README should say

- What the project does and the associated paper (citation/DOI when available).
- How to create the environment.
- Where the input data come from (source, version, path on FARM such as `/quobyte/emoniergrp/<dataset>/...`).
- The order to run scripts and which script makes which figure.

## Good practices

- Don't hard-code paths deep in scripts; put them in one config file.
- Fix random seeds for anything stochastic.
- Make figures from scripts, not by hand.
- Record model versions, configuration files, and run scripts for every simulation.

## Archiving at publication

1. Clean up the repository and make it public (discuss with the PI).
2. Connect the repository to [Zenodo](https://zenodo.org) and create a release to get a **DOI**.
3. Cite the code DOI in the paper's *Code and Data Availability* statement.

<!-- TODO: list lab paper repositories here as they are published. -->
