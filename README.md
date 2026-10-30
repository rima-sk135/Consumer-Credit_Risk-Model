# Consumer Credit Risk & Expected Loss Model

## Project Overview

This project develops a consumer credit risk model using LendingClub loan data to estimate borrower default risk and portfolio Expected Loss.

Two machine-learning models are compared:

- Logistic Regression
- Random Forest

The project follows a simplified credit-risk framework based on:

**Expected Loss = Probability of Default (PD) × Loss Given Default (LGD) × Exposure at Default (EAD)**

## Objectives

The main objectives of the project are to:

- Clean and preprocess LendingClub loan data
- Identify completed loans and define a binary default target
- Explore factors associated with loan default
- Build Logistic Regression and Random Forest models
- Evaluate model performance using classification metrics, ROC-AUC, and Gini
- Estimate Probability of Default (PD)
- Estimate Loss Given Default (LGD) using recoveries
- Approximate Exposure at Default (EAD) using funded loan amount
- Calculate loan-level and portfolio Expected Loss

## Dataset

The project uses LendingClub consumer loan data.

Only loans with known final outcomes were retained:

- Fully Paid → Non-default (`0`)
- Charged Off → Default (`1`)
- Default → Default (`1`)

Ongoing loan statuses such as Current, Late, and In Grace Period were excluded from model development.

The raw dataset is not included in this repository due to its size.

## Data Preprocessing

The preprocessing workflow includes:

- Filtering loans with completed outcomes
- Creating a binary default target
- Handling missing values
- Converting relevant variables to numeric format
- One-hot encoding categorical variables
- Removing variables that could introduce post-default information leakage
- Splitting the data into training and testing sets
- Standardizing features for Logistic Regression

## Models

### Logistic Regression

Logistic Regression was selected because of its interpretability and widespread use in credit-risk modeling.

Because the dataset is imbalanced, class weighting was used to improve the model's ability to identify defaulted loans.

### Random Forest

A Random Forest classifier was developed as a nonlinear alternative to Logistic Regression.

The model was also trained using balanced class weights.

## Model Performance

| Metric | Logistic Regression | Random Forest |
|---|---:|---:|
| ROC-AUC | 0.7091 | 0.7108 |
| Gini | 41.81% | 42.16% |
| Default Recall | 68% | 67% |
| Default Precision | 31% | 31% |
| Default F1-Score | 43% | 43% |

The Random Forest produces a slightly higher ROC-AUC and Gini coefficient. However, the difference is small, while Logistic Regression offers greater interpretability.

An additional experiment without class weighting demonstrated why accuracy alone can be misleading for imbalanced credit-risk datasets. Although the unweighted Logistic Regression achieved approximately 80% accuracy, it identified only around 6% of actual defaults.

## Expected Loss Framework

Expected Loss is estimated as:

**EL = PD × LGD × EAD**

Where:

- **PD (Probability of Default):** estimated from the classification model
- **LGD (Loss Given Default):** estimated using observed recoveries on defaulted loans
- **EAD (Exposure at Default):** approximated using the funded loan amount

Expected Loss is calculated at the individual loan level and then aggregated to estimate portfolio-level credit loss.

## Important Limitation

The current Expected Loss calculation should be interpreted as a **preliminary estimate**.

The classification models use balanced class weights to improve default detection. As a result, their raw predicted probabilities are not necessarily calibrated real-world probabilities of default.

Probability calibration is therefore an important future enhancement before interpreting the Expected Loss estimates as final portfolio loss forecasts.

Additionally, EAD and LGD are simplified approximations for the purposes of this project.

## Key Findings

- Both models demonstrate moderate discriminatory power, with ROC-AUC of approximately 0.71.
- Random Forest provides only a marginal improvement over Logistic Regression.
- Class weighting substantially improves the detection of defaulted loans.
- Logistic Regression remains attractive for credit-risk applications because of its interpretability.
- Expected Loss provides a framework for translating borrower-level default risk into estimated portfolio credit losses.

## Future Improvements

Potential extensions include:

- Probability calibration for more reliable PD estimates
- More sophisticated LGD modeling
- More precise EAD estimation
- Hyperparameter tuning
- Feature engineering and selection
- Credit scorecard development
- Risk segmentation by PD bands
- Model validation and stability testing

## Technologies Used

- Python
- pandas
- NumPy
- scikit-learn
- Matplotlib
- hvPlot
- Jupyter Notebook

## Repository Structure

```text
consumer-credit-risk-model/
├── README.md
├── credit_risk_model.ipynb
├── requirements.txt
├── .gitignore
├── images/
│   ├── roc_curve.png
│   └── feature_importance.png
└── data/
    └── README.md
```

## Disclaimer

This project is intended for educational and portfolio purposes. The models and Expected Loss estimates should not be interpreted as production-ready credit-risk models or lending recommendations.

## Note: 
Some exploratory visualizations were created using hvPlot/HoloViews and are interactive when the notebook is executed locally. Static versions of key visualizations are included for compatibility with GitHub.
