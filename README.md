# Civil Summons Enforcement Project

Repository for the Evidence for Justice Lab's 2021 civil summons data merging, cleaning, EDA, and model-building workflow.

The goal of this project is to create a reproducible analysis dataset for NYC civil summons research. The repository currently documents a 2021 workflow, with versioned outputs preserved at each stage so future work can be re-created or extended to later years. For future civil cummons years, this workflw can be replicated and/or consolidated. 

## What’s In This Repo

This repository contains:

- raw 2021 civil ciations transcribed from E4J lab members, 
- NYPD/OATH officer data,
- notebook cleaning workflows for civil summons and officer demographic data,
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
│   │   └── exploring_sept_citations.ipynb
│   ├── cs_eda_workingcode_4.29.26.ipynb
│   ├── manual-payroll-merge-data/
│   └── payroll-roster-ccrb-data/
├── 4-geocoding/
│   ├── geocoding.ipynb
│   └── geocoding_data/
├── 5-arrests_911_calls/
│   ├── 911CallCode.ipynb
│   ├── ArrestDataCode.ipynb
│   ├── merge_geocode_arrests.ipynb
│   └── arrest_calls_data/
├── 6-tidying_for_analysis/
│   └── clean_v7_model_6.1.26.ipynb
├── 7-models/
│   ├── model_v8.Rmd
│   └── model_data/
├── extra-data/
└── README.md
```

## Workflow Overview

The project follows a staged pipeline:

1. **Clean raw civil citation files**
2. **Clean and merge officer demographic files**
3. **Run Pre-Analysis EDA**
4. **Add Geocoding data**
5. **Add 911 Calls and Arrests data**
6. **Tidy Data for Final Analysis**
7. **Modelling in R** 

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

### 3) Run Pre-Analysis EDA

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

### 4) Geocoding

Location:

- `4-geocoding/geocoding.ipynb`
- `4-geocoding/geocoding_data/`

Purpose:

- geocode civil summons addresses to geographic coordinates,
- match addresses to census block data for spatial analysis.

Recommended reproduction path:

1. Start from the cleaned civil summons output.
2. Run `4-geocoding/geocoding.ipynb`.
3. Verify matched and unmatched outputs in `4-geocoding/geocoding_data/`.

### 5) Merging Arrest and 911 Call Data

Location:

- `5-arrests_911_calls/ArrestDataCode.ipynb`
- `5-arrests_911_calls/911CallCode.ipynb`
- `5-arrests_911_calls/merge_geocode_arrests.ipynb`
- `5-arrests_911_calls/arrest_calls_data/`

Purpose:

- clean and process NYPD arrest and 911 call records,
- merge geocoded civil summons data with arrest and call data.

Recommended reproduction path:

1. Run `ArrestDataCode.ipynb` and `911CallCode.ipynb` to process source files.
2. Run `merge_geocode_arrests.ipynb` to join with geocoded civil summons data.

### 6) Tidying for Analysis

Location:

- `6-tidying_for_analysis/clean_v7_model_6.1.26.ipynb`
- output: `7-models/model_data/civil_summons_2021_v8.csv`

Purpose:

- perform analysis-specific cleanup on the master dataset,
- prepare officer-level data for regression and EDA,
- generate the final current analysis dataset used in modeling.

Recommended reproduction path:

1. Start from `civil_summons_2021_v7`.
2. Run `6-tidying_for_analysis/clean_v7_model_6.1.26.ipynb`.
3. Verify the final cleaned analysis dataset is `civil_summons_2021_v8.csv`.

### 7) Modeling

Location:

- `7-models/model_v8.Rmd`
- `7-models/model_data/civil_summons_2021_v8.csv`

Purpose:

- run negative binomial models and EDA on the final analysis dataset.

Recommended reproduction path:

1. Start from `7-models/model_data/civil_summons_2021_v8.csv`.
2. Use `7-models/model_v8.Rmd` for the regression and model comparison workflow.

### EDA notebooks

- `3-analysis_EDA/cs_eda_workingcode_4.29.26.ipynb` — main analysis notebook for the master dataset.
- `3-analysis_EDA/archived-workbooks/exploring_sept_citations.ipynb` — supplemental notebook for exploring missing september civil summons data. 
- `3-analysis_EDA/archived-workbooks/` — older notebook versions 

### Geocoding notebooks

- `4-geocoding/geocoding.ipynb` — geocodes civil summons addresses and matches to census blocks.

### Arrests and 911 calls notebooks

- `5-arrests_911_calls/ArrestDataCode.ipynb` — cleans and processes NYPD arrest records.
- `5-arrests_911_calls/911CallCode.ipynb` — cleans and processes 911 call records.
- `5-arrests_911_calls/merge_geocode_arrests.ipynb` — merges geocoded civil summons with arrest/call data.

### Tidying for analysis

- `6-tidying_for_analysis/clean_v7_model_6.1.26.ipynb` — final analysis-specific cleaning before modeling.

### Model files

- `7-models/model_v8.Rmd` — R Markdown notebook for negative binomial models and EDA.
- `7-models/model_data/` — model-ready datasets and version history.

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
6. Run the geocoding notebook in `4-geocoding/`.
7. Run the arrest and 911 call notebooks in `5-arrests_911_calls/`.
8. Run the analysis-specific cleanup notebook in `6-tidying_for_analysis/`.
9. Run `7-models/model_v8.Rmd` for the modeling output.
10. Compare generated outputs against the committed CSVs in `7-models/model_data/` and the other stage folders.

## Suggested Local Setup

The repository uses a mix of Python notebooks and R Markdown. A minimal setup usually includes:

- Python 3.x with `pandas`, `numpy`, `jupyter`, and any notebook-specific packages used in the cleaning notebooks,
- R with `tidyverse`, `MASS`, `AER`, `sandwich`, `lmtest`, `car`, `broom`, `knitr`, and `stargazer` for the modeling notebook.

If you want a reproducible environment, create one per language and keep package versions pinned in your own local setup files.

## Data Notes

- This repository currently centers on the 2021 civil summons workflow.
- Versioned CSVs are stored in the repo to make the analysis reproducible even if earlier notebooks change.

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

This repository is maintained for research by the Evidence for Justice Lab. If you build on this code or data, cite the project and note the dataset version you used.

## Contact

For questions about the workflow, pipeline structure, or data definitions, contact the Evidence for Justice Lab team. 
