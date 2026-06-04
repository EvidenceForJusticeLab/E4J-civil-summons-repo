# Civil Summons Enforcement Project

Repository for the Evidence for Justice Lab's civil summons data cleaning, merging, exploratory analysis, and model-building workflow.

The goal of this project is to create and maintain a reproducible analysis dataset for NYC civil summons research. The repository currently documents a 2021 workflow, with versioned outputs preserved at each stage so future work can be re-created or extended to later years.

## What’s In This Repo

This repository contains:

- raw and semi-processed source files used in the cleaning pipeline,
- notebook-based cleaning workflows for civil summons and officer demographic data,
- analysis notebooks and R markdown for modeling,
- versioned CSV outputs from each major stage,
- archived notebook versions for traceability,
- EDA figures.

## Repository Layout

```text
.
├── 1-cleaning_raw_citations/
│   ├── 2021_raw_RA_Citation_Files/
│   ├── clean_2021_w_Sept.ipynb
│   └── clean_2021_w_Sept.csv
├── 2-cleaning_officer_dem/
│   ├── officer_dem_data/
│   ├── officer_dem_merge.ipynb - Colab.pdf
│   └── cleaned_demographic_merge_w_Sept.csv
├── 3-analysis_EDA/
│   ├── EDA_figures/
│   ├── archived-workbooks/
│   ├── clean_v7_model_6.1.26.ipynb
│   ├── cs_eda_workingcode_4.29.26.ipynb
│   ├── manual-payroll-merge-data/
│   └── payroll-roster-ccrb-data/
├── 4-models/
│   ├── exploring_sept_citations.ipynb
│   ├── model_v8.Rmd
│   └── model_data/
├── extra-data/
└── README.md
```

## Workflow Overview

The project follows a staged pipeline:

1. **Clean raw civil citation files**
2. **Clean and merge officer demographic files**
3. **Build the master analysis dataset**
4. **Run analysis-specific cleaning and modeling**

Each stage writes a dated, versioned output so the previous step can be reproduced and audited.

## Data Cleaning & Maintenance Guide

### 1) Cleaning Raw Civil Citations

Location:

- `1-cleaning_raw_citations/clean_2021_w_Sept.ipynb`
- `1-cleaning_raw_citations/2021_raw_RA_Citation_Files/`
- output: `1-cleaning_raw_citations/clean_2021_w_Sept.csv`

Purpose:

- combine individual raw citation CSVs into one cleaned civil summons file,
- standardize field names and formats,
- document every cleaning decision in the notebook and accompanying notes.

Raw 2021 civil citations from lab members:

- `2021 Civil Citation Data Entry Template JWM_03.20.25.csv`
- `2021 Civil Citation Data Entry Template Johnson_2025.01.30.csv`
- `2021 Civil Citation Data Entry Bagley_2025.02.23.csv`
- `2021 Civil Citation Data Entry Template AG 2025.02.27.csv`
- `2021 Civil Citation Data Entry Template_Baugh_2025.03.07.csv`
- `2021 Civil Citation Data Entry Template_Pozo_2025.03.08.csv`
- `2021_Civil_Summons_Sept.csv`

Recommended reproduction path:

1. Open `1-cleaning_raw_citations/clean_2021_w_Sept.ipynb`.
2. Run cells in order.
3. Verify the notebook writes `clean_2021_w_Sept.csv`.
4. Confirm the output schema matches the documented cleaning notes.

### 2) Cleaning Officer Demographic Data

Location:

- `2-cleaning_officer_dem/`
- `2-cleaning_officer_dem/officer_dem_data/`
- output: `2-cleaning_officer_dem/cleaned_demographic_merge_w_Sept.csv`

Purpose:

- merge officer demographic and disciplinary sources,
- prepare roster-linked officer attributes,
- produce a clean officer-level demographic dataset for downstream joins.

Key source files in the repo:

- `arrest_all.csv`
- `disp_actions5(1).csv`
- `OATH_Hearings_Division_Case_Status_NYPD ONLY (Jan 1 2021 to Aug 31 2022).csv`
- `ar_processed_clean.csv`
- `personnel file(3).csv`
- `allegation_detail_clean.csv`
- `charges_clean2.csv`
- `rs_processed_clean2.csv`
- `disciplinary_history_clean.csv`
- `first_set_updated.csv`

Recommended reproduction path:

1. Open the officer demographic notebook or the Colab export in `2-cleaning_officer_dem/`.
2. Re-run the merge logic using the cleaned source files in `officer_dem_data/`.
3. Save the output as `cleaned_demographic_merge_w_Sept.csv`.
4. Compare the output to the version committed in the repo before replacing anything.

### 3) Building the Master Dataset

Location:

- `3-analysis_EDA/cs_eda_workingcode_4.29.26.ipynb`
- `3-analysis_EDA/archived-workbooks/`
- output history preserved under `4-models/model_data/past_cs_versions/`

Purpose:

- join cleaned civil summons data with officer demographic data,
- create the analysis-ready master dataset,
- preserve intermediate versions for traceability.

