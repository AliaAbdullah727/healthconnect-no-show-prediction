# 🏥 HealthConnect: Patient No-Show Prediction

## Project Overview

HealthConnect is a healthcare data science project developed as part of the **AnalystLab Africa Experience Lab Internship Programme**.

The project explores how data and machine learning can help identify patient appointments at risk of becoming no-shows.

The main goal is to support better appointment planning and patient engagement.

This project is being developed across multiple stages.

**Week 4:** Problem Understanding & Solution Planning  
**Week 5:** Data Preparation, Feature Engineering & Baseline Modelling  
**Next:** Model Improvement & Validation

---

## Business Problem

Missed appointments can leave appointment slots unused and make clinic planning less efficient.

HealthConnect Clinic wants to explore whether machine learning can help identify appointments that have a higher risk of becoming no-shows.

A future prediction system could support actions such as targeted reminders and administrative follow-up.

The model is intended to support decision-making.

It is not intended to make clinical decisions.

---

## Machine Learning Problem

The project is designed as a **supervised binary classification problem**.

The model predicts:

- `0` → Attended
- `1` → No-Show

The main question is:

> **Can information available before a scheduled appointment be used to predict whether the patient will attend or become a no-show?**

Cancelled appointments were excluded from the initial modelling problem because cancellation represents a different behaviour from failing to attend.

---

# 📅 Project Progress

## Week 4 — Problem Understanding & ML Planning

Week 4 focused on building the foundation of the project.

### Completed

- Defined the business problem
- Defined the machine learning problem
- Reviewed the dataset and Data Dictionary
- Assessed initial data quality
- Defined the target variable
- Identified candidate predictors
- Defined how cancellations would be handled
- Identified potential data leakage
- Considered repeated patients
- Developed an initial modelling strategy

The Week 4 assessment showed that the dataset was suitable for developing an initial no-show prediction model.

---

# Week 5 — Data Preparation & Baseline Modelling

Week 5 moved the project from planning into practical machine learning development.

The main work included:

- Data preparation
- Exploratory data analysis
- Feature engineering
- Feature assessment
- Patient-level train/test splitting
- Categorical encoding
- Numerical scaling
- Logistic Regression baseline development
- Model evaluation
- Cross-track collaboration

---

## 📊 Exploratory Data Analysis

EDA was used to support preprocessing and modelling decisions.

The analysis included:

- Target distribution
- Numerical distributions
- Numerical features vs appointment outcome
- Categorical features vs appointment outcome
- Outlier assessment
- Correlation analysis

### Key Observation

**Booking lead time** showed one of the clearest numerical differences between attended and no-show appointments.

Several other features showed weaker individual relationships with the target.

These variables were not automatically removed because they may still provide useful information when considered together in a machine learning model.

---

## 🧹 Data Preparation

Missing values were handled based on their meaning.

### Reminder Channel

Missing `reminder_channel` values occurred when no reminder was sent.

These values were therefore replaced with:

`No Reminder`

This preserved the meaning of the missing information.

### Numerical Missing Values

Missing values in:

- `distance_to_clinic_km`
- `waiting_time_minutes`

were handled using median imputation.

No duplicate records were identified.

---

## 🎯 Target Preparation

The original appointment outcome contained:

- Attended
- No-Show
- Cancelled

For the baseline model:

```text
Attended  → 0
No-Show   → 1
Cancelled → Excluded
```

The final binary target was approximately balanced.

---

## 🛠️ Feature Engineering

Several features were explored during Week 5.

### Previous No-Show Rate

A historical no-show rate was created using previous appointment behaviour.

This provides more context than the raw number of previous no-shows.

### Previous No-Show Indicator

A binary feature was explored to identify patients with at least one previous no-show.

### Booking Lead Indicator

A binary representation of longer booking lead times was explored.

Correlation analysis showed that it was highly redundant with the original continuous `booking_lead_days` variable.

The continuous variable was therefore preferred.

### Booking Month

Booking month was extracted to allow possible seasonal booking patterns to be represented.

---

## 🔎 Feature Assessment

Engineered features were checked for redundancy before modelling.

Some engineered variables showed strong correlations with their original features.

For example:

- `long_booking_lead` and `booking_lead_days` → approximately **0.86**
- `has_previous_no_show` and `previous_no_shows` → approximately **0.86**

Redundant variables were not all included in the baseline model.

This helped keep the Logistic Regression model simpler and reduced unnecessary multicollinearity.

---

## 🔐 Patient-Level Train/Test Split

The dataset contains repeated appointments from the same patients.

A normal random row-level split could place appointments belonging to the same patient in both training and testing data.

To reduce this risk, a **patient-level GroupShuffleSplit** was used.

Approximately:

- **80%** of the data was used for training
- **20%** was used for testing

A validation check confirmed:

> **0 patients appeared in both the training and testing sets.**

This provides a more realistic test of performance on unseen patients.

---

# 🤖 Baseline Model

## Logistic Regression

Logistic Regression was selected as the baseline classifier.

