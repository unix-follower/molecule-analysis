# Molecule Analysis

## Run
```bash
export DATA_DIR="$(pwd)/data"
export DB_URL='postgresql+psycopg_async://chemist:chemist@192.168.64.2:5432/molecule_analysis?options=-csearch_path%3Dmolecule_analysis,public'

uv sync
source .venv/bin/activate
uv run jupyter lab --allow-root --ip=0.0.0.0 --NotebookApp.allow_origin='*'
```
http://192.168.64.2:8888/lab?token=<token>

## Check/format code
```sh
uv tool run ruff check --fix
uv tool run ruff format
```

## Data

The `data/` directory contains MoleculeNet-style benchmark datasets spanning:
- binary classification: BBBP, ClinTox, HIV
- regression: Delaney, Lipophilicity, SAMPL, BACE
- multitask or multilabel classification: Tox21, SIDER, MUV, ToxCast
- quantum-property regression: QM8, QM9, `qm7b.mat`

## Current Baselines

The first validated benchmark is the scikit-learn BBBP baseline using character n-gram TF-IDF plus logistic regression.

Validation metrics on the current split:
- accuracy: `0.8762`
- F1: `0.9159`
- ROC AUC: `0.9471`

The first regression benchmark is Delaney ESOL using tabular descriptors.

Validation metrics on the current split:
- ridge: RMSE `0.8650`, MAE `0.6414`, R2 `0.8515`
- random forest: RMSE `0.6137`, MAE `0.4316`, R2 `0.9252`
- PyTorch MLP: RMSE `0.6656`, MAE `0.4771`, R2 `0.9120`
- TensorFlow MLP: RMSE `0.6523`, MAE `0.4588`, R2 `0.9155`

The regression extension to Lipophilicity uses the same split contract with SMILES-text features.

Validation metrics on the current split:
- ridge: RMSE `0.8099`, MAE `0.6340`, R2 `0.5520`
- elasticnet: RMSE `0.8493`, MAE `0.6650`, R2 `0.5074`

Cross-framework BBBP validation metrics:
- scikit-learn: accuracy `0.8762`, F1 `0.9159`, ROC AUC `0.9471`
- PyTorch: accuracy `0.7296`, F1 `0.8019`, ROC AUC `0.8103`
- TensorFlow: accuracy `0.8241`, F1 `0.8916`, ROC AUC `0.8546`

## Design Notes

- Use `mol_id`, `CID`, `CMPD_CHEMBLID`, or `Compound ID` as natural keys when available.
- Treat sparse multitask labels as nullable targets.
- Normalize QM8 duplicate headers before any modeling work.
- Use framework-matched task definitions so comparisons are meaningful.
