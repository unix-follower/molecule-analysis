# GitHub Copilot Instructions

## Project Context

- This repository is a notebook-first molecular machine learning workspace built around datasets in `data/`.
- Use Python 3.12 and the existing `uv` workflow defined in `pyproject.toml`.
- Preferred tooling already in the project includes Jupyter, pandas, NumPy, scikit-learn, PyTorch, TensorFlow, matplotlib, and seaborn.

## Working Style

- Prefer small, focused changes that preserve the current notebook progression and repository structure.
- Keep exploratory and presentation-heavy work in `notebooks/`.
- Put reusable Python logic in `src/` when code is shared across notebooks or becomes long enough to obscure the notebook narrative.
- Do not rename or renumber the existing notebooks unless explicitly asked.
- Keep examples and code cells reproducible from the repository root.

## Data Handling

- Treat files in `data/` as source datasets; do not overwrite or silently reshape raw files in place.
- Preserve dataset-specific columns and target semantics, especially for sparse multitask labels and molecular identifiers.
- When adding derived outputs, place them in a clearly named new file or directory rather than modifying the original dataset.
- Prefer explicit train, validation, and test split handling so notebook metrics remain comparable across frameworks.

## Modeling Guidance

- Match the task type to the dataset: classification, regression, multitask classification, or quantum-property regression.
- Keep framework comparisons fair by using comparable splits, features, and evaluation metrics.
- Make metrics explicit in code and markdown, especially for BBBP, Delaney, and Lipophilicity baselines already documented in the README.
- Favor clear baseline implementations before introducing more complex molecular graph or message-passing models.

## Code Preferences

- Prefer existing dependencies over adding new packages unless there is a clear need.
- Follow the repository's current simple Python style: clear names, minimal abstraction, and straightforward data pipelines.
- Use relative paths rooted at the repository when loading local data.
- If a notebook grows repetitive, extract helper functions into `src/` instead of duplicating logic across notebooks.
- Update README notes when adding a meaningful new notebook, baseline, or workflow.

## Validation

- For Python changes, prefer lightweight validation that matches the scope of the edit.
- If you add or change modeling code, verify imports, data loading paths, and metric computation logic.
- Avoid broad refactors unrelated to the requested task.
