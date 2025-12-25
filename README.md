# Customer Churn Prediction Project

## Project Overview
This project focuses on predicting customer churn using the **Telco Customer Churn** dataset.

Customer churn means that a client stops using the company’s services.  
The goal of this project is not only to build machine learning models, but also to **optimize decision-making** by selecting an appropriate classification threshold.

The project follows a complete data science pipeline:
- Data preprocessing
- Feature engineering
- Model training
- Threshold optimization
- Model evaluation
- Model comparison

The implementation is done in **MATLAB**.

---

## Dataset
**Source:** Telco Customer Churn dataset  

Each row represents one customer.  
The target variable is:

- `Churn`
  - `Yes` → customer left
  - `No` → customer stayed

The dataset includes:
- Customer demographics
- Service usage information
- Contract and billing details

---

## Data Preprocessing

### Removing Unnecessary Columns
- The column `customerID` was removed since it does not provide predictive information.

---

### Handling Missing Values
- The feature `TotalCharges` contained 11 missing values.
- These rows correspond to customers with `tenure = 0`.
- Since this represents less than 0.2% of the dataset, the rows were removed.

---

### Target Encoding
- `Churn` was converted to a binary variable:
  - Yes → 1
  - No → 0

---

### Categorical Feature Encoding
- One-hot encoding was applied to categorical features.

Model-specific handling:
- **Logistic Regression:** one reference category removed to avoid multicollinearity.
- **Decision Tree / Random Forest:** all categories retained.

---

### Feature Scaling
- Numerical features were standardized **only for Logistic Regression**.
- Tree-based models were trained on raw numerical values.

---

### Train–Test Split
- 70% training set
- 30% test set
- Fixed random seed used for reproducibility

---

## Models Used

The following models were trained and evaluated:
1. Logistic Regression
2. Decision Tree
3. Random Forest

---

## Evaluation Strategy

In addition to standard evaluation at threshold = 0.5, this project includes **threshold optimization**.

Why threshold optimization is important:
- Churn datasets are usually **imbalanced**
- Missing a churner (False Negative) is more costly than a false alarm
- Default threshold (0.5) is often suboptimal

Thresholds were optimized to **maximize F1-score**, balancing precision and recall.

---

## Logistic Regression (Final Model)

### Model Description
Logistic Regression predicts the probability that a customer will churn.

Regularization was applied using:
- **Lambda (MATLAB):** 0.4706  
- **C (scikit-learn equivalent):** 2.1251  

---

### Threshold Optimization
- Tested multiple thresholds
- Best threshold selected based on F1-score

**Best threshold:** 0.30

---

### Final Results
- **ROC-AUC:** 0.825
- **Accuracy:** 76.1%
- **Precision:** 53.6%
- **Recall:** **73.9%**
- **F1-score:** **0.621**

---

### Interpretation
- Lowering the threshold significantly increased recall
- The model now detects most churners
- Some increase in false positives is accepted
- This setup is more suitable for churn prevention tasks

---

## Decision Tree (Final Model)

### Model Description
Decision Tree uses if–else rules to classify customers.

### Optimized Hyperparameters
- MinLeafSize: 47
- MaxNumSplits: 500
- CCP Alpha: 0.000857

---

### Threshold Optimization
**Best threshold:** 0.25

---

### Final Results
- **ROC-AUC:** 0.810
- **Accuracy:** 75.8%
- **Precision:** 53.2%
- **Recall:** **73.2%**
- **F1-score:** **0.617**

---

### Interpretation
- Threshold tuning significantly improved recall
- Performance is close to Logistic Regression
- Tree remains less stable but interpretable

---

## Random Forest (Baseline Threshold = 0.5)

### Model Description
Random Forest is an ensemble of decision trees.

### Results
- **ROC-AUC:** 0.838
- **Accuracy:** 79.4%
- **Precision:** 63.0%
- **Recall:** 54.5%
- **F1-score:** 0.584

---

### Interpretation
- Best ranking performance (AUC)
- Conservative at threshold 0.5
- Recall can be further improved by threshold tuning

---

## Model Comparison (After Threshold Optimization)

| Model | Threshold | AUC | Accuracy | Precision | Recall | F1 |
|-----|---------|----|---------|----------|--------|----|
| Logistic Regression | 0.30 | 0.825 | 76.1% | 53.6% | **73.9%** | **0.621** |
| Decision Tree | 0.25 | 0.810 | 75.8% | 53.2% | 73.2% | 0.617 |
| Random Forest | 0.50 | **0.838** | **79.4%** | **63.0%** | 54.5% | 0.584 |

---

## Final Conclusion

Key insights from the project:

- **Logistic Regression with threshold optimization** provides the best balance between recall and precision.
- **Decision Tree** becomes competitive after threshold tuning but remains less stable.
- **Random Forest** shows the strongest ranking ability (highest AUC) but requires threshold adjustment to improve recall.

### Recommended Model
- For **churn prevention (catch as many churners as possible)**:
  → **Logistic Regression with threshold = 0.30**
- For **ranking customers by churn risk**:
  → **Random Forest**

---

## Business Insight
Lowering the classification threshold allows the company to proactively identify at-risk customers and apply retention strategies, accepting a controlled number of false alarms in exchange for reducing lost customers.

---

## Future Improvements
- Threshold optimization for Random Forest
- Cost-sensitive learning
- Gradient boosting models (CatBoost, XGBoost)
- SHAP-based feature importance

---

