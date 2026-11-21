# Reproducing the Tabular Foundation Benchmark

This folder contains the datasets and model notebooks for the current multi-domain clinical tabular classification study. The study compares four tabular foundation models with two conventional baselines across ten binary classification datasets. A separate set of preprocessed dataset variants is retained for future studies and sensitivity analyses; those variants are not the inputs used for the current study.

## Folder guide

```text
GitHub/
├── Datasets Used in Research/
│   └── Preprocessed Data [Natural Class Ratio and Down Sample]/
│       └── 10 preprocessed CSV files used by the study notebooks
├── Additional Datasets/
│   ├── Preprocessed Data [Balanced and Down Sample]/
│   ├── Preprocessed Data [Balanced and Full Sample]/
│   └── Preprocessed Data [Natural Class Ratio and Full Sample]/
└── Model Codes/
    ├── TabDPT_Codes.ipynb
    ├── TabFM_Codes.ipynb
    ├── TabICL_Codes.ipynb
    ├── TabPFN_Codes.ipynb
    └── XGBoost_LogiReg_Codes.ipynb
```

All CSVs use `sex` as the first feature column and `outcome` as the final target column. `sex` is encoded 0 for female and 1 for male; `outcome` is binary (0 negative, 1 positive). Remaining categorical features are integer label-encoded. Missing predictor values are retained in the CSVs and imputed inside each training fold by the model code.

## Dataset suite used in this study

Use the files in `Datasets Used in Research/Preprocessed Data [Natural Class Ratio and Down Sample]/` for the current study. The counts below are the actual contents of that folder; class counts are shown as outcome 0 / outcome 1. The folder name reflects its dataset lineage, but some class counts were intentionally set to equal class sizes during preparation, and stroke retains all 249 available positive examples.

| Dataset | CSV filename | Domain | Rows | Outcome 0 / 1 |
|---|---|---|---:|---:|
| Alzheimer’s | `alzheimers_disease_data_preprocessed.csv` | Neurology | 700 | 350 / 350 |
| Arrhythmia | `arrhythmia_data_preprocessed.csv` | Cardiology | 452 | 245 / 207 |
| Autism screening | `autism_screening_data_preprocessed.csv` | Neurodevelopmental / psychiatry | 699 | 512 / 187 |
| COPD | `COPD_data_preprocessed.csv` | Pulmonology / respiratory | 630 | 192 / 438 |
| Diabetes | `diabetes_data_preprocessed.csv` | Endocrinology | 700 | 350 / 350 |
| Stroke | `healthcare-dataset-stroke-data_preprocessed.csv` | Neurology / cerebrovascular | 700 | 451 / 249 |
| Heart disease | `heart_disease_uci_preprocessed.csv` | Cardiology | 700 | 350 / 350 |
| Hepatitis | `hepatitis_data_preprocessed.csv` | Infectious disease / hepatology | 155 | 123 / 32 |
| Liver disease | `liver_patient_dataset_preprocessed.csv` | Hepatology | 700 | 350 / 350 |
| Thyroid | `thyroid_dataset_preprocessed.csv` | Endocrinology | 700 | 350 / 350 |

## Additional datasets

`Additional Datasets/` contains other versions of the same ten preprocessed datasets:

- **Balanced and Full Sample:** majority-class downsampling was applied to balance the classes, while keeping all available minority-class examples. Use this when a future analysis needs more rows but a balanced target.
- **Balanced and Down Sample:** a smaller balanced version, capped at 700 rows where possible. Use for future compute-limited or controlled-size comparisons.
- **Natural Class Ratio and Full Sample:** the full deduplicated preprocessed samples, retaining each dataset’s observed class distribution. Use for future studies that need the original imbalance or to test how conclusions change with sample size and class balance.

These are supplementary data variants. Do not substitute them into the current study runs unless intentionally conducting and labeling a new analysis.

## Models and notebooks

| Notebook | Models | Result CSVs produced |
|---|---|---|
| `Model Codes/TabPFN_Codes.ipynb` | TabPFN v2 | `TabPFN_Results.csv` |
| `Model Codes/TabICL_Codes.ipynb` | TabICL | `TabICL_Results.csv` |
| `Model Codes/TabFM_Codes.ipynb` | TabFM v1.0.0 | `TabFM_Results.csv` |
| `Model Codes/TabDPT_Codes.ipynb` | TabDPT v1.3 | `TabDPT_Results.csv` |
| `Model Codes/XGBoost_LogiReg_Codes.ipynb` | XGBoost and Logistic Regression | `XGBoost_Results.csv`, `LogisticRegression_Results.csv` |

The notebooks are written for Google Colab. Each starts by defining `/content/` paths for the ten CSVs. Upload the ten files from the study dataset folder to the Colab session so each path resolves, then run the notebook from top to bottom. Alternatively, edit the first notebook cell to point to the uploaded files’ actual paths. The notebooks install their required packages in setup cells; use a GPU runtime for the foundation-model notebooks where available. TabPFN downloads its pretrained checkpoint as part of setup. Model weights and hardware can affect runtime and exact results.

### Reproduction sequence

1. Make a working copy of the required notebook in Colab. Keep the original notebook unchanged as the repository record.
2. Upload the ten CSV files from the study dataset folder. Confirm the filenames match the paths declared in the first cell.
3. Run the dependency/setup cells, then run the evaluation cells in order. The baseline notebook evaluates Logistic Regression and XGBoost separately and saves a CSV for each.
4. Save the generated result CSVs alongside your run notes. Record the notebook revision, package versions, runtime type/GPU, and any changed settings so results can be audited.
5. For final reporting, aggregate fold-level metrics as mean and standard deviation and follow the project’s Reporting Structure. Keep model results separate until the prescribed aggregation/composite analysis step.

## Evaluation details and dataset note

The notebooks implement stratified cross-validation, fold-specific KNN imputation, a 0.5 classification threshold, and clean-versus-noisy test evaluation. In the baseline notebook, Bayesian hyperparameter optimization is performed within each outer training fold using Optuna/TPE, inner 3-fold stratified CV, and 5 trials per outer fold. Gaussian perturbations are applied to held-out test features using a scale derived from that fold’s training data (10% of the training-fold feature standard deviation). Robustness metrics are computed for each fold and summarized across folds; the noise procedure is not a repetition over five noise seeds.

For dataset provenance and preprocessing details, refer to the project’s dataset source links and preprocessing guidelines maintained with the study materials. The study CSVs are already preprocessed; do not run preprocessing again during model evaluation.
