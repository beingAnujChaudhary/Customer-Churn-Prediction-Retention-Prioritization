# Experiment 02: XGBoost Hyperparameter Tuning

## Metadata
- **Date**: 2026-05-15 20:51
- **Purpose**: Optimize XGBoost via RandomizedSearchCV
- **Status**: ✅ Complete

## Results
| Metric | Baseline | Tuned | Δ |
|--------|----------|-------|---|
| AUC | 0.8734 | 0.9911 | +0.1177 |
| F1-Score | 0.7640 | 0.8968 | +0.1328 |

## Best Parameters
```python
{'subsample': 0.7, 'scale_pos_weight': 5.221966205837173, 'random_state': 42, 'n_estimators': 250, 'max_depth': 9, 'learning_rate': 0.1, 'eval_metric': 'auc', 'colsample_bytree': 0.8}
```

## CV Performance
- Best CV AUC: 0.9933
- CV Folds: 5 (Stratified)
- Iterations: 20

## Artifacts
- Model: `output/models/xgboost_tuned.pkl`
- Plot: `output/plots/tuning_comparison.png`
- Data: `data/processed/customers_v3.csv`

## Notes
- Used `scale_pos_weight` to handle class imbalance
- Search space aligned with README methodology
- Next: SHAP explainability on tuned model
