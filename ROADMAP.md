# Roadmap

## Vision

A clear, reproducible reference for handling extreme class imbalance in fraud detection, with honest evaluation.

## Milestones

### Now

- [ ] Fix leakage: split first, then apply SMOTE to the training set only, and re-report the metrics
- [ ] Add `requirements.txt` with pinned versions
- [ ] Report precision-recall AUC and a confusion matrix instead of accuracy

### Next

- [ ] Add gradient boosting (XGBoost, LightGBM) and class-weighted baselines
- [ ] Threshold tuning against a cost matrix
- [ ] Wrap the saved model in a small Streamlit or FastAPI app

### Later

- [ ] Model card and fairness review
- [ ] Drift monitoring and retraining notes
- [ ] Reproducible pipeline with DVC or MLflow

## Where this can go

| Direction | Idea |
| --------- | ---- |
| **Startup** | A fraud-scoring API for small payment businesses that cannot afford enterprise tooling. |
| **Publication** | A short comparative study of resampling methods under correct evaluation; replication notes for the Kaggle dataset. |

> **Intellectual property.** If you intend to patent a novel method, file before disclosing it publicly: public disclosure can bar patentability in many jurisdictions. Consult a patent attorney.

## How to help

See [CONTRIBUTING.md](CONTRIBUTING.md) and the issues labelled `good first issue`.