It was chosen because it is:

- Simple
- Interpretable
- Appropriate for binary classification
- Useful as a benchmark for future models

The goal of Week 5 was to establish a reliable baseline rather than maximize model performance.

---

# 📈 Baseline Results

The Logistic Regression model achieved:

| Metric | Result |
|---|---:|
| Accuracy | **63%** |
| No-Show Precision | **62%** |
| No-Show Recall | **65%** |
| No-Show F1-Score | **64%** |
| ROC-AUC | **0.68** |

### Confusion Matrix

| Actual | Predicted Attended | Predicted No-Show |
|---|---:|---:|
| Attended | 292 | 191 |
| No-Show | 168 | 315 |

The model correctly identified **315 actual no-show appointments**.

It identified approximately **65% of all actual no-shows** in the test set.

---

## Model Interpretation

The baseline results show that the available appointment data contains useful predictive information.

The ROC-AUC of **0.68** indicates moderate ability to distinguish between attended and no-show appointments.

However, the model still misses a meaningful number of no-shows.

The current model should therefore be treated as a **baseline**, not a final solution.

Future models will be compared against these results.

---

# 🤝 Cross-Track Collaboration

During Week 5, I collaborated with a **Data Analytics intern**.

Their analysis highlighted:

- Reminder status
- Previous no-show history
- Booking lead time

as factors related to appointment attendance patterns.

These findings supported several observations from the Data Science analysis.

In particular, booking lead time showed one of the clearest differences during EDA, while previous no-show behaviour was incorporated into feature engineering.

The collaboration provided an additional operational perspective and strengthened the interpretation of the modelling decisions.

---

# ⚠️ Modelling Considerations

Several issues remain important:

- Data leakage
- Repeated patients
- Missing data
- Feature redundancy
- Model interpretability
- Prediction timing
- Fairness and bias
- Generalisation

Patient-level validation should continue to be used during future model development.

Predictions should support patient engagement and administrative planning.

They should not be used to restrict access to healthcare.

---

# 🚀 Next Phase

The next stage will build on the Week 5 Logistic Regression baseline.

Planned work includes:

- Comparing additional classification algorithms
- Improving feature engineering
- Further feature selection
- Model tuning where appropriate
- Model comparison
- Feature importance and interpretation
- Decision-threshold analysis
- Fairness considerations
- More robust model validation

Any future model should demonstrate meaningful improvement over the Week 5 baseline.

---

# 📁 Repository Structure

```text
healthconnect-no-show-prediction/
│
├── data/
│   ├── HealthConnect_Appointment_Data.csv
│   ├── HealthConnect_Data_Dictionary.xlsx
│   └── processed/
│       └── HealthConnect_Processed_Data.csv
│
├── notebooks/
│   └── Week5_HealthConnect_Baseline_Model.ipynb
│
├── reports/
│   ├── HealthConnect_ML_Problem_Definition.pdf
│   ├── HealthConnect_Week4_Project_Summary.pdf
│   ├── HealthConnect_Week5_Baseline_Modelling_Report.pdf
│   └── HealthConnect_Week5_Project_Summary.pdf
│
└── README.md
```

---

# 🧰 Tools & Technologies

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Jupyter Notebook / Google Colab
- Git
- GitHub

---

# 👨‍⚕️ About Me

I am a medical student interested in **Data Science, Artificial Intelligence, and Medical Research**.

I am particularly interested in using data and AI to understand healthcare problems and support better decision-making.

---

# ⚠️ Disclaimer

HealthConnect Clinic and the project dataset are fictional resources created for the AnalystLab Africa Experience Lab.

This project is for educational and portfolio purposes.

The model is not a clinical diagnostic tool and should not be used to make medical decisions.

---

# Acknowledgement

This project is being developed as part of the **AnalystLab Africa Experience Lab Internship Programme**.

#AnalystLabAfrica
---

## Dataset

The HealthConnect Appointment Dataset contains:

- **5,000 appointment records**
- **18 variables**
- **1,696 unique patients**

The dataset includes information about:

- Patient demographics
- Appointment details
- Booking information
- Previous appointments
- Previous no-shows
- Appointment reminders
- Distance to the clinic
- Estimated waiting time
- Appointment outcomes

---

## Initial Data Assessment

The dataset was reviewed for:

- Missing values
- Duplicate records
- Date consistency
- Appointment-history consistency
- Target distribution
- Feature suitability
- Potential data leakage

### Key Findings

- **0 duplicate records**
- No booking dates occurred after appointment dates
- Previous no-shows never exceeded previous appointments
- Booking lead-day values were internally consistent
- Most variables had little or no missing data

Three variables contained missing values:

| Feature | Missing | Percentage |
|---|---:|---:|
| `reminder_channel` | 1,366 | 27.32% |
| `distance_to_clinic_km` | 90 | 1.80% |
| `waiting_time_minutes` | 60 | 1.20% |

The missing values in `reminder_channel` were found to be structural.

When no reminder was sent, there was no reminder channel to record.

---

