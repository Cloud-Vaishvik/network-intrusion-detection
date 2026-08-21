# Network Intrusion Detection using Random Forest

## Overview

A machine-learning project for binary network intrusion detection. Network connections are classified as **Normal** or **Attack** using Random Forest.

The project demonstrates a complete ML workflow:

- Data loading and preparation
- Binary target creation
- Categorical feature encoding with `OneHotEncoder`
- Baseline Random Forest
- Randomized hyperparameter search with 3-fold cross-validation
- Test-set evaluation
- Confusion-matrix analysis
- Baseline vs tuned model comparison

## Dataset

The notebook expects the following files:

```text
data/
├── KDDTrain+.txt
└── KDDTest+.txt
```

The dataset files are intentionally not included in this repository. Add them locally before running the notebook.

## Methodology

Categorical features:

- `protocol_type`
- `service`
- `flag`

The remaining features are treated as numerical features and passed through unchanged.

The baseline model uses:

- Random Forest
- 100 estimators
- `random_state=42`

Hyperparameter tuning uses `RandomizedSearchCV` with:

- 10 sampled configurations
- 3-fold cross-validation
- F1 scoring
- `random_state=42`

## Results

| Model | Accuracy | Precision | Recall | F1 Score |
|---|---:|---:|---:|---:|
| Baseline Random Forest | 77.84% | 96.90% | 63.09% | 76.42% |
| Tuned Random Forest | 77.69% | 96.88% | 62.84% | 76.23% |

### Best hyperparameters

```text
n_estimators: 150
max_depth: 30
min_samples_split: 2
min_samples_leaf: 1
max_features: sqrt
```

### Confusion Matrix

For the tuned model:

```text
                 Predicted
              Normal  Attack

Actual Normal   9451     260
Actual Attack   4769    8064
```

The most important limitation is the **4,769 false negatives**: actual attacks that were classified as normal traffic. This explains the moderate attack recall of 62.84%.

## Interpretation

Hyperparameter tuning did not produce a meaningful overall improvement over the baseline. The tuned model's F1 score remained very close to the baseline, while attack recall decreased slightly.

This suggests that further improvement should focus on approaches beyond conventional hyperparameter search, such as:

- Class weighting
- Decision-threshold optimization
- Feature engineering
- Alternative machine-learning algorithms
- More targeted treatment of false negatives

## Repository Structure

```text
network-intrusion-detection/
├── README.md
├── network_intrusion_detection.ipynb
├── requirements.txt
├── .gitignore
├── data/
│   └── README.md
└── results/
    ├── model_comparison.png
    └── confusion_matrix.png
```

## How to Run

1. Clone the repository.
2. Install the dependencies:

```bash
pip install -r requirements.txt
```

3. Put `KDDTrain+.txt` and `KDDTest+.txt` in `data/`.
4. Open `network_intrusion_detection.ipynb`.
5. Run the notebook from top to bottom.

## Technologies

- Python
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
- Jupyter Notebook

## Project Status

**Completed — baseline modeling, hyperparameter tuning, evaluation, visualization, and error analysis.**
