# 🏥 HealthConnect: Patient No-Show Prediction

## Project Overview

HealthConnect is a healthcare data science project developed as part of the **AnalystLab Africa Experience Lab Internship Programme**.

The project explores how data and machine learning can help HealthConnect Clinic understand and reduce missed patient appointments.

The main Data Science objective is to determine whether information available before an appointment can be used to predict the risk of a patient becoming a **no-show**.

This repository will develop across multiple stages, starting with problem understanding and eventually moving into machine learning development, testing, and refinement.

---

## Business Problem

Missed appointments can leave appointment slots unused and make clinic planning less efficient.

HealthConnect Clinic wants to explore whether machine learning can help identify appointments with a higher risk of becoming no-shows.

A future prediction system could support actions such as targeted reminders and administrative follow-up.

The model is intended to support decision-making. It is not intended to make medical decisions.

---

## Machine Learning Problem

The proposed task is a **supervised binary classification problem**.

The model will aim to predict:

- `0` → Attended
- `1` → No-Show

The main question is:

> **Can information available before a scheduled appointment be used to predict whether the patient will attend or become a no-show?**

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

I am a medical student with an interest in **data science, artificial intelligence, and medical research**.

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
