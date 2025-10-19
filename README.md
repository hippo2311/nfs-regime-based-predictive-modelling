# nfs-regime-based-predictive-modelling

## Overview
Market regimes, defined by macroeconomic, financial, and geopolitical conditions, influence the performance of equity factors. By identifying the current regime and comparing it to historical periods, we can predict factor
returns and generate alpha with a long-short factor portfolio.

## Methodology
### Macroeconomic Variable Selection
- Choose variables that have strongly influence over equity factors.
- Filter by relevance using historical correlation with factor returns, and remove highly collinear variables.

### Feature Construction
- For each selected macro variable, construct three features:
  - Level: Normalised current value
  - Trend: Recent direction over the past 12 months
  - Volatility: Historical variability over the past 12 months

### Regime Identification
- Construct a feature vector for the current month using all macro features.
- Compute similarity to historical months using Euclidean distance.
- Identify most similar historical months for predictions and least similar for robustness checks.

### Factor Return Prediction
- For each equity factor [Market, Size, Value, Momentum]
  - Compute average return in most similar historical months.
  - Predict next-period direction: long if positive, short if negative.

### Long-Short Factor Portfolio
- Take independent long/short positions per factor based on predictions.
- Optionally weight positions by similarity score or inverse factor volatility.
- Portfolio is market-neutral with long predicted winners and short predicted losers.

### Performance Evaluation
- Track factor portfolio returns and evaluate predictive power via: Hit rate, information coefficient (IC), Sharpe ratio / alpha
- Compare performance during most similar vs least similar regimes

## Development Details
### Branching and Naming Standards
For any changes, create a separate branch and follow:

`[type-of-change]/[name-of-change]`, e.g. `data/add-dataset` or `model/init-regime-model` or `feat/similarity-weights`

Once changes are final, create a pull request and merge to master.

### Commits and PRs
Commit messages follow the following structure:
- `feat(model): add volatility feature for macro series`
- `fix(eval): correct IC calculation window`

### Codebase Structure

```
/configs/              # YAML configs (data, features, regime, eval, portfolio)
/data/
  raw/                 # untouched downloads (gitignored)
    macro_data.csv
    factor_returns.csv
  processed/           # model-ready features (gitignored)
    merged_data.csv
/notebooks/
  exploratory_analysis.ipynb
  feature_validation.ipnyb
  performance_eval.ipnyb
/src/
  data_loader.py
  feature_engineering.py
  factor_prediction.py
  regime_model.py
  evaluation.py
/docs/
  literature_review.md
  project_scope.md
  methodology_draft.md
  slides.pdf
/tests/                # pytest
.gitignore
makefile
requirements.txt
```
