# Blood Biomarker Classification Model

## 1. Problem Definition

We built a machine learning classification model that predicts patient condition based on blood biomarkers.

### Target Classes
- Bacterial Infection  
- Normal  
- Possible Immunodeficiency / Severe Condition  

---

## 2. Medical Background

### What are we modeling?

We use white blood cell (WBC) biomarkers as key indicators of immune system activity.

---

### Main Biomarkers Used

#### 1. WBC (White Blood Cell Count)
- Unit: ×10³ cells / µL  
- Normal range: 4 – 11  

**Interpretation:**
- High WBC → infection / inflammation  
- Very high WBC → strong immune response (often bacterial)  
- Low WBC → immune suppression / bone marrow issues  

---

#### 2. Neutrophils (%)
- Normal: 40 – 70%  

**Interpretation:**
- High → bacterial infection  
- First responders to bacterial infections  

---

#### 3. Lymphocytes (%)
- Normal: 20 – 40%  

**Interpretation:**
- High → viral infection (generally)  
- Low → often bacterial infection  

---

#### 4. NLR (Neutrophil-to-Lymphocyte Ratio)

\[
NLR = \frac{Neutrophils}{Lymphocytes}
\]

**Interpretation:**
- High NLR → strong indicator of bacterial infection  
- Low NLR → normal or viral pattern  

---

### Key Medical Logic

| Pattern | Interpretation |
|--------|---------------|
| High WBC + High Neutrophils + Low Lymphocytes | Bacterial Infection |
| Normal values | Normal |
| Very Low WBC | Immunodeficiency / Severe Condition |

---

## 3. Feature Engineering

### Features Used
- WBC_log  
- Neutrophils  
- Lymphocytes  
- NLR  

---

### Transformations

#### 1. Log Transformation (WBC)
WBC_log = log(WBC)

**Why:**
- WBC distribution is highly skewed  
- Handles extreme outliers (up to ~1500)  
- Improves model stability  

---

#### 2. NLR Feature
NLR = Neutrophils / Lymphocytes  

**Why:**
- Captures interaction between immune markers  
- Clinically meaningful feature  
- Strong predictive power  

---

#### 3. Scaling
Applied StandardScaler

**Why:**
- Features have different ranges  
- Ensures fair contribution to model  

---

## 4. Model Architecture

### Model Used
Random Forest Classifier

### Why Random Forest?
- Handles non-linear relationships  
- Works well with small to medium datasets  
- Robust to noise  
- Provides feature importance  

---

### How it works (simplified)
- Multiple decision trees are trained  
- Each tree predicts independently  
- Final output = majority vote  

---

## 5. Model Performance

### Accuracy
- Train: 1.00  
- Test: 0.944  

---

### Interpretation

- Train = 100% → possible overfitting  
- Test = 94.4% → strong generalization  

---

### Classification Report Summary

| Class | Performance |
|------|------------|
| Bacterial Infection | High recall |
| Normal | High accuracy |
| Severe | Weak / underrepresented |

---

## 6. Critical Issue Found

### Class Imbalance Problem
- Dataset has 3 classes  
- Test set had only 2 classes  

### Implications
- Model did not fully learn all classes equally  
- Severe class is underrepresented  

---

## 7. Model Behavior Analysis

### Feature Importance

- WBC_log: 0.65  
- Neutrophils: 0.22  
- NLR: 0.07  
- Lymphocytes: 0.04  

---

### Insight

- WBC_log is the dominant feature  
- Neutrophils provide secondary confirmation  
- NLR adds additional signal  
- Lymphocytes contribute least  

This aligns with real clinical reasoning.

---

## 8. Model Testing

### Issue
Manual predictions were incorrect.

### Root Cause
- Missing log transformation  
- Missing scaling  
- Incorrect feature order  

---

### Fix
Pipeline corrected:

- Apply log transformation  
- Create NLR  
- Apply scaler  
- Predict  

---

## 9. Deployment (Web App)

### Stack
- Flask backend  
- Joblib model loading  

### User Inputs
- WBC  
- Neutrophils  
- Lymphocytes  

---

### Important Detail
The web app applies the same preprocessing:

- log(WBC)  
- NLR calculation  
- scaling  

Ensures consistency with training.

---

## 10. Limitations

### 1. Class Imbalance
Severe cases are underrepresented.

### 2. Limited Features
Only 3–4 biomarkers used.

Missing important clinical indicators:
- CRP  
- Platelets  
- Hemoglobin  
- Symptoms  

### 3. No Localization
Model predicts condition only, not infection location.

### 4. Dataset Size
Small dataset increases overfitting risk.

---

## 11. Possible Improvements

### Technical Improvements
- Cross-validation  
- SMOTE for balancing  
- XGBoost or LightGBM  

---

### Medical Improvements
- Add CRP  
- Add Platelets  
- Add Hemoglobin  
- Include clinical symptoms  

---

### Product Improvements
- Show prediction probabilities  
- Add explanations like:
  "High WBC and neutrophils suggest bacterial infection"

---

## Final Summary

We built a machine learning model that:

- Uses WBC biomarkers  
- Applies feature engineering (log + NLR)  
- Uses Random Forest classifier  
- Achieves ~94% accuracy  
- Mimics clinical reasoning patterns  

It serves as a strong prototype for a clinical decision support system.
