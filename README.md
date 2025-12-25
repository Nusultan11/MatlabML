# Customer Churn Prediction (MATLAB)

## Project Overview
This project focuses on predicting **customer churn** using the Telco Customer Churn dataset.

Customer churn means that a customer stops using the company’s services.  
The main goal of this project is to **identify customers who are likely to leave**, so that the business can take proactive retention actions.

The project follows a **complete machine learning pipeline**:
- Data preprocessing
- Feature engineering
- Model training
- Hyperparameter tuning
- Threshold optimization
- Model evaluation and comparison

All experiments are implemented in **MATLAB**.

---

## Dataset Description
**Dataset:** Telco Customer Churn  

Each row represents one customer.  
The target variable is:

- `Churn`
  - `Yes` → customer left
  - `No` → customer stayed

The dataset contains:
- Customer demographics
- Service usage information
- Contract and billing details

Approximate dataset size: **~7000 customers**

---

## Data Preprocessing

### Removing Non-Informative Columns
- The column `customerID` was removed because it is only an identifier and does not help predict churn.

---

### Handling Missing Values
- The feature `TotalCharges` contained **11 missing values**.
- These rows correspond to customers with `tenure = 0`.
- Since this is less than **0.2% of the dataset**, the rows were removed.

---

### Target Encoding
- The target variable `Churn` was converted to binary format:
  - Yes → 1
  - No → 0

---

### Categorical Feature Encoding
- One-hot encoding was applied to all categorical features.

Model-specific handling:
- **Logistic Regression:** one reference category removed to avoid multicollinearity.
- **Decision Tree / Random Forest:** all categories kept.

---

### Feature Scaling
- Numerical features (`tenure`, `MonthlyCharges`, `TotalCharges`) were standardized **only for Logistic Regression**.
- Tree-based models were trained on raw numerical values.

---

### Train–Test Split
- 70% training data
- 30% test data
- Fixed random seed for reproducibility

---

## Models Used

The following models were trained and evaluated:

1. Logistic Regression  
2. Decision Tree  
3. Random Forest  

All models were evaluated on the same test set.

---

## Evaluation Metrics

The following metrics were used:
- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC

Because churn data is imbalanced and **missing a churner is costly**, special attention was paid to **Recall** and **F1-score**.

---

## Threshold Optimization

### Why Threshold Optimization?
The default classification threshold (0.5) is often **not optimal** for churn problems.

- False Negatives (missed churners) are more expensive than False Positives
- Lowering the threshold allows the model to catch more churners

Thresholds were optimized to **maximize F1-score**, balancing precision and recall.

---

## Logistic Regression (Final)

**Regularization:**
- Lambda (MATLAB): 0.4706  
- C (scikit-learn equivalent): 2.1251  

**Optimal threshold:** 0.30  

**Final Results:**
- ROC-AUC: 0.825
- Accuracy: 76.1%
- Precision: 53.6%
- Recall: **73.9%**
- F1-score: **0.621**

**Interpretation:**  
Lowering the threshold significantly improved recall, making Logistic Regression a strong baseline model for churn prevention.

---

## Decision Tree (Final)

**Optimized hyperparameters:**
- MinLeafSize: 47
- MaxNumSplits: 500
- CCP Alpha: 0.000857

**Optimal threshold:** 0.25  

**Final Results:**
- ROC-AUC: 0.810
- Accuracy: 75.8%
- Precision: 53.2%
- Recall: **73.2%**
- F1-score: **0.617**

**Interpretation:**  
Threshold tuning improved recall, but the model remains less stable than ensemble methods.

---

## Random Forest (Final – Best Model)

**Optimized hyperparameters:**
- Number of trees: **624**
- Max depth: **5**
- Min leaf size: **47**

**Optimal threshold:** **0.35**

**Final Results:**
- ROC-AUC: **0.843**
- Accuracy: 78.2%
- Precision: 57.1%
- Recall: **72.0%**
- F1-score: **0.637**

**Interpretation:**  
After hyperparameter tuning and threshold optimization, Random Forest achieved the **best overall performance**.

---

## Final Model Comparison

| Model | Threshold | AUC | Accuracy | Precision | Recall | F1 |
|-----|---------|----|---------|----------|--------|----|
| Logistic Regression | 0.30 | 0.825 | 76.1% | 53.6% | **73.9%** | 0.621 |
| Decision Tree | 0.25 | 0.810 | 75.8% | 53.2% | 73.2% | 0.617 |
| **Random Forest** | **0.35** | **0.843** | 78.2% | 57.1% | 72.0% | **0.637** |

---

## Final Conclusion

After optimizing both **model hyperparameters** and **classification thresholds**:

- **Random Forest** achieved the best balance between precision and recall
- Logistic Regression remains a strong and interpretable baseline
- Decision Tree is mainly useful for interpretability

### Recommended Model
✅ **Random Forest with threshold = 0.35**

This model is the most suitable for churn prevention tasks.

---

## Business Impact

By optimizing the classification threshold, the company can:
- Identify more at-risk customers
- Reduce customer loss
- Apply proactive retention strategies

Accepting a controlled number of false alarms is justified by the reduction in missed churners.

---

## Future Improvements
- Cost-sensitive learning
- Gradient boosting models (CatBoost, XGBoost)
- SHAP-based feature importance
- Model deployment and monitoring

---
