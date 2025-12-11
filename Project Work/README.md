# NeurIPS Open Polymer Prediction 2025 – Solution Pipeline

This repository contains an end-to-end machine learning pipeline developed for the **NeurIPS Open Polymer Prediction 2025** challenge on Kaggle.

The goal of the competition is to predict several polymer physical properties — **Tg**, **FFV**, **Tc**, **Density**, and **Rg** — from molecular structures provided as **SMILES** strings.

This project implements a full feature–engineering and modeling workflow based on RDKit molecular descriptors, Morgan fingerprints, feature cleaning, feature selection and per-target LightGBM models.


---

## **Reference to the Kaggle Challenge**
The official competition page is available here:

 *Kaggle: NeurIPS Open Polymer Prediction 2025*  
https://www.kaggle.com/competitions/neurips-open-polymer-prediction-2025

It provides the dataset, the evaluation metrics, and the competition rules.


---

## **Project Overview**

This repository includes:

### **1. Descriptor and Feature Generation**
- RDKit molecular descriptors (hundreds of physico-chemical properties)
- Morgan fingerprints (1024 bits)

### **2. Data Cleaning Pipeline**
- Replacement of ±∞ with NaN  
- Removal of columns with excessive missing values  
- KNN imputation  
- Clipping of extreme outliers  
- Standardization per target

### **3. Global Feature Selection**
- Removal of constant and quasi-constant features  
- Correlation filtering  
- LightGBM importance ranking  
- Selection of **top 300 most informative features**

The same selected features are then used for all targets.

### **4. Target-Specific Modeling**
For each property (Tg, FFV, Tc, Density, Rg):
- Rows with available labels are selected
- StandardScaler applied
- LightGBM model trained with K-fold CV
- OOF MAE recorded
- Test predictions averaged across folds

### **5. Output**
- A `predictions.csv` file containing the predictions for all targets
