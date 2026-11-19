# Data Management

Climate research produces a lot of data. A little discipline up front saves a lot of pain.

## Principles

- **Raw data are read-only.** Never modify downloaded data in place; write processed output
  elsewhere.
- **Document provenance**: source, version, download date, and processing steps.
- **Share, don't duplicate**: datasets others may use go in a dedicated folder under `/quobyte/emoniergrp/` and are listed
  in the group `README` (see [Data Transfer and Storage](../Computing/DataTransferAndStorage.md)).
- **Back up what can't be regenerated** to the lab Box folder. FARM has no backups
  ([how to back up to Box](../Computing/DataTransferAndStorage.md#backing-up-to-box)).

## File formats

- **NetCDF4** (with CF conventions) is the standard for gridded climate data. Use internal
  compression (`zlib`, level 1–4) to save a lot of space:
  `ds.to_netcdf("out.nc", encoding={v: {"zlib": True, "complevel": 4} for v in ds.data_vars})`
- **Zarr** works well for very large, chunked, cloud-style datasets.
- **CSV/Parquet** for tabular data (Parquet is smaller and faster).
- Store data at the precision you need (`float32` is usually enough for model output).

## Naming

Use descriptive, sortable, space-free names, e.g.
`tas_CESM2_ssp245_r1i1p1f1_2015-2100_annual.nc` (variable, model, scenario, member, period,
frequency).

## Metadata

Add global attributes to the files you produce (`title`, `source`, `history`, `institution`,
`contact`), and units/long names for every variable.

## Publishing data

Data supporting a paper should be deposited in a public repository with a DOI (e.g., Zenodo,
Dryad), following the journal's data policy. Large model output may need a specialized archive;
discuss with the PI early.

## Where things live

| What | Where |
|------|-------|
| Code | GitHub ([UCDglobalchange](https://github.com/UCDglobalchange)) |
| Working data and model runs | FARM, `/quobyte/emoniergrp/<username>/` |
| Shared datasets | FARM, a dedicated folder under `/quobyte/emoniergrp/`, listed in the group `README` |
| Long-term copies of irreplaceable data | Lab Box folder |
| Published data and code | Zenodo, Dryad, or another public repository, with a DOI |
