# 🚚 Predictive Maintenance Classification for a Delivery Company

## Overview

This project develops a **binary classification model for predictive maintenance** using device measurement data.

The objective is to predict whether a device will experience a failure:

- `0` = No Failure
- `1` = Failure

The main challenge is the **extreme class imbalance** in the dataset. Because failures are very rare, overall Accuracy alone is not sufficient for evaluating model performance.

The analysis therefore focuses especially on:

- Precision
- Recall
- F1 Score
- Confusion Matrix

for the **Failure class**.

---

## Business Objective

For a delivery company, unexpected device failures can lead to operational delays, additional costs, and service interruptions.

The goal of this project is to use historical device measurements to identify potential failures and support **preventive maintenance decisions**.

A useful model should not only classify normal observations correctly but also detect rare failure cases.

---

## Dataset

The project uses the dataset:

`failure.csv`

Dataset size:

- **124,494 observations**
- **12 columns**

The dataset contains:

- `date`
- `device`
- `failure`
- `attribute1`
- `attribute2`
- `attribute3`
- `attribute4`
- `attribute5`
- `attribute6`
- `attribute7`
- `attribute8`
- `attribute9`

The target variable is:

`failure`

---

## Class Distribution

The dataset is extremely imbalanced:

| Class | Observations | Percentage |
| --- | ---: | ---: |
| No Failure (`0`) | 124,388 | 99.9149% |
| Failure (`1`) | 106 | 0.0851% |

This imbalance makes Accuracy potentially misleading.

A model that predicts almost every observation as **No Failure** could still achieve very high Accuracy while failing to detect actual failures.

---

## Project Workflow

```text
Raw Device Data
↓
Data Inspection
↓
Exploratory Data Analysis
↓
Feature / Target Preparation
↓
Stratified Train-Test Split
↓
StandardScaler
↓
SMOTE on Training Data Only
↓
Classification Model Comparison
↓
Failure Precision / Recall / F1 Score
↓
Final Model Selection
↓
Confusion Matrix
↓
Feature Importance
↓
Predictive Maintenance Interpretation
```

---

## Data Preparation

The following preprocessing steps were applied:

1. The dataset was inspected for shape, missing values, and duplicates.
2. `failure` was defined as the target variable.
3. `date` and `device` were excluded from the basic numerical classification model.
4. The original dataset was divided into training and test sets.
5. A **stratified train-test split** was used to preserve the original failure ratio.
6. Numerical features were standardized using `StandardScaler`.

The scaler was fitted only on the training data and then applied to the test data.

---

## Handling Class Imbalance with SMOTE

**SMOTE — Synthetic Minority Over-sampling Technique** was used to address the highly imbalanced target variable.

Instead of simply duplicating minority-class observations, SMOTE creates synthetic observations based on existing minority-class examples.

In this project, SMOTE was applied **only to the training dataset after the train-test split**.

This is important because applying SMOTE before splitting the data could introduce **data leakage** and produce overly optimistic model performance.

Reference:

Chawla, N. V., Bowyer, K. W., Hall, L. O., & Kegelmeyer, W. P. (2002).  
*SMOTE: Synthetic Minority Over-sampling Technique*.  
Journal of Artificial Intelligence Research, 16, 321–357.

---

## Classification Models

Several classification algorithms were compared:

- Gaussian Naive Bayes
- Logistic Regression
- Decision Tree Classifier
- Random Forest Classifier
- Gradient Boosting Classifier
- AdaBoost Classifier

The models were trained using the SMOTE-balanced training dataset and evaluated using the unchanged test dataset.

---

## Evaluation Metrics

Because the Failure class is extremely rare, model evaluation focuses especially on the minority class.

### Accuracy

Measures the overall percentage of correct predictions.

However, Accuracy can be misleading when one class dominates the dataset.

### Precision

Measures how many observations predicted as failures were actually failures.

### Recall

Measures how many actual failures were correctly identified.

For predictive maintenance, Recall is particularly important because a **False Negative represents a real failure that was not detected**.

### F1 Score

Combines Precision and Recall into one metric.

The final model was selected primarily using the **Failure F1 Score**, while also considering Precision and Recall.

---

## Model Comparison

| Model | Accuracy | Failure Precision | Failure Recall | Failure F1 |
| --- | ---: | ---: | ---: | ---: |
| **Random Forest** | **0.9987** | **0.1333** | 0.0952 | **0.1111** |
| Decision Tree | 0.9980 | 0.0882 | 0.1429 | 0.1091 |
| Gaussian Naive Bayes | 0.9882 | 0.0243 | 0.3333 | 0.0453 |
| Logistic Regression | 0.9665 | 0.0120 | 0.4762 | 0.0234 |
| Gradient Boosting | 0.9660 | 0.0107 | 0.4286 | 0.0208 |
| AdaBoost | 0.9286 | 0.0078 | **0.6667** | 0.0155 |

The results demonstrate the trade-off between identifying more failures and generating more false-positive predictions.

---

## Selected Model

The **Random Forest Classifier** achieved the highest Failure F1 Score among the tested models and was selected as the final model.

### Final Test Results

- **Accuracy:** 0.9987
- **Failure Precision:** 0.1333
- **Failure Recall:** 0.0952
- **Failure F1 Score:** 0.1111

The test set contained only **21 actual failure observations**, which makes reliable failure detection particularly challenging.

---

## Confusion Matrix

A confusion matrix was used to analyze the final predictions.

It distinguishes between:

- **True Negatives:** No Failure correctly predicted
- **False Positives:** Failure predicted although no failure occurred
- **False Negatives:** Actual failures that were missed
- **True Positives:** Failures correctly detected

For predictive maintenance, **False Negatives are particularly important**, because they represent failures that were not identified in advance.

---

## Feature Importance

Feature importance from the selected Random Forest model was also analyzed to understand which device attributes contributed most strongly to the model's predictions.

This provides additional interpretability and can help identify measurements that may be relevant for future predictive-maintenance analysis.

---

## Key Findings

The project highlights an important issue in real-world classification:

> **High Accuracy does not necessarily mean that a model performs well on the class that matters most.**

Although the Random Forest model achieved approximately **99.87% overall Accuracy**, its Failure Recall was only **9.52%**.

This indicates that the model classified normal observations very effectively but still missed many actual failures.

The extreme rarity of the Failure class is therefore the central modeling challenge.

---

## Possible Improvements

Future improvements could include:

- collecting more actual failure observations
- additional feature engineering
- incorporating temporal information
- hyperparameter tuning
- decision-threshold optimization
- cost-sensitive learning
- repeated or cross-validated model evaluation
- comparison with additional imbalance-handling techniques

These approaches could improve the model's ability to detect rare failures.

---

## Technologies

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- imbalanced-learn
- SMOTE
- Jupyter Notebook

---

## Conclusion

This project demonstrates a complete machine-learning workflow for **predictive maintenance classification**.

The main challenge was the extreme imbalance between normal device observations and actual failures.

To address this:

- the original data was split before resampling,
- numerical features were standardized,
- SMOTE was applied only to the training data,
- multiple classification algorithms were compared,
- and model selection focused on Failure Precision, Recall, and F1 Score rather than Accuracy alone.

The **Random Forest Classifier** achieved the highest Failure F1 Score among the tested models.

At the same time, the low Failure Recall shows that rare-event detection remains difficult with the available data.

Overall, the project demonstrates why **class imbalance handling and appropriate evaluation metrics are essential when building predictive-maintenance classification systems**.
