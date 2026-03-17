# Audit Risk Analysis by Accounting Cycle

## Project Overview

This project analyzes a structured database of **audit risks classified
by accounting cycle**.\
The objective is to identify **high-risk areas**, evaluate **audit
assertions**, and build a **risk matrix to support audit planning**.

The methodology follows international auditing standards:

-   ISA 315 -- Identifying and Assessing Risks
-   ISA 330 -- Auditor's Responses to Assessed Risks

The analysis was implemented using **Python and Google Colab**.

------------------------------------------------------------------------

# Dataset Description

The dataset contains structured information about **audit risks across
accounting cycles**.

## Dataset Structure

  Column                  Description
  ----------------------- ----------------------------------
  ID                      Risk identifier
  Cycle comptable         Accounting cycle
  Processus               Operational process
  Compte concerné         Related accounts (Moroccan GAAP)
  Description du risque   Description of the audit risk
  Type de risque          Inherent / Control / Fraud
  Assertion impactée      Audit assertion affected
  Niveau de risque        Risk level
  Procédure d'audit       Recommended audit procedure
  Documents de preuve     Supporting documents

------------------------------------------------------------------------

# Accounting Cycles Covered

The dataset includes risks related to the following cycles:

-   Sales and Accounts Receivable
-   Purchases and Accounts Payable
-   Treasury
-   Fixed Assets
-   Inventory
-   Payroll
-   Taxation
-   Equity

These cycles represent the **main areas typically evaluated during
financial audits**.

------------------------------------------------------------------------

# Data Preparation

Data preprocessing steps included:

-   removing duplicates
-   handling missing values
-   cleaning text columns
-   validating data types

These steps ensure the dataset is **consistent and reliable for
analysis**.

------------------------------------------------------------------------

# Exploratory Data Analysis

## Risk Distribution by Accounting Cycle

This analysis identifies **which cycles contain the highest number of
risks**, highlighting potential weaknesses in internal control.

## Risk Distribution by Type

Three main risk categories are analyzed:

  Risk Type       Description
  --------------- --------------------------------------------
  Inherent Risk   Risk due to the nature of operations
  Control Risk    Weakness in internal controls
  Fraud Risk      Potential manipulation or misappropriation

------------------------------------------------------------------------

## Audit Assertions Analysis

Audit assertions represent the **objectives auditors test during an
audit**.

  Assertion                Meaning
  ------------------------ ---------------------------------------------
  Existence                Transactions actually occurred
  Completeness             All transactions are recorded
  Valuation                Amounts are correctly valued
  Rights and Obligations   Assets belong to the company
  Presentation             Financial information is properly disclosed

------------------------------------------------------------------------

# Risk Scoring Model

Risk levels were converted into numeric scores.

  Risk Level   Score
  ------------ -------
  Low          1
  Medium       2
  High         3

This scoring allows the calculation of:

-   average risk score by cycle
-   risk concentration areas
-   prioritization of audit procedures

------------------------------------------------------------------------

# Risk Matrix

A **risk matrix** was built combining:

-   Accounting cycle
-   Audit assertion

This matrix highlights **areas where risks are concentrated**.

Such matrices are widely used in audit firms such as:

-   Deloitte
-   PwC
-   EY
-   KPMG

for **audit planning and risk assessment**.

------------------------------------------------------------------------

# Fraud Risk Analysis

Fraud-related risks were isolated and analyzed separately.

Typical fraud risks include:

-   cash misappropriation
-   fictitious invoices
-   inventory manipulation
-   ghost employees

These risks require **enhanced audit procedures**.

------------------------------------------------------------------------

# Key Insights

Main findings from the analysis:

-   Certain accounting cycles present **higher concentrations of risks**
-   Some assertions are **more frequently affected**
-   Fraud risks are present in **specific operational processes**

These insights help auditors **focus on high-risk areas**.

------------------------------------------------------------------------

# Recommendations

Based on the analysis, several recommendations can be made:

1.  Prioritize cycles with **high average risk scores**
2.  Strengthen **internal control systems**
3.  Implement **additional procedures for fraud risks**
4.  Use **risk matrices** during audit planning

------------------------------------------------------------------------

# Technologies Used

-   Python
-   Pandas
-   Matplotlib
-   Google Colab
-   Excel

------------------------------------------------------------------------

# Project Structure

    audit-risk-analysis/
    │
    ├── data/
    │   └── audit_risk_database.xlsx
    │
    ├── notebooks/
    │   └── audit_risk_analysis.ipynb
    │
    ├── results/
    │   └── audit_analysis_results.xlsx
    │
    └── README.md

------------------------------------------------------------------------

# Conclusion

This project demonstrates how **data analysis techniques can enhance
audit planning** by identifying risk concentrations and supporting
evidence-based decision making.

Such approaches contribute to the development of **data-driven auditing
practices**, increasingly adopted by modern audit firms.
