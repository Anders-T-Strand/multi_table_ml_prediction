# multi_table_ml_prediction

This project demonstrates a complete end-to-end workflow for integrating siloed datasets, performing data cleaning, engineering, and comparing three supervised learning models (Random Forest, XGBoost, CatBoost).

## 🚀 Overview
- Multi-source dataset merging  
- PII handling  
- Missing data imputation  
- Outlier detection  
- Categorical encoding  
- Leakage prevention  
- Full ML pipeline with GridSearchCV  
- Evaluation using Accuracy, F1 score, and confusion matrices  

## 📁 Repository Structure
```
multi-table-ml-prediction/
│
├── README.md
├── data/
│   ├── demographics.csv
│   ├── behavioral_metrics.csv
│   └── survey_scores.csv
│
├── notebook/
│   └── analysis_and_modeling.ipynb
│
└── src/
    └── data_prep_utils.py
```

## 🧠 Key Skills Demonstrated
- Multi-table merging  
- Data cleaning and hygiene  
- Handling missingness  
- ML pipeline design  
- Hyperparameter tuning  
- Tree-based model comparison  
- Avoiding data leakage  

## 📊 Modeling Workflow
1. Preprocessing  
2. Train/test split (stratified)  
3. GridSearchCV with 5-fold cross-validation  
4. Model training  
5. Evaluation & comparison  

## 🏆 Results
Typical results show strong performance across all models, with XGBoost or CatBoost often achieving the best F1 score.

## 📘 Notebook
The full cleaned notebook is located at:  
`/notebook/analysis_and_modeling.ipynb`

---

If you have questions about the workflow or want to extend the project, feel free to reach out.
