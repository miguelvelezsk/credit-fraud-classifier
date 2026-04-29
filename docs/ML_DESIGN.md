# ML Design Document

## Problem Statement

Incidents of identity fraud, which often involve credit card fraud, cost financial institutions and consumers billions of dollars annually. It’s important to protect our billing accounts, and both parties share the responsibility: banks and users have to use good practices to protect.

But, it is a hard challenge, because fraudsters are increasingly sophisticated and difficult to detect. Additionally, fraud datasets are highly imbalanced **since** 99% of transactions are legitimate. This is why a Machine Learning model becomes a critical tool to detect and prevent fraud in credit cards, predicting fraud cases thanks to information such as: transaction date, amount, transaction type, and location.

## Data Dictionary

- **TransactionID:**
    - **Type:** Integer
    - **Description:** A unique identifier for each transaction
    - **Range:** 1 - 100000
- **TransactionDate:**
    - **Type:** Date
    - **Description:** The date and time when the transaction occurred. The timestamps are randomly generated within the last year.
    - **Range:** 2023-10-21 - 2024-10-21
- **Amount:**
    - **Type:** Float
    - **Description:** The amount of money for each transaction
    - **Range:** 1.05 - 5000
- **MerchantID:**
    - **Type:** Integer
    - **Description:** A unique identifier for the merchant where the transaction took place
    - **Range:** 1 - 1000
- **TransactionType:**
    - **Type:** String
    - **Description:** The type of transaction
    - **Values:** purchase | refund
- **Location:**
    - **Type:** String
    - **Description:** The geographic location where the transaction occurred. The dataset includes multiple major cities across the U.S.
    - **Range:** 10 unique cities in U.S.
- **IsFraud:**
    - **Type:** Integer
    - **Description:** A binary indicator of whether the transaction is fraudulent (1) or not (0). About 1% of the transactions are marked as fraudulent.
    - **Values:** 0 | 1

## Feature Engineering Plan

- Include one-hot encoding to `Location`  and `TransactionType`
- Extract from `TransactionDate:`
    - **DayOfWeek:** Day of the week (0 - 6)
    - **IsWeekend:** A binary value, 1 if is a weekend, 0 for weekdays
    - **Month:** Month of the year (1 - 12)
- Drop `TransactionID` and `MerchantID` since they are arbitrary identifiers that carry no predictive value

## Model Selection

### Logistic regression

This model serves as a simple baseline, and it is well-suited for binary classification, useful for our dataset because it has just 2 values, 1 or 0 (fraud or no fraud)

### **Random forest**

This model handles non-linear relationships well, and categorical variables with one-hot encoding, useful for `Location`  and `TransactionType`

The hypothesis is that Random forest is the best model for this problem, due to that the variables are non-linearly dependent and the dataset is highly imbalanced (1% fraud rate)

## Evaluation Metrics

### F1-Score

An important metric, since we don't want false positives nor false negatives. If we block the card of a legitimate client, the user is going to be mad with the bank, and it damages the bank’s reputation. On the other hand, if we don’t detect a fraud, a fraudster will steal other people’s money

### ROC-AUC

An important metric to measure how well the model separates the two classes.