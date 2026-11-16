<div align="center">

# Credit Card Fraud Detection

Machine-learning notebook that detects fraudulent card transactions and compares three ways
of handling an extremely imbalanced dataset.

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?logo=scikitlearn&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?logo=jupyter&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green)

</div>

## Table of contents

- [Overview](#overview)
- [Dataset](#dataset)
- [Method](#method)
- [Results](#results)
- [Getting started](#getting-started)
- [Using the saved model](#using-the-saved-model)
- [Project structure](#project-structure)
- [License](#license)

## Overview

Fraud is rare, so a model that labels everything "normal" is already 99.8% accurate and
completely useless. This project shows why accuracy is the wrong yardstick here and how
**undersampling** and **SMOTE oversampling** change precision, recall and F1.

## Dataset

The notebook expects `creditcard.csv`, the public
[Credit Card Fraud Detection dataset](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)
from Kaggle (European cardholders, September 2013). It is not included in this repository.

| Property      | Value                                                    |
| ------------- | -------------------------------------------------------- |
| Rows          | 284,807 (275,663 after removing duplicates)              |
| Features      | `V1`–`V28` (PCA components), `Time`, `Amount`            |
| Target        | `Class`: 0 normal, 1 fraud                               |
| Class balance | 473 fraud against 275,190 normal after de-duplication    |

## Method

1. Load the data and check shape, types and missing values.
2. Standardise `Amount` with `StandardScaler` and drop `Time`.
3. Remove duplicate rows.
4. Split 80/20 into train and test sets (`random_state=42`).
5. Train **Logistic Regression** and a **Decision Tree** on each of three strategies:
   - the **imbalanced** data as is,
   - **undersampling**: 473 normal rows matched to 473 fraud rows,
   - **oversampling** with **SMOTE**: fraud synthesised up to 275,190 rows.
6. Compare accuracy, precision, recall and F1.
7. Fit the Decision Tree on the SMOTE data and save it with `joblib`.

## Results

| Strategy      | Model               | Accuracy | Precision | Recall | F1     |
| ------------- | ------------------- | -------- | --------- | ------ | ------ |
| Imbalanced    | Logistic Regression | 0.9992   | 0.887     | 0.604  | 0.719  |
| Imbalanced    | Decision Tree       | 0.9990   | 0.686     | 0.769  | 0.725  |
| Undersampling | Logistic Regression | 0.926    | 0.949     | 0.912  | 0.930  |
| Undersampling | Decision Tree       | 0.884    | 0.892     | 0.892  | 0.892  |
| SMOTE         | Logistic Regression | 0.944    | 0.973     | 0.913  | 0.942  |
| SMOTE         | Decision Tree       | 0.998    | 0.998     | 0.999  | 0.998  |

**Reading the table:** on the raw data accuracy looks excellent but recall shows that many
frauds are missed. Rebalancing lifts recall sharply. The saved model is the SMOTE Decision
Tree.

> **Caveat.** SMOTE was applied **before** the train/test split, so synthetic points derived
> from test-set neighbours leak into training. The SMOTE scores, especially the near-perfect
> Decision Tree, are therefore optimistic. For an honest estimate, split first and apply
> SMOTE to the training portion only.

## Getting started

```bash
git clone https://github.com/gitswagata1/CCradFD.git
cd CCradFD
pip install pandas numpy scikit-learn imbalanced-learn seaborn matplotlib joblib jupyter
# download creditcard.csv from Kaggle into this folder
jupyter notebook "Credit Card Fraud Detection.ipynb"
```

## Using the saved model

`credit_card_model.pkl` is a scikit-learn Decision Tree trained on the 29 features
`V1`–`V28` and standardised `Amount`, in that order.

```python
import joblib

model = joblib.load("credit_card_model.pkl")
prediction = model.predict([features])[0]   # features: list of 29 numbers
print("Fraud transaction" if prediction == 1 else "Normal transaction")
```

Load pickle files only from sources you trust, because unpickling can execute code. Use the
same scikit-learn version as the notebook where possible.

## Project structure

```
CCradFD/
├── Credit Card Fraud Detection.ipynb   Full analysis and training
├── credit_card_model.pkl               Trained Decision Tree
├── CHANGELOG.md
├── SECURITY.md
├── LICENSE
└── README.md
```

## License

Released under the [MIT License](LICENSE).

---

Built by **Swagata Banerjee** · [GitHub](https://github.com/gitswagata1)
