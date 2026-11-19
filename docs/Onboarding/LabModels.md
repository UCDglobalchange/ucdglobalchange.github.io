# Lab Models: GDSTEM and AquaCrop

The lab develops and maintains two models that many lab projects use. New members working on
these projects should read this page early, then ask the PI which repositories and training are
relevant to their project.

```{note}
Many of the repositories below are **private** to the
[UCDglobalchange](https://github.com/UCDglobalchange) organization. You will see them once you
have joined the organization ([Git and GitHub](../CodingAndData/GitAndGithub.md)).
```

## GDSTEM

**GDSTEM**, the Global Dynamical and Structural Terrestrial Ecosystem Model, is the lab's
terrestrial ecosystem model. It was used for the TRENDYv14 simulations, part of the Global Carbon
Budget 2025.

🌐 **Website:** <https://ucdglobalchange.github.io/gdstem/>

| Repository | What it contains |
|------------|------------------|
| [gdstem-trendyv14](https://github.com/UCDglobalchange/gdstem-trendyv14) | Model source code used for the TRENDYv14 simulations |
| [gdstem-preprocessing](https://github.com/UCDglobalchange/gdstem-preprocessing) | Preparing model inputs, including land-use change and climate input files |
| [gdstem-postprocessing](https://github.com/UCDglobalchange/gdstem-postprocessing) | Converting model output to NetCDF following the TRENDY protocol |
| [gdstem-analysis](https://github.com/UCDglobalchange/gdstem-analysis) | Analyzing model inputs and outputs |
| [GDSTEM-analysis-R](https://github.com/UCDglobalchange/GDSTEM-analysis-R) | Analysis of GDSTEM data in R |
| [gdstem-calibration](https://github.com/UCDglobalchange/gdstem-calibration) | Automating the model calibration |
| [gdstem-bayesian-calibration](https://github.com/UCDglobalchange/gdstem-bayesian-calibration) | Bayesian calibration workflow using FLUXNET tower sites |
| [gdstem-calibration-data](https://github.com/UCDglobalchange/gdstem-calibration-data) | High-resolution products sampled to tower tables for calibration |
| [gdstem](https://github.com/UCDglobalchange/gdstem) | Source of the GDSTEM website |

<!-- TODO: add a recommended learning path for GDSTEM (key papers, first exercise, how to run a test simulation on FARM). -->

## AquaCrop

**AquaCrop** is a crop-growth model. The lab maintains a **Python-based implementation of AquaCrop**
and a calibration framework that works from field to global scales, including fertilizer-response
and sensitivity-analysis developments.

🌐 **Website:** <https://ucdglobalchange.github.io/aquacrop/>

| Repository | What it contains |
|------------|------------------|
| [AquaCrop_Python](https://github.com/UCDglobalchange/AquaCrop_Python) | The Python AquaCrop simulation pipeline, plus calibration and sensitivity-analysis pipelines (Sobol/Saltelli samples run as Slurm array jobs) |
| [AquaCrop_GGCMI](https://github.com/UCDglobalchange/AquaCrop_GGCMI) | AquaCrop-Python code and inputs for GGCMI simulations |
| [aquacrop](https://github.com/UCDglobalchange/aquacrop) | Source of the AquaCrop website |

<!-- TODO: add a recommended learning path for AquaCrop (key papers, first exercise, running a test case on FARM). -->

## Running the models on FARM

Both models are run on FARM. Before running them, work through
[Computing on FARM](../Computing/GettingStartedWithFARM.md), and follow the README of each
repository. Keep model code in your folder under `/quobyte/emoniergrp/<username>/`, and keep large
inputs shared by several people in a dedicated folder under `/quobyte/emoniergrp/`.
