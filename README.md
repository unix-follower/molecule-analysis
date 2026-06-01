## Molecule Analysis

This project is a notebook-first study and implementation track for molecular machine learning on the datasets in `data/`.

The current focus is foundations first:
- dataset audit and task profiling
- math requirements for modeling
- baseline ML notebooks in scikit-learn, PyTorch, and TensorFlow
- graph-theory requirements for later molecular and assay graph work

## Environment

This project uses `uv` and Python 3.12.

Setup:

```bash
cd /Users/Artsem_Nikitsenka/molecule-analysis
uv sync
```

Run Jupyter:

```bash
cd /Users/Artsem_Nikitsenka/molecule-analysis
uv run jupyter lab
```

## Data

The `data/` directory contains MoleculeNet-style benchmark datasets spanning:
- binary classification: BBBP, ClinTox, HIV
- regression: Delaney, Lipophilicity, SAMPL, BACE
- multitask or multilabel classification: Tox21, SIDER, MUV, ToxCast
- quantum-property regression: QM8, QM9, `qm7b.mat`

## Notebook Order

Start here:

1. `notebooks/01_data_audit.ipynb`
2. `notebooks/02_math_requirements.ipynb`
3. `notebooks/03_ml_requirements.ipynb`
4. `notebooks/04_graph_theory_requirements.ipynb`

Current implementation notebooks:

5. `notebooks/05_sklearn_bbbp_baseline.ipynb`
6. `notebooks/06_sklearn_delaney_regression.ipynb`
7. `notebooks/07_pytorch_bbbp_baseline.ipynb`
8. `notebooks/08_tensorflow_bbbp_baseline.ipynb`
9. `notebooks/09_framework_comparison.ipynb`
10. `notebooks/10_pytorch_delaney_regression.ipynb`
11. `notebooks/11_tensorflow_delaney_regression.ipynb`
12. `notebooks/12_graph_representations.ipynb`
13. `notebooks/13_graph_learning_intuition.ipynb`
14. `notebooks/14_sklearn_lipophilicity_regression.ipynb`

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

- Use small, clean datasets first before scaling to sparse wide datasets like ToxCast.
- Use `mol_id`, `CID`, `CMPD_CHEMBLID`, or `Compound ID` as natural keys when available.
- Treat sparse multitask labels as nullable targets.
- Normalize QM8 duplicate headers before any modeling work.
- Use framework-matched task definitions so comparisons are meaningful.

## Next Work

1. Add a matched results export workflow so notebook metrics can be refreshed automatically.
2. Add a true graph-model notebook after the current message-passing intuition notebook.
3. Add deeper benchmarks for SAMPL and Tox21.
