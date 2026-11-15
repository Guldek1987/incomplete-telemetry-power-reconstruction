# Data

The study uses four public household electricity datasets. The raw releases are not redistributed here because of their size (the two primary archives alone exceed the GitHub file limit). This folder contains the small descriptor files that define how the raw data are interpreted and split.

## Files in this folder

| File | Content |
|---|---|
| `dataset_manifest.csv` | Source URL, DOI, licence, size in bytes and SHA-256 of every Plegma and REFIT file used |
| `data_sources.json` | Landing pages and direct file endpoints of the Plegma and REFIT releases |
| `sensor_semantic_map.csv` | Metadata-derived mapping of every original Plegma channel to a canonical sensor type, unit and role |
| `sensor_schema.json` | Canonical sensor vocabulary used by all models |
| `split_manifest.csv` | Per-house calendar boundaries of the development, calibration, temporal hold-out and group hold-out partitions |
| `fold_manifest.csv` | Boundaries of the three expanding-window development folds (training, inner validation, outer validation) |

## Raw releases to download

| Dataset | Role in the study | Source | Licence | Files |
|---|---|---|---|---|
| Plegma | Development, calibration, temporal and unseen-house hold-out | [doi:10.15129/3b01a6c6-2efd-424a-b8b8-5fe7fa445ded](https://doi.org/10.15129/3b01a6c6-2efd-424a-b8b8-5fe7fa445ded) | CC BY 4.0 | `PlegmaDataset_Raw.7z` (783,216,452 bytes), `PlegmaDataset_README_20240326.txt` |
| REFIT | External zero-shot evaluation | [doi:10.15129/31da3ece-f902-4e95-a093-e0a9536983c4](https://doi.org/10.15129/31da3ece-f902-4e95-a093-e0a9536983c4) | CC BY 4.0 | `REFIT_RAW_081116.7z` (698,108,465 bytes), `RAW_READ_ME_081116.txt`, `MetaData_Tables.xlsx` |
| UK-DALE (2017 release) | External evaluation on measured active power | [UKERC Energy Data Centre](http://data.ukedc.rl.ac.uk/cgi-bin/dataset_catalogue/view.cgi.py?id=19); descriptor [doi:10.1038/sdata.2015.7](https://doi.org/10.1038/sdata.2015.7) | CC BY 4.0 | 1-second and 6-second house data with metadata |
| ECO | Independent zero-shot evaluation (six households) | [doi:10.5905/ethz-1007-35](https://doi.org/10.5905/ethz-1007-35); [project page](https://vs.inf.ethz.ch/res/show.html?what=eco-data) | see the ECO project page | `0N_sm_csv.zip`, `0N_plugs_csv.zip` for N = 1…6 |

SHA-256 values of the Plegma and REFIT files are listed in `dataset_manifest.csv`; compare them after download. The publishers did not supply checksums, so these values identify the copies used in this study.

## Expected local layout

The notebooks address data relative to the repository root:

```text
Data/Raw/Plegma/PlegmaDataset_Raw.7z
Data/Raw/REFIT/REFIT_RAW_081116.7z
Data/Raw/REFIT/MetaData_Tables.xlsx
Data/Interim/Plegma_extracted/Raw_Dataset/House_01 … House_13
Data/Interim/REFIT_extracted/
Data/Acquisition/ECO/archives/0N_sm_csv.zip, 0N_plugs_csv.zip
Data/Raw/UKDALE_2017/
```

Extract the Plegma and REFIT archives into the matching `Data/Interim/*_extracted/` folders. Notebook 01 starts from these extracted files and builds every later intermediate product (minute-level tables, split identifiers, causal features).

## Measurement notes

- Plegma reports active power in W at a nominal 10-second cadence; all analyses use 1-minute means in kW. No target value is interpolated, clipped or forward-filled.
- A minute is an eligible target only if the aggregate channel has at least five valid readings in at least five 10-second bins, no within-minute gap longer than 20 s and no out-of-range reading.
- REFIT aggregate readings come from a current clamp with a fixed nominal voltage and the release contains collection-time filling. REFIT results therefore describe transfer to nominal power readings, not to independently measured active power.
- In Plegma House 8 the `ac_1` and `dishwasher` streams are identical; they are treated as one channel.
