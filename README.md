# Home Credit Default Risk

## Overview
This project focuses on the Home Credit Default Risk problem, with the goal of predicting which applicants are more likely to have repayment difficulties. The work includes a reproducible data preparation pipeline, a modeling notebook, and a model card notebook that summarizes the final model for a business stakeholder audience.

Because the target is highly imbalanced, model evaluation emphasizes **AUC** rather than accuracy alone.

## Project Files
- `data_preparation.py` — reproducible script for cleaning and feature engineering the raw data
- `modeling_notebook.ipynb` — modeling notebook for benchmark comparison, model evaluation, class imbalance testing, tuning, and Kaggle submission
- `model_card_notebook.ipynb` — business-facing model documentation covering performance, threshold choice, explainability, fairness, and deployment risks
- `README.md` — project documentation

## Data Preparation
The data preparation pipeline processes the raw Home Credit files into cleaned, feature-engineered datasets ready for modeling.

### Input files
- `application_train.csv`
- `application_test.csv`
- `bureau.csv`
- `previous_application.csv`
- `installments_payments.csv`

### Output files
- `train_prepared.parquet`
- `test_prepared.parquet`

### Key preparation steps
- fits preprocessing choices on the training data only to avoid leakage
- aggregates auxiliary tables to the applicant level (`SK_ID_CURR`)
- creates engineered features such as ratios, bins, and cleaned categorical fields
- saves modeling-ready outputs in Parquet format

### Requirements
```bash
pip install pandas numpy pyarrow
```

### Run the pipeline
```bash
python data_preparation.py
```

## Modeling
The modeling notebook compares several candidate approaches for predicting default risk.

### Models tested
- Majority Classifier
- Logistic Regression
- Random Forest
- Histogram-Based Gradient Boosting

### Evaluation approach
- **3-fold cross-validation**
- **AUC as the primary metric**
- accuracy reported only as a secondary metric because of class imbalance

### Class imbalance strategies tested
- no resampling
- random oversampling
- random undersampling
- SMOTE
- class weighting

### Supplementary features
The modeling workflow compares base application features against feature sets enhanced with aggregated supplementary information from:
- `bureau.csv`
- `previous_application.csv`
- `installments_payments.csv`

Additional supplementary tables such as `POS_CASH_balance.csv` and `credit_card_balance.csv` can also be incorporated.

## Final Model Selection
The final model was selected based on **cross-validation performance**, using **AUC** as the main model selection criterion. This approach was more appropriate than accuracy because the dataset is highly imbalanced and the goal is to rank higher-risk applicants above lower-risk applicants.

After model comparison and tuning, the best-performing approach from cross-validation was trained on the full training data and used to generate the Kaggle submission.

## Kaggle Result
**Public leaderboard score:** `0.71984`

## Model Card
The model card notebook documents the final Home Credit Default Risk model for a business stakeholder audience. It summarizes the selected model, intended use, performance metrics, threshold analysis, explainability, fairness checks, adverse-action mappings, and key limitations.

### What the model card covers
- **Model details** — final model type, training data summary, and tuning approach
- **Intended use** — how the model supports lending decisions and what it is not designed for
- **Performance metrics** — AUC, precision, and recall with both validation and Kaggle reporting
- **Decision threshold analysis** — a business-oriented threshold recommendation based on lending profit, loss given default, and recovery assumptions
- **Explainability** — SHAP-based feature importance to show what drives predictions
- **Adverse action mapping** — translation of important technical features into plain-language denial reasons
- **Fairness analysis** — comparison of approval rates across `CODE_GENDER` and `NAME_EDUCATION_TYPE`
- **Limitations and risks** — practical, legal, and modeling caveats for deployment

### Business takeaway
The model card frames the final model as a **decision-support tool**, not a production-ready automated underwriting system. It provides a structured way to discuss expected business value, threshold choices, explainability, fairness concerns, and deployment risks with non-technical stakeholders.

## Key Takeaways
- Accuracy alone is misleading for this problem because the target is highly imbalanced.
- AUC is the most useful metric for comparing candidate models.
- Supplementary aggregated credit-history features improve the modeling signal.
- Cross-validation provided a reliable way to compare models before submission.
- The model card extends the project from model building into business-facing model governance and communication.

## Next Steps
- add exact cross-validated AUC results for each model
- continue feature engineering from additional supplementary tables
- test other boosting methods such as XGBoost or LightGBM
- keep generated artifacts and submission files out of GitHub using `.gitignore`

## Reproducibility Notes
The project separates data preparation from modeling so the workflow is easier to maintain, rerun, and improve. Large generated files such as model artifacts and submission CSVs should be saved locally and excluded from version control.
