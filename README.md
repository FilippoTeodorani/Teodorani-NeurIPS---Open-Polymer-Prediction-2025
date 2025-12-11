# Open Polymer Property Prediction — NeurIPS 2025

## Project Overview

This repository contains a solution developed for the **NeurIPS Open Polymer Prediction 2025** competition.  
The objective is to predict key physicochemical properties of polymers from their **SMILES (Simplified Molecular Input Line Entry System)** representations using machine learning and cheminformatics.

**SMILES** is a notation that encodes a chemical structure as a text string, allowing molecules to be represented in a compact, machine-readable format. For example, `C(CO)O` represents ethanol. Using SMILES, we can compute molecular descriptors, fingerprints, or feed the strings directly into models for property prediction.

### Target properties
- **Tg** — Glass Transition Temperature  
- **FFV** — Fractional Free Volume  
- **Tc** — Crystallization Temperature  
- **Density** — Polymer density  
- **Rg** — Radius of Gyration

## Pipeline Overview

1. **Data loading** — load `train.csv` and `test.csv` into pandas DataFrames. SMILES strings are read from the `SMILES` column for each polymer.

2. **Feature engineering** — generate molecular descriptors using `compute_rdkit_descriptors` and Morgan fingerprints using `mol_to_morgan_fp_array`. The `build_feature_matrix` function combines these into a numeric matrix for modeling.

3. **Data cleaning & preprocessing** — handle missing values, clip extreme values, and ensure train/test alignment using `data_cleaning`. Features are scaled before modeling.

4. **Feature selection** — remove low-variance and highly correlated features, then select top features based on LightGBM importance via `feature_selection_pipeline`.

5. **Modeling** — train per-target LightGBM regressors with K-Fold cross-validation using `train_lgb_per_target`. Out-of-fold predictions are stored for validation
6.  **Ensembling & postprocessing** — combine outputs from multiple models or folds to improve robustness and reduce variance in predictions.

## Results

The results presented below are based on out-of-fold predictions.  
For each target property, the mean absolute error (MAE) obtained during cross-validation is:

- **Tg** — 88.070  
- **FFV** — 0.021
- **Tc** — 0.077  
- **Density** — 0.108  
- **Rg** — 3.904

Note: The competition had a **hidden test set**. The score reported on that set, after submitting the notebook, was **0.11318** (weighted MAE as defined by the competition).  
This value reflects the final performance of the model on unseen data.
