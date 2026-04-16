# 🔭 Galaxy Morphology Classification & Clustering
### A Data Mining Study on the SDSS DR7 Morphological Catalogue

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.0%2B-orange?logo=scikit-learn&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green)

---

## 📋 Overview

This project applies **supervised classification** and **unsupervised clustering** techniques to automatically classify 670,560 galaxies from the [Sloan Digital Sky Survey Data Release 7 (SDSS-DR7)](https://www.sdss.org/) as either **elliptical** or **spiral**, using non-parametric morphological features.

The dataset is sourced from the published catalogue of **Barchi et al. (2019)** — *Machine and Deep Learning Applied to Galaxy Morphology: A Comparative Study* — and includes six photometric shape parameters computed using the CyMorph pipeline.

> **Science Question:** *Can non-parametric morphological features reliably and automatically classify galaxies as elliptical or spiral — and can unsupervised clustering reveal additional sub-structure within these classes?*

---

## 📁 Repository Structure

```
├── galaxy_morphology_notebook.ipynb   # Main annotated Jupyter notebook
├── galaxy_analysis.py                 # Standalone Python script (all figures)
├── galaxy_report_final.docx           # 3-page Policy/Research Summary report
├── README.md                          # This file
└── figures/                           # Generated output figures
    ├── fig1_distributions.png
    ├── fig2_correlation.png
    ├── fig3_confusion.png
    ├── fig4_importance.png
    ├── fig5_comparison.png
    ├── fig6_elbow.png
    └── fig7_clusters.png
```

> ⚠️ The dataset file (`Barchi19_Morph-catalog_670k-galaxies.csv`) is not included in this repository due to file size. Download it from the [CDS catalogue](https://cdsarc.cds.unistra.fr/) or the link below.

---

## 📊 Dataset

| Property | Value |
|---|---|
| **Source** | Barchi et al. (2019), SDSS-DR7 |
| **Total galaxies** | 670,560 |
| **Galaxies after cleaning** | 505,170 (75.3%) |
| **Features** | C, A, S, G2, H, K (6 non-parametric) |
| **Classes** | 0 = Elliptical, 1 = Spiral |
| **Class balance (clean)** | ~12.8% elliptical / 87.2% spiral |

**Download the dataset:**
- [Barchi et al. 2019 — VizieR CDS](https://cdsarc.cds.unistra.fr/)
- [Kaggle mirror (if available)](https://www.kaggle.com/)

---

## 🔬 Methods

### Data Cleaning & Pre-processing
- Removal of **sentinel values** (~−9999) indicating failed CyMorph measurements
- Removal of rows with **non-zero Error flags** (computation failures)
- Exclusion of **ambiguous labels** (`'U'` = uncertain, `'--'` = unclassified)
- **Balanced stratified sampling** (30,000 per class) to address class imbalance
- **StandardScaler** normalisation for Logistic Regression

### Supervised Classification
Three models trained on an 80/20 stratified train-test split:

| Model | Key Hyperparameters |
|---|---|
| Decision Tree | `max_depth=8` |
| Random Forest | `n_estimators=100`, `max_depth=10` |
| Logistic Regression | `max_iter=500`, L2 regularisation |

### Unsupervised Clustering
- **K-Means** with k evaluated from 2–8
- Optimal k selected via **Elbow Method** + **Silhouette Score**
- **PCA** (2 components) used for cluster visualisation

---

## 📈 Results

### Classification Performance

| Model | Accuracy | AUC-ROC | F1 (macro) |
|---|---|---|---|
| Decision Tree | 96.97% | 0.9912 | 0.97 |
| **Random Forest** | **97.81%** | **0.9973** | **0.98** |
| Logistic Regression | 90.91% | 0.9741 | 0.91 |

- **Random Forest** achieves the best performance across all metrics
- **Smoothness (S)** and **Concentration (C)** are the most important features, accounting for >60% of the Random Forest's predictive power
- Logistic Regression's 90.9% accuracy confirms the classes are approximately linearly separable

### Clustering
- Silhouette Score peaks at **k = 2** (score = 0.349), recovering the elliptical/spiral split without labels
- **k = 3** reveals an interpretable third sub-group: compact, low-smoothness spirals distinct from the diffuse spiral population
- PCA explains **75.3%** of total variance across 2 components (PC1 = 49.5%, PC2 = 25.8%)

---

## 🚀 Getting Started

### Prerequisites

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

### Run the Notebook

```bash
git clone https://github.com/YOUR_USERNAME/galaxy-morphology-classification
cd galaxy-morphology-classification

# Place the dataset CSV in the project root, then:
jupyter notebook galaxy_morphology_notebook.ipynb
```

### Run the Python Script

```bash
# Update DATA_PATH in galaxy_analysis.py to point to your CSV, then:
python galaxy_analysis.py
```

All 7 figures will be saved as `.png` files in the working directory.

---

## 🎨 Visualisation Style

All figures use a **dark navy background** (`#1A1A2E`) with a **unique colour per section** — every bar, histogram panel, cluster, and feature point is individually coloured for maximum visual clarity.

The colour palette used:
`#E63946` · `#F4A261` · `#2A9D8F` · `#457B9D` · `#9B5DE5` · `#F15BB5` · `#00BBF9` · `#00F5D4` · `#FEE440` · `#FB5607` · `#8338EC` · `#06D6A0`

---

## 📄 Report

A 3-page **Policy/Research Summary** (`galaxy_report_final.docx`) is included, formatted in a 2-column academic layout. It contains:
- Problem Statement
- Data Summary
- Methods
- Results (with all figures and a performance table)
- Conclusions & Recommendations
- References

---

## 📚 References

- Barchi, P.H., et al. (2019). *Machine and Deep Learning Applied to Galaxy Morphology — A Comparative Study*. Astronomy and Computing, 30, 100334.
- Dominguez-Sanchez, H., et al. (2018). *Improving galaxy morphologies for SDSS with Deep Learning*. MNRAS, 476(3), 3661–3676.
- Ivezic, Z., et al. (2019). *LSST: From Science Drivers to Reference Design and Anticipated Data Products*. ApJ, 873(2), 111.
- Pedregosa, F., et al. (2011). *Scikit-learn: Machine Learning in Python*. JMLR, 12, 2825–2830.
- York, D.G., et al. (2000). *The Sloan Digital Sky Survey: Technical Summary*. AJ, 120(3), 1579–1587.

---

## 📝 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

*Data Mining Project · SDSS-DR7 · Barchi et al. (2019)*
