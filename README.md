# Thesis-Final: Replication Materials

## Overview
This repository contains all code and data required to replicate the analyses 
in this thesis. Data collection scripts are provided for transparency but do 
not need to be re-run — all pre-collected data is provided.

## Folder Structure
```text
Master_Thesis/
├── data/                          # All input and output CSV files
├── raw_data/                      # All raw data: collected API data, and survivor public interview PDFs
├── figures                        # All Python generated figures
├── R_data/                        # RDS model objects and R-generated CSVs
├── R_Figures/                     # All R generated figures
├── R_Tables/                      # All R generated tables
├── 1_data_collection.ipynb        # API data collection (API)
├── 2_member_collection.ipynb      # Member data collection (API)
├── 3_merge_speeches.ipynb         # Merges annual speech CSVs
├── 4_data_cleaning.ipynb          # Cleans and merges speeches with members
├── 5_baseline_collection.ipynb    # Collects control group speeches and cleans and merges with members (API)
├── Phase1_final.ipynb             # Phase1 analysis (keyness - log likelihood, ParlaSent)
├── Phase2_3_final.ipynb           # Phase2 & 3 analysis: survivor similarity, role switchers OLS, Granger Causality
└── STM_others_final.Rmd           # Structural Topic Model (R), Krippendorf, Change-point analysis, Multivariate Mixed-effects model
```

## How to Reproduce

### Steps

**Step 1 & 2 — Data Collection (SKIP)**
Steps 1 and 2 fetch from the Oireachtas API and take several hours. 
Pre-collected data is provided in `data/`. Start from Step 3.

**Step 3 — Merge Speeches**
Run `3_merge_speeches.ipynb`
Combines annual speech CSV files into one merged file.
Input:  `data/final_speeches_*_mother_and_baby_homes_speeches.csv`
Output: `data/merged_speeches_all_mother_and_baby_homes_speeches.csv`

**Step 4 — Data Cleaning**
Run `4_data_cleaning.ipynb`
Merges speeches with member metadata, assigns gender, cleans text.
Input:  `data/merged_speeches_all_mother_and_baby_homes_speeches.csv`
        `data/members_with_gender_guess.csv`
Output: `data/df_merged_clean.csv`
        `data/parliamentary_baseline_cleaned.csv`

**Step 5 — Baseline Collection (SKIP)**
Also hits the Oireachtas API. Pre-collected data provided in `data/`.

**Step 6 — Phase 1 Python Analysis**
Run `Phase1_final.ipynb`
Runs keyness analysis and ParlaSent sentiment scoring.
Input:  `data/df_merged_clean.csv`
        `data/parliamentary_baseline_cleaned.csv`
Output: `data/df_merged_clean_sentimentscores.csv`
        `data/df_baseline_cleaned_sentimentscores.csv`
Note: ParlaSent scoring is slow on CPU (approx. 10-20 minutes).

**Step 7 — Phase 1 R Analysis**
Open and knit `STM_others_final.Rmd` in RStudio.
Runs STM and Krippendorf
Set REFIT_MODELS <- FALSE and REFIT_BASELINE <- FALSE to load 
pre-fitted models from `R_data/` (recommended — fitting takes hours).
Input:  `data/df_merged_clean.csv`
        `data/parliamentary_baseline_cleaned.csv`
Output: `R_data/regime_gov_monthly.csv`
        `R_data/topic_gov_monthly.csv`
        `R_data/theta_speech_level.csv`

**Step 8 — Phase 2 &3 Python Analysis**
Run `Phase2_3_final.ipynb`
Runs survivor similarity, role switchers OLS, Granger Causality.
Input:  `data/df_merged_clean_sentimentscores.csv`
        `data/df_baseline_cleaned_sentimentscores.csv`
        `R_data/regime_gov_monthly.csv`
        `R_data/topic_gov_monthly.csv`
        `R_data/theta_speech_level.csv`
        `Interviews/` (survivor PDF transcripts)
Output: `data/all_switchers_combined.csv`
        `data/df_mbh_quarterly.csv`

**Step 9 — Change-Point & Mixed Effects (R)**
Return to `STM_others_final.Rmd` and run from Phase2 for Change-Point Analysis, Mixed Effects Multivariate model
Input:  `data/df_mbh_quarterly.csv`
        `R_data/regime_gov_monthly.csv`
        `data/all_switchers_combined.csv`

## Notes
- All paths are relative — run scripts from the `Master_Thesis/` directory
- Figures are saved to `figures/` or `R_figures/`, tables to `R_Tables/`
- Pre-fitted STM models are in `R_data/` as .rds files
- Steps 1, 2, and 5 are data collection files through an API
