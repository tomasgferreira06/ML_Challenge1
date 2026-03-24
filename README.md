# Binary Bean Classification — Morphological Feature Analysis

![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-F7931E?logo=scikit-learn&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data-150458?logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-Compute-013243?logo=numpy&logoColor=white)
![License](https://img.shields.io/badge/License-Academic-lightgrey)

---

## Overview

This project was developed for the **Machine Learning** course unit of the **Master's in Artificial Intelligence** at the **University of Coimbra** (DEI). It addresses a binary classification problem in the context of food safety: detecting and segregating an allergenic bean variety (*White Haricot Bean*) from a mixed production line using morphological measurements extracted from imaging systems.

The core constraint is asymmetric misclassification cost — a false negative (an allergenic bean classified as safe) carries public health risk, while a false positive (a safe bean rejected) only incurs operational cost. This makes **recall of the allergenic class** the primary optimization target.

---

## Problem Statement

A food processing facility packages mixed dry bean products. One variety, the White Haricot Bean, contains an allergenic protein compound. The objective is to build a classifier that identifies this variety from six quantitative morphological features, minimizing contamination risk.

The dataset (`binary_beans_english.csv`) is a synthetic dataset containing **6,000 observations** with six continuous predictive variables  (`Length`, `Width`, `Area`, `Perimeter`, `Roundness`, `Compactness`) and a binary target (`White Haricot Bean` vs `Mixed Other Beans`). The class distribution is imbalanced (~63% White Haricot Bean, ~37% Mixed Other Beans).

---

## Methodology

### Data Preprocessing

- Removal of observations with missing values in either features or target (190 rows, ~3% of the dataset), following a listwise deletion strategy.
- Label encoding: `White Haricot Bean → 1`, `Mixed Other Beans → 0`.

### Exploratory Data Analysis

- Univariate analysis via histograms and density plots per class.
- Bivariate analysis through boxplots, pairplots, and Spearman correlation matrix.
- Key finding: dimensional features (`Length`, `Width`, `Area`, `Perimeter`) exhibit strong inter-class separation; shape features (`Roundness`, `Compactness`) show high overlap between classes.

### Feature Selection

- **Spearman Correlation Analysis** — identified high multicollinearity among dimensional features (e.g., `Area` ↔ `Width`: ρ = 0.89) and near-zero linear correlation of `Roundness` with the target.
- **ReliefF Algorithm** — produced a non-linear importance ranking. Notably, `Roundness` ranked third despite low linear correlation, indicating contextual discriminative value captured through feature interactions. `Compactness` and `Width` ranked lowest and were removed for the reduced feature set.

### Feature Engineering

- Derived features including aspect ratio, elongation index, and geometric combinations were explored via ReliefF re-evaluation and Spearman analysis.

### Data Preparation

- Stratified 75/25 train-test split to preserve class proportions.
- StandardScaler normalization fitted on training data only to prevent data leakage.
- Two feature configurations tested: full set (6 features) and reduced set (4 features after ReliefF-guided removal).

### Models

| Model | Key Hyperparameters |
|---|---|
| Decision Tree | `max_depth` tuned via recall curve (optimal: 3) |
| K-Nearest Neighbors | `k` tuned from 1–49 (optimal: 49 full / 35 reduced) |
| Logistic Regression | Default with `max_iter=1000` |
| KNN + PCA | PCA with 2 and 3 components combined with tuned KNN |

---

## Results

### Global Metrics

| Model | Accuracy | Precision | Recall | F1-score |
|---|---|---|---|---|
| Decision Tree (depth=3) | 0.810 | 0.814 | 0.810 | 0.802 |
| KNN (k=49) | 0.818 | 0.816 | 0.818 | 0.814 |
| KNN Reduced (k=35) | 0.826 | 0.824 | 0.826 | 0.823 |
| Logistic Regression | 0.821 | 0.819 | 0.821 | 0.819 |
| KNN + PCA(2) | 0.806 | 0.804 | 0.806 | 0.802 |
| KNN + PCA(3) | 0.820 | 0.818 | 0.820 | 0.816 |

### White Haricot Bean (Allergenic Class)

| Model | Precision | Recall | F1-score |
|---|---|---|---|
| **Decision Tree (depth=3)** | 0.800 | **0.933** | 0.861 |
| KNN (k=49) | 0.828 | 0.898 | 0.862 |
| KNN Reduced (k=35) | 0.836 | 0.902 | 0.868 |
| Logistic Regression | 0.841 | 0.884 | 0.862 |
| KNN + PCA(2) | 0.816 | 0.894 | 0.854 |
| KNN + PCA(3) | 0.830 | 0.899 | 0.863 |

The **Decision Tree (max_depth=3)** achieved the highest recall for the allergenic class at **0.933**, making it the selected model. This comes at the cost of lower recall on the safe class (0.599), which translates to higher operational waste but minimizes contamination risk, an acceptable trade-off given the problem constraints.

---

## Tech Stack

- **Python 3.x**
- **pandas** / **NumPy** — data manipulation and numerical computation
- **matplotlib** / **seaborn** — visualization (histograms, boxplots, density plots, heatmaps, pairplots)
- **scikit-learn** — `DecisionTreeClassifier`, `KNeighborsClassifier`, `LogisticRegression`, `PCA`, `StandardScaler`, `train_test_split`, evaluation metrics
- **skrebate** — ReliefF feature importance algorithm

---

## Conclusions and Future Work

The Decision Tree classifier was selected as the most appropriate model for the food safety use case, prioritizing recall of the allergenic class over global accuracy. However, the resulting false positive rate raises questions about economic viability in a production environment.

The main recommendation for future work is **dataset enrichment**, the current morphological features are insufficient for high-precision separation. Adding features with greater discriminative power (e.g., color, texture) could reduce false positives without compromising safety guarantees.

---

## Authors

- **Tomás Ferreira** — University of Coimbra
- **Eduardo Pereira** — University of Coimbra

*Developed for the Machine Learning course, MSc in Artificial Intelligence, University of Coimbra — November 2025*