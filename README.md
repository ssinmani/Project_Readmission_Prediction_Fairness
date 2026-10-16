# Hospital Readmission Prediction

## Goal
Predict whether a diabetic patient will be readmitted within 30 days using three models, then check whether the best model works equally well across age, sex and race groups.

## Data
Cleaned dataset from my Project 1 ([link to Project 1 repo]): 68,167 patients (one visit each) from 130 US hospitals, 1999–2008. 9.04% were readmitted within 30 days.

## Method
- Removed `readmitted` to avoid data leakage
- New features: `total_visits` (all past visits) and `meds_changed` (number of medicine dose changes)
- Age groups converted to numbers; top 10 values kept for diagnosis and specialty columns, the rest grouped as "Other"; one-hot encoding for all text columns
- 80/20 stratified split, 0 patients in both sets
- Models: logistic regression, random forest, gradient boosting (all with balanced class weights)

## Results
| Model | ROC-AUC | PR-AUC | Recall |
|---|---|---|---|
| Logistic regression | 0.650 | 0.170 | 0.538 |
| Random forest | 0.643 | 0.170 | 0.108 |
| Gradient boosting | 0.652 | 0.177 | 0.514 |

PR-AUC is the most informative metric here because only 9% of patients are readmitted (baseline 0.09). The best model is about twice as good as guessing at finding readmitted patients.

## Most important features
Discharge to a nursing or rehab facility, past inpatient stays, length of stay, number of diagnoses and age. These describe frailer, sicker patients and match my Project 1 findings.

## Error analysis
Missed patients had almost no past hospital stays and short stays, so they looked low-risk in the data. The model cannot see why they came back.

## Fairness
- Gender: similar performance for women and men
- Age: older patients are caught more often than younger ones
- Race: lower recall for African American patients (0.42) than Caucasian patients (0.54)
- Very small groups (Asian, Hispanic, age 90+) give unreliable numbers

## A mistake I caught
My first results showed ROC-AUC = 1.0, which was unrealistic. The original `readmitted` column had been encoded into the features, leaking the answer. After removing it, scores became realistic (~0.65).

## Limitations
- Data is from 1999–2008 and from one dataset only
- Readmissions to other hospitals may be missing
- Default 0.5 threshold; random forest recall could improve with a different threshold
- Small groups make some fairness results unreliable
- Moderate performance (ROC-AUC ~0.65): important reasons for readmission are not in the data

## Discl
This is a learning project. The model is not validated for clinical use and must not be used for real medical decisions.aimer

## Tools
Python, pandas, scikit-learn, Kaggle