Version history in the repo:

- `civil_summons_2021_v1.csv`
- `civil_summons_2021_v2.csv`
- `civil_summons_2021_v3.csv`
- `civil_summons_2021_v4.csv`
- `civil_summons_2021_v5.csv`
- `civil_summons_2021_v6.csv`
- `civil_summons_2021_v7.csv`
- current analysis data used: `4-models/model_data/civil_summons_2021_v8.csv`

Recommended reproduction path:

1. Start from `cleaned_demographic_merge_w_Sept.csv`.
2. Run `3-analysis_EDA/cs_eda_workingcode_4.29.26.ipynb` cell by cell.
3. Use the notebook as the source of truth for column-by-column cleaning decisions.
4. Confirm the notebook generates the expected `civil_summons_2021_v7` intermediate.

### 4) Cleaning for Analysis and Modeling

Location:

- `3-analysis_EDA/clean_v7_model_6.1.26.ipynb`
- `4-models/model_v8.Rmd`
- `4-models/model_data/civil_summons_2021_v8.csv`

Purpose:

- perform analysis-specific cleanup on the master dataset,
- prepare officer-level data for regression and EDA,
- generate the final current analysis dataset used in modeling.

Recommended reproduction path:

1. Start from `civil_summons_2021_v7`.
2. Run `3-analysis_EDA/clean_v7_model_6.1.26.ipynb`.
3. Verify the final cleaned analysis dataset is `civil_summons_2021_v8.csv`.
4. Use `4-models/model_v8.Rmd` for the regression and model comparison workflow.

## Analysis and Modeling Files

### EDA notebooks

- `3-analysis_EDA/cs_eda_workingcode_4.29.26.ipynb` — main analysis notebook for the master dataset.
- `3-analysis_EDA/clean_v7_model_6.1.26.ipynb` — final analysis-specific cleaning before modeling.
- `3-analysis_EDA/archived-workbooks/` — older notebook versions preserved for comparison and auditability.

### Model files

- `4-models/model_v8.Rmd` — R Markdown notebook for negative binomial models and EDA.
- `4-models/exploring_sept_citations.ipynb` — supplemental notebook for exploring civil summons data.
- `4-models/model_data/` — model-ready datasets and version history.

### Merge support files

- `3-analysis_EDA/manual-payroll-merge-data/` — intermediate files for payroll/roster/CCRB reconciliation.
- `3-analysis_EDA/payroll-roster-ccrb-data/` — source data used for crosswalk and officer attribute merges.
- `3-analysis_EDA/EDA_figures/` — saved exploratory figures.

## Reproducibility Checklist

To recreate the pipeline from a fresh clone:

1. Clone the repo.
2. Open the project in VS Code, JupyterLab, or RStudio.
3. Run the raw citation cleaning notebook in `1-cleaning_raw_citations/`.
4. Run the officer demographic merge in `2-cleaning_officer_dem/`.
5. Run the master dataset notebook in `3-analysis_EDA/`.
6. Run the analysis-specific cleanup notebook.
7. Run `4-models/model_v8.Rmd` for the modeling output.
8. Compare generated outputs against the committed CSVs in `4-models/model_data/` and the other stage folders.

## Suggested Local Setup

The repository uses a mix of Python notebooks and R Markdown. A minimal setup usually includes:

- Python 3.x with `pandas`, `numpy`, `jupyter`, and any notebook-specific packages used in the cleaning notebooks,
- R with `tidyverse`, `MASS`, `AER`, `sandwich`, `lmtest`, `car`, `broom`, `knitr`, and `stargazer` for the modeling notebook.

If you want a reproducible environment, create one per language and keep package versions pinned in your own local setup files.

## Data Notes

- This repository currently centers on the 2021 civil summons workflow.
- Some filenames and notes reference 2022 raw file collection, but the committed tree here contains the 2021 pipeline materials.
- Versioned CSVs are intentionally stored in the repo to make the analysis reproducible even if earlier notebooks change.
- Some files appear to be generated from spreadsheets or manual merges; those are preserved as intermediate artifacts rather than source-of-truth inputs.

## Output Conventions

- Stage outputs use descriptive names with version tags, such as `clean_2021_w_Sept.csv`, `cleaned_demographic_merge_w_Sept.csv`, and `civil_summons_2021_v8.csv`.
- Earlier analysis versions are stored under `4-models/model_data/past_cs_versions/`.
- Notebook versions in `archived-workbooks/` should be treated as historical references, not active sources.

## Working With the Repository

When updating the pipeline:

- duplicate notebooks before making major logic changes,
- keep a change log in the notebook or in accompanying documentation,
- preserve intermediate outputs when a transformation changes the schema,
- document any manual fixes or joins that cannot be inferred from code alone.

## Citation / Use

This repository is maintained for research and educational work by the Evidence for Justice Lab. If you build on this code or data, cite the project and note the dataset version you used.

## Contact

For questions about the workflow, pipeline structure, or data definitions, contact the Evidence for Justice Lab team or the repository maintainer.
