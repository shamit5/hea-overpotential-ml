HEA Overpotential ML

Machine learning pipeline for predicting the oxygen evolution reaction (OER) overpotential of Fe–Co–Cr–Mn–Cu high-entropy alloy (HEA) compositions — from exploratory data analysis through model comparison to SHAP-based interpretability.

Overview

This repository contains a full, reproducible ML workflow built on a 70-sample dataset of quinary alloy compositions (Fe, Co, Cr, Mn, Cu) and their measured OER overpotential. The goal is twofold:

Predict overpotential from composition, using a validation scheme appropriate for a small dataset (Leave-One-Out CV).
Explain what drives the prediction, using SHAP and permutation importance, to surface an actionable materials-design rule rather than a black-box score.
Key Findings
Best model: tuned XGBoost, LOOCV R² = 0.768, MAE ≈ 10.1 mV — close to the ~15–25 mV noise floor set by duplicate-composition measurements, meaning there is limited room to improve without cleaner or additional data.
Dominant driver: the Cr·Fe interaction — not either element alone — is the strongest predictor of overpotential, confirmed independently by SHAP and permutation importance. High simultaneous Cr and Fe fractions consistently predict worse (higher) overpotential.
Negative results, reported honestly: ANN and RNN/LSTM architectures were tested and both failed decisively (R² far below zero) — expected given the small sample size and, for the RNN, an architectural mismatch (composition data has no meaningful sequence). Ensembling (voting/stacking) also failed to beat the single tuned model. These are treated as informative findings, not omissions.
Model	LOOCV R²	MAE (mV)	RMSE (mV)
XGBoost (tuned)	0.768	10.09	13.89
XGBoost (untuned)	0.741	10.56	14.66
Gradient Boosting	0.720	10.89	15.24
Random Forest	0.657	11.93	16.87
Lasso / ElasticNet / PLS	~0.58	~14.1	~18.6
Ridge	0.471	15.98	20.97
SVR (RBF)	0.428	16.07	21.79
ANN (MLP)	−6.68	56.03	79.87
RNN (LSTM)	−113.5	307.13	308.44
Repository Structure
hea-overpotential-ml/
├── README.md
├── data/
│   └── dataset_with_selected_25_features.xlsx
├── notebooks/
│   ├── HEA_EDA_and_SHAP_Analysis.ipynb            # EDA + interpretability only
│   └── HEA_Overpotential_Full_Pipeline.ipynb       # full modeling pipeline
└── requirements.txt
Methodology

Exploratory Data Analysis

Data integrity checks (missing values, duplicates, composition sum-to-1 validation)
Duplicate-composition analysis to establish a measurement noise floor
Full feature correlation heatmap, per-element scatter plots, pairplots
Multicollinearity check (VIF) on base composition fractions
PCA projection and K-Means clustering (exploratory, not used as model input)
Lasso-based feature relevance as an independent cross-check

Modeling

Leave-One-Out Cross-Validation throughout, chosen for the small sample size (n=70)
9+ model families compared: linear/regularized (Ridge, Lasso, ElasticNet, PLS), kernel (SVR, Gaussian Process), and tree/boosting (Random Forest, Gradient Boosting, XGBoost)
Hyperparameter search on the winning model family
ANN and RNN tested for completeness and reported as negative results
Voting and Stacking ensembles, plus seed-bagging, tested as combination strategies

Interpretability

SHAP (TreeExplainer) summary and dependence plots
Permutation importance as an independent cross-check on SHAP
Getting Started
bash
git clone https://github.com/<your-username>/hea-overpotential-ml.git
cd hea-overpotential-ml
pip install -r requirements.txt
jupyter notebook notebooks/HEA_EDA_and_SHAP_Analysis.ipynb
Requirements
Python 3.10+
pandas, numpy, scikit-learn, xgboost, shap, statsmodels, matplotlib, seaborn, openpyxl
Dataset

data/dataset_with_selected_25_features.xlsx — 70 rows, 5 base composition fractions (Fe, Co, Cr, Mn, Cu) plus 22 engineered polynomial/interaction features, and the measured Overpotential (mV) target.

Author

Shamit — B.Tech Engineering Physics, Delhi Technological University

License

MIT
