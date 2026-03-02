
# Home Credit Default Risk - Data Preparation

## Description
This project provides a reproducible data preparation pipeline for the Home Credit Default Risk dataset. It processes raw CSV files (`application_train.csv`, `application_test.csv`, `bureau.csv`, `previous_application.csv`, `installments_payments.csv`) into clean, feature-engineered Parquet files suitable for machine learning models.

## Requirements
To run this script, you need the following Python libraries:
- `pandas`
- `numpy`
- `pyarrow` or `fastparquet` (for Parquet I/O)

You can install them via pip:
```bash
pip install pandas numpy pyarrow
```

## Usage
1. Ensure all raw CSV files are located in the same directory as `data_preparation.py`.
2. Run the script using Python:
```bash
python data_preparation.py
```

## Output
The script generates two output files in the same directory:
- `train_prepared.parquet`: Processed training data with the target variable.
- `test_prepared.parquet`: Processed test data aligned with the training columns.

## Key Features
- **No Data Leakage**: Preparation parameters (medians, bin edges, categorical levels) are fit solely on the training set and applied to the test set.
- **Aggregation**: Aggregates auxiliary tables (`bureau`, `previous_application`, `installments_payments`) to the applicant level (`SK_ID_CURR`).
- **Transformation**: Performs feature engineering including binning, ratio calculation, and categorical encoding.
Writing README.md
Final Task
Subtask:
Verify the created files and summarize the deliverables.

Summary:
Q&A
Question: How can the entire data preparation pipeline be consolidated into a reproducible format? Answer: The pipeline was consolidated by exporting all helper functions, import statements, and execution logic into a single Python script named data_preparation.py using the %%writefile command. This script loads the raw CSVs, processes them using fit parameters from the training set, aggregates auxiliary tables, and saves the results as Parquet files.

Question: How should the new script be documented for users? Answer: A README.md file was created to document the project. It includes sections on the project description, library requirements (pandas, numpy, pyarrow), usage instructions (python data_preparation.py), output descriptions, and key features like leakage prevention and data aggregation.

Data Analysis Key Findings
Script Generation: A standalone Python script data_preparation.py was successfully created. It contains the complete logic to transform raw data into train_prepared.parquet and test_prepared.parquet.
Feature Engineering Consistency: The script ensures consistency between train and test sets by fitting parameters (e.g., medians for imputation, bin edges for age/employment, categorical levels) solely on the training data and applying them to the test data.
Data Aggregation: The pipeline aggregates data from auxiliary tables (bureau, previous_application, installments_payments) to the applicant level (SK_ID_CURR) before merging.
Parquet Compatibility: The script explicitly handles data type conversions, specifically converting pandas Interval objects (from binning) to strings, ensuring the data can be saved to the Parquet format without errors.
Insights or Next Steps
Reproducibility: The creation of data_preparation.py decouples the data engineering process from the exploration notebooks, allowing for easier deployment and version control of the preprocessing logic.
Automation: This script can now be integrated into a larger MLOps pipeline (e.g., as a step in an Airflow DAG or a Kubeflow component) to automatically retrain models when new data arrives.