## Target Definition

The original `appointment_outcome` variable contains:

| Outcome | Records |
|---|---:|
| No-Show | 2,423 |
| Attended | 2,314 |
| Cancelled | 263 |

For the initial machine learning problem:

```text
Attended  → 0
No-Show   → 1
Cancelled → Excluded
```

Cancelled appointments are excluded because cancellation represents a different behaviour from failing to attend without cancelling.

After excluding cancellations, **4,737 appointments** remain for the proposed binary classification problem.

---

## Potential Features

Initial candidate predictors include:

### Patient Information
- Age
- Gender

### Appointment Information
- Appointment type
- Appointment day
- Appointment time

### Booking Information
- Booking lead days

### Patient History
- Previous appointments
- Previous no-shows

### Reminder Information
- Reminder sent
- Reminder channel

### Access & Operational Information
- Distance to clinic
- Estimated waiting time

---

## Features Requiring Special Handling

### `appointment_id`

This is a unique identifier and will not be used as a predictive feature.

### `patient_id`

Patient ID will not be used directly as a predictor.

However, it may be important during train-test splitting because the same patient can appear in multiple appointment records.

### `age_group`

AgeGroup is derived from Age and may introduce redundant information.

Its usefulness will be evaluated before modelling.

### Raw Date Variables

`booking_date` and `appointment_date` will not automatically be used as raw model inputs.

Useful information may instead be extracted from them where appropriate.

### `appointment_outcome`

This variable is the source of the target and must not be included as a predictor.

---

## Data Leakage Considerations

Only information available before the appointment should be used for prediction.

The initial proposed prediction point is **shortly before the scheduled appointment**.

This allows reminder information to potentially be used.

The dataset also contains repeated appointments from the same patients.

A patient-level splitting strategy will therefore be considered to reduce the risk of information from the same patient appearing in both training and testing data.

---

## Proposed Machine Learning Workflow

```text
Appointment Data
      ↓
Data Validation
      ↓
Exclude Cancelled Appointments
      ↓
Create Binary Target
      ↓
Feature Preparation
      ↓
Patient-Aware Train/Test Split
      ↓
Missing Value Handling
      ↓
Categorical Encoding
      ↓
Scaling Where Required
      ↓
Baseline Model
      ↓
Model Comparison
      ↓
Performance Evaluation
      ↓
Model Interpretation
```

---

## Proposed Models

Future development may include:

- Logistic Regression
- Decision Tree
- Random Forest
- Other suitable classification algorithms

Logistic Regression may be used as an interpretable baseline before comparing more complex models.

---

## Evaluation Strategy

Model performance will not be judged using accuracy alone.

Important metrics may include:

- **Recall**
- **Precision**
- **F1-score**
- **ROC-AUC**

Recall will be particularly important because the model should successfully identify appointments that are genuinely at risk of becoming no-shows.

The final decision threshold should also consider how the clinic intends to use the predictions.

---

## Key Considerations

The project will need to consider:

- Missing data
- Repeated patients
- Data leakage
- Prediction timing
- Feature availability
- Cancellations
- Model interpretability
- Fairness and bias
- Generalisation

Predictions should be used to support patient engagement and administrative decisions.

They should not be used to deny care or make clinical decisions.

---

## Week 4 Progress

### HealthConnect Project Kickoff & Problem Understanding

Completed:

- Business problem definition
- Dataset and resource review
- Initial data-quality assessment
- Machine learning problem definition
- Target-variable definition
- Cancellation strategy
- Potential feature identification
- Leakage assessment
- Initial modelling strategy
- Risk and limitation assessment

**Status: Week 4 Complete ✅**

---

## Next Phase

The next stage will focus on:

- Deeper exploratory analysis
- Missing-value treatment
- Feature engineering
- Data preprocessing
- Leakage-safe dataset splitting
- Baseline model development
- Model evaluation and comparison

---

## Repository Structure

```text
healthconnect-no-show-prediction/
│
├── data/
│   ├── HealthConnect_Appointment_Data.csv
│   └── HealthConnect_Data_Dictionary.xlsx
│
├── reports/
│   ├── HealthConnect_ML_Problem_Definition.docx
│   └── HealthConnect_Week4_Project_Summary.docx
│
└── README.md
```

---

## Tools

Current and planned tools include:

- Python
- Pandas
- NumPy
- Matplotlib
- Scikit-learn
- Jupyter Notebook / Google Colab
- Git
- GitHub

---

## About Me

I am Alia Al-Qadri, a medical student with an interest in **data science, artificial intelligence, and medical research**.

I am particularly interested in using data and AI to understand healthcare problems and support better decision-making.

---

## Disclaimer

HealthConnect Clinic and its dataset are fictional resources created for the AnalystLab Africa Experience Lab.

This project is for educational and portfolio purposes.

The proposed machine learning system is not a clinical diagnostic tool.

---

## Acknowledgement

This project is being developed as part of the **AnalystLab Africa Experience Lab Internship Programme**.

#AnalystLabAfrica
