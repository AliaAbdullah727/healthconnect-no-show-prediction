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

I am Alia Al-Qadri, a medical student interested in **Data Science, Artificial Intelligence, and Medical Research**.

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

## Week 6 — Model Improvement, Error Analysis & Validation

Week 6 focused on improving and validating the Week 5 patient no-show prediction baseline.

### Work Completed

- Performed targeted analysis using findings from cross-track Data Analytics collaboration.
- Investigated false positives and false negatives from the Week 5 Logistic Regression baseline.
- Refined features based on previous no-show history, booking lead time, reminders and appointment characteristics.
- Tested interaction features identified through deeper analysis.
- Compared Logistic Regression, Random Forest and Gradient Boosting.
- Performed feature-importance and decision-threshold analysis.
- Selected Gradient Boosting as the Week 6 candidate model.
- Prepared model requirements and preprocessing decisions for ML Engineering integration.

### Model Comparison

| Model | Accuracy | No-Show Recall | No-Show F1 | ROC-AUC |
|---|---:|---:|---:|---:|
| Baseline Logistic Regression | 0.63 | 0.65 | 0.64 | 0.68 |
| Refined Logistic Regression | 0.64 | 0.66 | 0.64 | 0.68 |
| Random Forest | 0.61 | 0.62 | 0.61 | 0.64 |
| Gradient Boosting | 0.65 | 0.65 | 0.65 | 0.68 |

### Key Findings

Previous no-show history showed a clear relationship with future no-show behaviour.

Booking lead time remained an important predictive signal and was also the highest-ranked feature in the Random Forest importance analysis.

Reminder status showed an association with attendance, and this relationship varied across appointment types.

Gradient Boosting provided the strongest overall classification performance, although improvement over Logistic Regression was modest.

### Threshold Analysis

The default 0.50 threshold provided balanced classification performance.

A 0.45 threshold increased No-Show recall to approximately 74%, showing that the decision threshold could be adjusted if HealthConnect prioritizes identifying more potential no-shows.

### Cross-Track Integration

Data Analytics findings were used to guide feature investigation and modelling decisions.

Data Science outputs, preprocessing decisions and model requirements were also shared with the ML Engineering track to support downstream pipeline integration.

### Week 7

The next stage will focus on model testing, stability, threshold validation, reproducibility and integration testing.

## Week 7 — Model Testing, Refinement & Validation

Week 7 focused on systematically testing the Gradient Boosting model selected in Week 6.

The goal was to check whether the model generalised to unseen patients, identify weaknesses, refine the model where necessary, and validate the results.

### Testing Completed

- Performed 5-fold patient-grouped cross-validation.
- Compared cross-validation and held-out test performance.
- Tested for overfitting using training vs test performance.
- Tuned the Gradient Boosting model based on the testing results.
- Re-tested the refined model.
- Evaluated performance across important patient and appointment segments.
- Investigated false positives and false negatives.
- Tested an alternative prediction threshold.
- Validated an important model finding with the Data Analytics track.

### Model Refinement

The Week 6 Gradient Boosting model showed some evidence of overfitting.

- Training ROC-AUC: **0.77**
- Test ROC-AUC: **0.68**

After tuning the model, held-out performance improved.

| Metric | Week 6 Model | Tuned Week 7 Model |
|---|---:|---:|
| Accuracy | 0.65 | 0.66 |
| No-Show Precision | 0.64 | 0.66 |
| No-Show Recall | 0.65 | 0.67 |
| No-Show F1 | 0.65 | 0.67 |
| ROC-AUC | 0.68 | **0.72** |

The tuned model also showed less evidence of overfitting.

### Segment Testing

Model performance was not consistent across all groups.

**Previous no-show history**
- Previous no-show: **78% recall**
- No previous no-show: **58% recall**

**Booking lead time**
- >30 days: **94.3% recall**
- ≤30 days: **23.0% recall**

**Appointment type**
- Diagnostic Test: **80% recall**
- Specialist Consultation: **53% recall**

The largest weakness was the model's ability to identify no-shows among appointments booked within 30 days.

### Error Analysis

False-negative no-shows had an average booking lead time of approximately **17 days**.

Correctly identified no-shows had an average booking lead time of approximately **44 days**.

This showed that the model performs better when stronger historical risk patterns are present.

### Threshold Testing

A lower classification threshold of **0.45** was tested.

For appointments booked within 30 days, No-Show recall improved:

**23.0% → 37.7%**

However, accuracy decreased and the underlying weakness remained.

The **0.50 threshold** was therefore retained as the main model threshold, while 0.45 remains a possible higher-recall alternative.

### Cross-Track Validation

The booking lead-time finding was independently checked with the Data Analytics track.

Their analysis found:

- ≤30 days: **36.84% actual no-show rate**
- >30 days: **60.31% actual no-show rate**

This supported booking lead time as an important risk factor.

The model showed a much larger difference in recall between these groups, suggesting that it may rely heavily on booking lead time when identifying high-risk appointments.

Earlier Data Analytics risk segmentation also helped guide the feature and segment testing performed during model validation.

### Final Model

The tuned Gradient Boosting model achieved:

- **66% accuracy**
- **66% No-Show precision**
- **67% No-Show recall**
- **67% No-Show F1-score**
- **0.72 ROC-AUC**

The model is best considered a **risk-screening and decision-support tool**, rather than a standalone decision system.

### Remaining Limitations

The model still has difficulty identifying:

- Short-lead no-shows.
- Patients without previous no-show history.
- Some Specialist Consultation no-shows.

These limitations should be considered during final HealthConnect integration.

### Week 8

Week 8 will focus on final integration, presentation, documentation, and communicating the validated HealthConnect solution.
