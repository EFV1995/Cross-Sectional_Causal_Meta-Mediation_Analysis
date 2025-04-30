# Mediation and PCA Analysis Scripts

This repository contains a set of Python and R scripts for mediation analysis, regression modeling, and principal component analysis (PCA) in a microbiome and nutrition context.

## Contents

### 1. `mediation_unadjusted.py`
- Performs bootstrap-based mediation analysis **without covariate adjustment**.
- Calculates indirect, direct, and total effects along with bootstrap confidence intervals.
- Exports results to Excel.

### 2. `mediation_adjusted_delivery.py`
- Same as above but **adjusts for delivery mode** as a covariate.
- Allows comparison between adjusted and unadjusted mediation effects.

### 3. `mediation_path_diagram.py`
- Visualizes top mediation paths using NetworkX.
- Includes `a`, `b`, and `a×b` effect labels for each mediator.
- Shows both **unadjusted and adjusted** results side by side.

### 4. `diet_regression_with_fdr.py`
- Performs linear regression between **dietary scores** and microbial/diversity features.
- Applies **FDR correction** using the Benjamini-Hochberg method.
- Saves full and significant results to Excel.

### 5. `pca_biplot_cleaned.R`
- Conducts PCA on dietary scores and visualizes with a **biplot**.
- Displays vectors for food groups and dietary indices.
- Clusters samples using k-means and plots ellipses around clusters.
- Final figure saved as a publication-quality PNG.

---

## Requirements

### Python:
- `pandas`, `numpy`, `statsmodels`, `matplotlib`, `networkx`, `openpyxl`
- Optional: `joblib`, `tqdm`

### R:
- `tidyverse`, `ggrepel`, `ggplot2`, `readxl`
