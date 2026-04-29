
# Credit Risk Prediction and Expected Loss Analysis

## 1. Project Overview

This project builds a credit risk prediction model to estimate borrower default probability using loan-level applicant data. The goal is not only to predict default risk, but also to translate model outputs into business decisions such as risk segmentation, expected loss estimation, and approval strategy simulation.

The project follows a complete credit risk analytics workflow:

1. Data cleaning and preprocessing
2. Exploratory data analysis
3. Feature engineering
4. Logistic Regression baseline model
5. Random Forest model
6. Risk band segmentation
7. Expected loss analysis using PD × LGD × EAD
8. Approval strategy simulation

---

## 2. Dataset

The dataset contains borrower and loan-level features, including:

- Borrower age
- Borrower income
- Home ownership status
- Employment length
- Loan intent
- Loan grade
- Loan amount
- Loan interest rate
- Loan percent income
- Historical default indicator
- Credit history length
- Loan status

The target variable is:

- `loan_status = 1`: Default / high-risk borrower
- `loan_status = 0`: Non-default borrower

After data cleaning, the final dataset contains 32,409 observations.

---

## 3. Exploratory Data Analysis

Key EDA findings:

- Default rate increases sharply from Grade A to Grade G.
- Borrowers who rent show higher default rates than borrowers who own homes or have mortgages.
- Debt consolidation, medical loans, and home improvement loans have relatively higher default rates.
- Defaulted borrowers tend to have higher interest rates.
- Defaulted borrowers also tend to have higher loan-to-income ratios.
- The dataset is imbalanced, with default borrowers representing about 21.87% of the cleaned sample.

These findings suggest that loan grade, interest rate, home ownership, loan purpose, and debt burden are important risk indicators.

---

## 4. Feature Engineering

Several business-driven features were created:

- `loan_to_income_ratio = loan_amnt / person_income`
- `income_per_year_employed = person_income / (person_emp_length + 1)`
- `high_loan_percent_income`
- `high_interest_rate`

Categorical variables were converted into dummy variables using one-hot encoding.

A stratified train-test split was used to preserve the default rate distribution in both training and testing datasets.

---

## 5. Model Performance

Two models were trained and compared:

| Model | Accuracy | Precision | Recall | F1 Score | AUC |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 0.813 | 0.551 | 0.786 | 0.648 | 0.880 |
| Random Forest | Higher than baseline | Strong performance | Strong performance | Strong performance | 0.923 |

The Logistic Regression model provides an interpretable baseline, while the Random Forest model achieves stronger predictive performance by capturing nonlinear risk patterns.

The Random Forest model achieved an AUC of 0.923, indicating strong ability to distinguish between default and non-default borrowers.

---

## 6. Feature Importance

The top features from the Random Forest model include:

- Loan-to-income ratio
- Loan interest rate
- Loan percent income
- Borrower income
- Loan grade
- Home ownership status
- High interest rate indicator
- High loan burden indicator
- Loan amount
- Employment length

These features are consistent with credit risk intuition: borrowers with higher debt burden, higher interest rates, lower income, and weaker loan grades tend to have higher default risk.

---

## 7. Risk Band Segmentation

Predicted default probabilities were converted into five risk bands:

| Risk Band | Actual Default Rate | Average Predicted PD |
|---|---:|---:|
| Very Low Risk | 0.00% | 3.96% |
| Low Risk | 0.65% | 7.80% |
| Medium Risk | 3.71% | 15.32% |
| High Risk | 9.24% | 26.25% |
| Very High Risk | 55.96% | 69.63% |

The actual default rate increases clearly across risk bands, showing that the model can effectively rank borrowers by credit risk.

---

## 8. Expected Loss Analysis

Expected Loss was calculated using the formula:

Expected Loss = PD × LGD × EAD

Assumptions:

- PD = predicted default probability from the Random Forest model
- LGD = 45%
- EAD = loan amount

Average expected loss per borrower increases significantly across risk bands:

| Risk Band | Average Expected Loss |
|---|---:|
| Very Low Risk | 158.73 |
| Low Risk | 329.36 |
| Medium Risk | 657.42 |
| High Risk | 977.54 |
| Very High Risk | 3536.26 |

This converts model predictions into a business-relevant credit loss estimate.

---

## 9. Approval Strategy Simulation

A simple approval strategy was designed based on predicted default probability:

| Predicted PD | Decision |
|---:|---|
| Below 20% | Approve |
| 20% to 35% | Manual Review |
| 35% or above | Reject |

Decision summary:

| Decision | Borrower Count | Actual Default Rate | Total Expected Loss |
|---|---:|---:|---:|
| Approve | 2686 | 2.68% | 1,457,519 |
| Manual Review | 1666 | 9.24% | 1,628,589 |
| Reject | 2130 | 55.96% | 7,532,228 |

Strategy impact:

- Baseline expected loss: 10,618,336
- Strategy expected loss after rejecting highest-risk borrowers: 3,086,108
- Expected loss reduction: 7,532,228
- Expected loss reduction percentage: 70.96%
- Borrowers rejected: 2,130
- Borrowers rejected percentage: 32.86%

This shows that rejecting the highest-risk segment can substantially reduce expected credit loss.

---

## 10. Business Recommendations

Based on the analysis, the following credit risk actions are recommended:

1. Automatically approve borrowers with predicted PD below 20%, subject to standard compliance checks.
2. Send borrowers with predicted PD between 20% and 35% to manual review.
3. Reject or apply stricter underwriting rules to borrowers with predicted PD above 35%.
4. Monitor high-risk borrowers using loan-to-income ratio, interest rate, and loan grade as key early-warning indicators.
5. Use expected loss estimates to support risk-based pricing, credit limit decisions, and portfolio monitoring.

---

## 11. Skills Demonstrated

This project demonstrates:

- Python data analysis
- Data cleaning
- Exploratory data analysis
- Feature engineering
- Classification modeling
- Logistic Regression
- Random Forest
- Model evaluation using AUC, precision, recall, F1-score, and confusion matrix
- Risk segmentation
- Expected loss modeling
- Credit risk decision strategy
- Business interpretation of machine learning models

---

## 12. Summary

In this project, I built a credit default prediction model using borrower-level loan data. I started with data cleaning and EDA, then engineered several risk-related features such as loan-to-income ratio and high-interest-rate indicators. I trained a Logistic Regression baseline model and a Random Forest model. The Random Forest model achieved an AUC of 0.923, outperforming the Logistic Regression baseline.

After estimating each borrower's probability of default, I converted the predictions into five risk bands and calculated expected loss using a simplified PD × LGD × EAD framework. Finally, I simulated an approval strategy where low-risk borrowers were approved, medium-risk borrowers were sent to manual review, and high-risk borrowers were rejected. The strategy reduced estimated expected loss by about 70.96% while rejecting about 32.86% of borrowers.
