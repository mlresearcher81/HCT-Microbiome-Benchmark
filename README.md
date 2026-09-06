# HCT-Microbiome Benchmark

A reproducible pipeline for longitudinal gut microbiome surveillance and early dysbiosis forecasting after hematopoietic cell transplantation (HCT).

This repository includes data preprocessing, temporal sequence alignment, and training/evaluation code for an auto-regressive Bi-LSTM model plus static baseline models (XGBoost, Random Forest, Logistic Regression).

## Quickstart

For the fastest evaluation experience, run the benchmark in Google Colab with zero local setup.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/mlresearcher81/HCT-Microbiome-Benchmark/blob/main/notebooks/HCT_Microbiome_Benchmark.ipynb)

## What’s included

- `data/HCT_Microbiome_Benchmark.csv`: anonymized sample dataset
- `data/data_dictionary.md`: clinical and taxonomic feature descriptions
- `notebooks/HCT_Microbiome_Benchmark.ipynb`: end-to-end executable pipeline
- `requirements.txt`: dependency list for local execution

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
│   ├── raw/
│   ├── HCT_Microbiome_Benchmark.csv
│   └── Data Dictionary.md
├── data-preprocessing/
│   └── dataset_generation.ipynb
├── notebook/
│   └── benchmark_evaluation.ipynb
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
