# Pipeline-MLOPS

End-to-end example ML pipeline using DVC and dvclive for reproducible experiments.

## Overview

This repository contains a small, well-structured machine learning pipeline that demonstrates data ingestion, preprocessing, feature engineering, model training, and evaluation. The project uses DVC for data and pipeline orchestration and dvclive for lightweight experiment metrics.

Key components:
- Data management: `data/` (raw, interim, processed)
- Pipeline definitions: `dvc.yaml` and `params.yaml`
- Pipeline code: scripts in `src/`
- Outputs: `models/`, `dvclive/`, `reports/`

## Repository structure

- `dvc.yaml` - DVC pipeline definition
- `params.yaml` - Parameter values used by the pipeline
- `data/` - dataset folders
  - `raw/` - raw source files (train.csv, test.csv)
  - `interim/` - intermediate files used during processing
  - `processed/` - processed features / tfidf outputs
- `dvclive/` - experiment metrics and plots
- `models/` - saved model artifacts
- `reports/` - aggregated metrics and reports
- `src/` - pipeline scripts
  - [src/data_ingestion.py](src/data_ingestion.py)
  - [src/data_preprocessing.py](src/data_preprocessing.py)
  - [src/feature_engineering.py](src/feature_engineering.py)
  - [src/model_building.py](src/model_building.py)
  - [src/model_evaluation.py](src/model_evaluation.py)

## Prerequisites

- Python 3.8+
- git
- DVC (https://dvc.org)
- (Optional) Conda or virtualenv to manage dependencies

Create and activate a virtual environment (example):

Windows (PowerShell):

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

macOS / Linux (bash):

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Note: a `requirements.txt` file is not included by default — add one listing packages your scripts require (e.g., pandas, scikit-learn, dvc, dvclive).

## Configuration

- Edit `params.yaml` to change hyperparameters and pipeline options.
- `dvc.yaml` defines the pipeline stages and dependencies.

## Quickstart — reproduce the pipeline

If DVC remote storage is configured and data tracked, you can reproduce the full pipeline with:

```bash
# pull tracked data/artifacts if remote is used
dvc pull

# run the pipeline (reproduces stages in dvc.yaml)
dvc repro
```

Alternatively run steps manually (order matters):

```bash
python src/data_ingestion.py
python src/data_preprocessing.py
python src/feature_engineering.py
python src/model_building.py
python src/model_evaluation.py
```

Each script reads from `data/raw` (or upstream outputs) and writes its outputs under `data/interim` or `data/processed`, models under `models/`, and metrics under `dvclive/` and `reports/`.

## Data

- Raw input: [data/raw/train.csv](data/raw/train.csv), [data/raw/test.csv](data/raw/test.csv)
- Processed outputs: [data/processed/train_tfidf.csv](data/processed/train_tfidf.csv), [data/processed/test_tfidf.csv](data/processed/test_tfidf.csv)

Be mindful of sensitive data — do not commit secrets to git or push private data to public remotes.

## Metrics & experiments

- dvclive stores per-run metrics in `dvclive/metrics.json` and TSV plots in `dvclive/plots/metrics/`.
- Aggregate metrics are stored in `reports/metrics.json`.

## Models and artifacts

- Trained model files are saved in `models/` after running `src/model_building.py`.
- Use `dvc add` to track large model files and push them to remote storage.

## Development notes

- Scripts are simple, single-purpose modules under `src/`. Read and modify them to adapt the pipeline stages.
- To add a new pipeline stage, update `dvc.yaml` and include the appropriate command and dependencies.

## Common commands

- Run tests (if present): `pytest`
- Show DVC metrics: `dvc metrics show`
- Show DVC pipeline DAG: `dvc dag`

## Contributing

1. Fork the repository
2. Create a branch for your feature or fix
3. Open a pull request with a clear description of changes

## License

This project is provided under the terms of the LICENSE file in the repository.

---

If you'd like, I can also generate a `requirements.txt` and add badge and usage examples for CI/CD or Docker.
