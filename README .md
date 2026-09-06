# Network Intrusion Detection System (NIDS) — CICIoT2023 DDoS Detection

## Overview

This project implements a machine-learning-based **Network Intrusion Detection System (NIDS)** for detecting **DDoS attacks** using a subset of the **CICIoT2023 dataset**.

The system classifies network traffic into:

- **Benign traffic**
- **DDoS traffic**

Multiple CICIoT2023 DDoS attack subtypes are combined into a single binary DDoS class.

---

## Dataset

The selected CICIoT2023 subset includes:

- Benign traffic
- DDoS-ACK Fragmentation
- DDoS-HTTP Flood
- DDoS-ICMP Flood
- DDoS-ICMP Fragmentation
- DDoS-PSHACK Flood
- DDoS-RSTFIN Flood
- DDoS-SlowLoris
- DDoS-SYN Flood
- DDoS-SynonymousIP Flood
- DDoS-TCP Flood
- DDoS-UDP Flood
- DDoS-UDP Fragmentation

### Dataset Sampling

To create a manageable experimental dataset:

- Benign traffic is sampled from the selected benign CSV.
- A fixed number of records is sampled from each DDoS subtype.
- All selected records are combined into one dataset.
- The final dataset is shuffled using a fixed random seed for reproducibility.

The dataset itself is excluded from GitHub because of its size.

---

## Features and Target

The machine-learning dataset contains **39 numerical network-flow features**.

Two target-related columns are used:

- `binary_label`
  - `0` → Benign
  - `1` → DDoS

- `attack_type`
  - Stores the original attack subtype for subtype-level analysis.

Both target-related columns are excluded from the feature matrix used for model training.

---

## Methodology

### 1. Data Loading and Cleaning

The notebook:

- Loads the selected CICIoT2023 CSV files.
- Cleans column names.
- Replaces positive and negative infinity values with missing values.
- Samples the required records.
- Adds binary and attack subtype labels.
- Combines all selected traffic into one dataset.

### 2. Leakage-Resistant Train/Test Split

A standard row-level random split can allow identical network-flow feature patterns to appear in both training and testing data.

To prevent this, the project uses a **duplicate-grouped train/test split**:

- Each unique feature pattern is identified using a hash.
- Unique feature patterns are split into training and testing groups.
- All occurrences of the same feature pattern remain entirely in either the training set or testing set.
- This prevents exact feature duplicates from crossing the train/test boundary.

The split is approximately:

- **80% training data**
- **20% testing data**

while preserving class proportions.

### 3. Preprocessing

A Scikit-learn preprocessing pipeline is used before model training.

### 4. Machine Learning Models

The notebook includes:

- Baseline Random Forest
- Class-Weighted Random Forest
- Hyperparameter-Tuned Random Forest using `RandomizedSearchCV`

### 5. Evaluation

Models are evaluated using:

- Accuracy
- Precision
- Recall
- F1 Score
- ROC-AUC
- Confusion Matrix

---

## Verified Baseline Result

Using the duplicate-grouped evaluation split, the baseline Random Forest produced:

| Actual / Predicted | Benign | DDoS |
|---|---:|---:|
| **Benign** | 70,104 | 0 |
| **DDoS** | 4 | 129,238 |

### Test Set Summary

- **Total test samples:** 199,346
- **Incorrect predictions:** 4
- **Correct predictions:** 199,342
- **Approximate accuracy:** **99.998%**

The final split was also verified to ensure that **0 identical feature rows were shared between the training and testing sets**.

---

## Project Structure

```text
network-intrusion-detection/
│
├── network_intrusion_detection.ipynb
├── README.md
├── requirements.txt
├── .gitignore
├── confusion_matrix.png
├── model_comparison.png
│
└── data/
    └── CICIoT2023 dataset files (not included in GitHub)
```

---

## Installation

Clone the repository:

```bash
git clone https://github.com/Cloud-Vaishvik/network-intrusion-detection.git
cd network-intrusion-detection
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Launch Jupyter:

```bash
jupyter notebook
```

Then open:

```text
network_intrusion_detection.ipynb
```

---

## Requirements

The project uses:

- pandas
- numpy
- scikit-learn
- matplotlib
- jupyter

See `requirements.txt` for the package versions.

---

## How to Run

1. Download the required CICIoT2023 CSV files.
2. Create a `data` folder inside the project directory.
3. Place the selected dataset files inside the appropriate dataset folder.
4. Open `network_intrusion_detection.ipynb`.
5. Run the notebook from top to bottom.

The dataset is intentionally excluded from Git tracking.

---

## Visualizations

The repository includes:

- `model_comparison.png` — comparison of evaluated models.
- `confusion_matrix.png` — confusion matrix for the evaluated classifier.

---

## Key Learning Outcomes

This project demonstrates:

- Working with a modern cybersecurity dataset.
- Network traffic classification.
- Binary DDoS detection.
- Large CSV dataset handling.
- Scikit-learn preprocessing pipelines.
- Random Forest classification.
- Class weighting.
- Hyperparameter optimization with RandomizedSearchCV.
- Leakage-resistant evaluation.
- Performance evaluation using multiple metrics.
- Confusion matrix analysis.

---

## Future Improvements

Potential extensions include:

- Testing on completely unseen traffic captures or environments.
- Evaluating additional machine-learning models.
- Building a real-time network monitoring pipeline.
- Creating a web dashboard for live intrusion detection.
- Extending the project to multi-class attack classification.

---

## Author

**Vaishvik**

GitHub: https://github.com/Cloud-Vaishvik
