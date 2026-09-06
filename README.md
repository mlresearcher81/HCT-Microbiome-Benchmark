# HCT-Microbiome Benchmark

A reproducible pipeline for longitudinal gut microbiome surveillance and early dysbiosis forecasting after hematopoietic cell transplantation (HCT).

This repository includes data preprocessing, temporal sequence alignment, and training/evaluation code for an auto-regressive Bi-LSTM model plus static baseline models (XGBoost, Random Forest, Logistic Regression).

## Quickstart

Run the pipeline directly in Google Colab with zero local setup. You can either generate the dataset from the raw files or jump straight into the benchmark evaluation.

**1. Generate the Dataset:**
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/mlresearcher81/HCT-Microbiome-Benchmark/blob/main/data-preprocessing/HCT_Clinical_Microbiome_Integration.ipynb)

**2. Run Benchmark Evaluation:**
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/mlresearcher81/HCT-Microbiome-Benchmark/blob/main/notebook/HCT_Microbiome_Benchmark.ipynb)


## What’s Included

- **`data/raw/`**: Contains the original source CSV files required to build the benchmark:
  - `tblASVsamples.csv`
  - `tblASVtaxonomy_silva132_v4v5_filter.csv`
  - `tblcounts_asv_melt.csv`
  - `tblhctmeta.csv`
  - `tbltemperature.csv`
  - `tblwbc.csv`
- **`data/HCT_Microbiome_Benchmark.csv`**: The final, temporally aligned feature matrix ready for machine learning.
- **`data/Data Dictionary.md`**: Outlines the schema, provenance, and definitions for the clinical and taxonomic features.
- **`data-preprocessing/HCT_Clinical_Microbiome_Integration.ipynb`**: The Python notebook containing the exact scripts used to merge the raw data, forward-fill clinical variables, and zero-fill missing taxonomic observations.
- **`notebook/HCT_Microbiome_Benchmark.ipynb`**: The end-to-end executable pipeline for model training and evaluation.
- **`requirements.txt`**: Dependency list for local execution.

## Requirements

- Python 3.10 or later
- `pip` or `conda`
- Jupyter Notebook for local interactive execution

## Local setup

### Option A: Python `venv`

```bash
git clone https://github.com/mlresearcher81/HCT-Microbiome-Benchmark.git
cd HCT-Microbiome-Benchmark
python -m venv hct_env
```

Activate the virtual environment:

- macOS / Linux:
  ```bash
  source hct_env/bin/activate
  ```
- Windows:
  ```powershell
  .\hct_env\Scripts\activate
  ```

Install requirements:

```bash
pip install -r requirements.txt
```

### Option B: `conda`

```bash
git clone https://github.com/mlresearcher81/HCT-Microbiome-Benchmark.git
cd HCT-Microbiome-Benchmark
conda create -n hct_env python=3.10 -y
conda activate hct_env
pip install -r requirements.txt
```

## Running the notebook locally

If Jupyter is not installed in the environment:

```bash
pip install notebook
```

Start Jupyter Notebook:

```bash
jupyter notebook
```

In the browser:

1. Open the `notebooks/` folder
2. Open `HCT_Microbiome_Benchmark.ipynb`
3. Choose `Kernel -> Restart & Run All`

## Repository structure

```text
HCT-Microbiome-Benchmark/
├── data/
│   ├── raw/csv files
│   ├── HCT_Microbiome_Benchmark.csv
│   └── Data Dictionary.md
├── data-preprocessing/
│   └── HCT_Clinical_Microbiome_Integration.ipynb
├── notebook/
│   └── HCT_Microbiome_Benchmark.ipynb
├── requirements.txt
└── README.md
```

## Data

The `data/` directory contains the benchmark dataset and a feature dictionary describing clinical and taxonomic variables. If the data are not included directly, the notebook will document where to obtain or generate the required files.

## Notes

- The notebook is the primary entry point for reproducing results.
- Use the activated environment before running any commands.
- If installation fails, confirm your Python version and reinstall dependencies.

## License

This code and dataset are provided in support of the accepted camera-ready manuscript for the IEEE International Conference on Knowledge Graph (ICKG) 2026.
