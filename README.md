# Tabular Foundation Models (TFMs) Benchmark in WHO-informed Datasets

This folder contains the datasets and model/analysis notebooks for the current multi-domain clinical tabular classification study. The study compares four tabular foundation models with two conventional baselines across ten binary classification datasets. A separate set of preprocessed dataset variants is retained for future studies and sensitivity analyses; those variants are not the inputs used for the current study.

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
    └── Aggregation_Composites_StatAnalysis.ipynb
```

All CSVs use `sex` as the first feature column and `outcome` as the final target column. `sex` is encoded 0 for female and 1 for male; `outcome` is binary (0 negative, 1 positive). Remaining categorical features are integer label-encoded. Missing predictor values are retained in the CSVs and imputed inside each training fold by the model code.

## Models and notebooks

| Notebook | Models | Result CSVs produced |
|---|---|---|
| `Model Codes/TabPFN_Codes.ipynb` | TabPFN v2 | `TabPFN_Results.csv` |
| `Model Codes/TabICL_Codes.ipynb` | TabICL | `TabICL_Results.csv` |
| `Model Codes/TabFM_Codes.ipynb` | TabFM v1.0.0 | `TabFM_Results.csv` |
| `Model Codes/TabDPT_Codes.ipynb` | TabDPT v1.3 | `TabDPT_Results.csv` |
| `Model Codes/XGBoost_LogiReg_Codes.ipynb` | XGBoost and Logistic Regression | `XGBoost_Results.csv`, `LogisticRegression_Results.csv` |

The notebooks are written for Google Colab. Each starts by defining `/content/` paths for the ten CSVs. Upload the ten files from the study dataset folder to the Colab session so each path resolves, then run the notebooks (different session preferred to avoid dependency class in TFMs). Alternatively, edit the first notebook cell to point to the uploaded files’ actual paths. The notebooks install their required packages in setup cells; use a GPU runtime for the foundation-model notebooks where available. TabPFN downloads its pretrained checkpoint as part of setup. Model weights and hardware can affect runtime and exact results.

Use the files in `Datasets Used in Research/Preprocessed Data [Natural Class Ratio and Down Sample]/` for the current study. Run the .ipynb files accordingly in `Model Codes/`, and it will generate excel/csv files of each model performance. Then use the `Model Codes/Aggregation_Composites_StatAnalysis.ipynb` to generate the tables.

## Additional datasets

`Additional Datasets/` contains other versions of the same ten preprocessed datasets:

- **Balanced and Full Sample:** majority-class downsampling was applied to balance the classes, while keeping all available minority-class examples. Use this when a future analysis needs more rows but a balanced target.
- **Balanced and Down Sample:** a smaller balanced version, capped at 700 rows where possible. Use for future compute-limited or controlled-size comparisons.
- **Natural Class Ratio and Full Sample:** the full deduplicated preprocessed samples, retaining each dataset’s observed class distribution. Use for future studies that need the original natural imbalance or to test how conclusions change with sample size and class balance.

`Original Dataset Source Links.docx` provides the original datasets' source links. 



