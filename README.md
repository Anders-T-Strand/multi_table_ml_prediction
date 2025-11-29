# Multi-Table Machine Learning Prediction

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![Jupyter Notebook](https://img.shields.io/badge/jupyter-%23F37626.svg?style=flat&logo=jupyter&logoColor=white)](https://jupyter.org/)

A comprehensive data science project demonstrating end-to-end machine learning workflow including data integration from multiple sources, feature engineering, and model comparison using ensemble methods.

## 📋 Table of Contents
- [Overview](#overview)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Models & Results](#models--results)
- [Installation](#installation)
- [Project Structure](#project-structure)
- [Key Findings](#key-findings)
- [Technologies Used](#technologies-used)
- [Future Improvements](#future-improvements)
- [License](#license)

## 🎯 Overview

This project tackles a binary classification problem using data from three separate information systems. The main objectives are:

- **Data Integration**: Merge datasets from different sources using engineered join keys
- **Feature Engineering**: Create meaningful features while avoiding data leakage
- **Model Comparison**: Evaluate three tree-based ensemble methods
- **Production-Ready Pipeline**: Build reproducible preprocessing and modeling pipelines

## 📊 Dataset

The project works with three complementary datasets:

### 1. Demographics Dataset (1,010 rows × 13 columns)
- Personal information (names, contact details, location)
- Date of birth and temporal features
- Semi-constant identifiers

### 2. Behavioral Metrics Dataset (1,010 rows × 16 columns)
- Informative features correlated with target
- Multicollinearity features (intentionally correlated)
- Outlier-injected features
- Scaled features demonstrating preprocessing effects

### 3. Survey Scores Dataset (1,030 rows × 10 columns)
- Quantile-based categorical features
- Random choice features
- Contact information for linking

**Target Variable**: Binary classification (`class`: 0 or 1) - approximately balanced (50.4% / 49.6%)

## 🔬 Methodology

### 1. Data Merging Strategy

**Join Keys Engineered**:
- `login_id`: Extracted username from email addresses (demographics ↔ behavioral)
- `contact_phone`: Last 7 digits of phone numbers (demographics ↔ survey)

**Join Approach**:
```
behavioral_metrics (base) 
    ⟶ LEFT JOIN demographics ON login_id
    ⟶ LEFT JOIN survey_scores ON contact_phone
```

This ensures all behavioral records (containing the target) are retained while enriching with available demographic and survey data.

### 2. Data Cleaning

**Removed**:
- ✓ PII (names, emails, addresses, phone numbers, zip codes)
- ✓ Exact duplicate rows (25 duplicates removed from survey data)
- ✓ Quasi-constant features (>99% same value)
- ✓ Leakage features (target-derived or highly correlated: `target`, `corr_feature_class`, quantile bins)

**Final Dataset**: 1,009 rows × 13 features (after cleaning)

### 3. Preprocessing Pipeline

**For Numeric Features**:
- Median imputation for missing values
- No scaling (tree-based models are scale-invariant)

**For Categorical Features**:
- Most frequent value imputation
- One-hot encoding with unknown category handling

### 4. Model Training & Evaluation

**Models Compared**:
1. **Random Forest** - Robust to outliers, handles non-linear relationships
2. **XGBoost** - Gradient boosting with regularization
3. **CatBoost** - Optimized for categorical features with ordered boosting

**Hyperparameter Tuning**:
- GridSearchCV with 5-fold stratified cross-validation
- Optimized for F1 score (balances precision and recall)
- 80/20 train-test split (stratified)

## 🏆 Models & Results

| Model | Test Accuracy | Test F1 Score | Best Hyperparameters |
|-------|--------------|---------------|---------------------|
| **Random Forest** | 83.17% | 83.33% | `n_estimators=100`, `max_depth=None`, `min_samples_split=2` |
| **XGBoost** ⭐ | **85.64%** | **86.12%** | `learning_rate=0.1`, `max_depth=6`, `n_estimators=400`, `subsample=0.8` |
| **CatBoost** | 85.64% | 85.71% | `depth=6`, `iterations=200`, `learning_rate=0.1` |

### Best Model: XGBoost

**Test Performance**:
- **Accuracy**: 85.64%
- **F1 Score**: 86.12%
- **Precision (Class 0)**: 89%
- **Recall (Class 0)**: 81%
- **Precision (Class 1)**: 83%
- **Recall (Class 1)**: 90%

**Confusion Matrix**:
```
[[83  19]   ← Class 0: 83 correct, 19 misclassified
 [10  90]]  ← Class 1: 90 correct, 10 misclassified
```

## 🚀 Installation

### Prerequisites
- Python 3.8+
- pip or conda

### Setup

```bash
# Clone the repository
git clone https://github.com/yourusername/multi-table-ml-prediction.git
cd multi-table-ml-prediction

# Create virtual environment (recommended)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### Required Packages

See [requirements.txt](requirements.txt) for the complete list. Main dependencies include:

- **Data Processing**: pandas, numpy
- **Machine Learning**: scikit-learn, xgboost, catboost
- **Visualization**: matplotlib, seaborn
- **Development**: jupyter, ipykernel

## 📁 Project Structure

```
multi-table-ml-prediction/
│
├── data/
│   ├── demographics.csv
│   ├── behavioral_metrics.csv
│   └── survey_scores.csv
│
├── multi_table_ml_prediction.ipynb   # Main analysis notebook
├── README.md                           # Project documentation
├── requirements.txt                    # Python dependencies
├── .gitignore
└── LICENSE
```

## 🔍 Key Findings

1. **Feature Importance**: Informative numeric features (`informative_1`, `informative_2`) and multicollinearity features showed the strongest separation between classes.

2. **Missing Data Handling**: Median imputation for numeric features and most-frequent imputation for categoricals proved effective without significant information loss.

3. **Model Performance**: 
   - XGBoost and CatBoost achieved nearly identical accuracy (~85.6%)
   - XGBoost had slightly better F1 score due to better balance between precision and recall
   - Random Forest performed well but ~2.5% lower than gradient boosting methods

4. **Leakage Prevention**: Removing target-derived features was crucial—initial runs with leakage features showed unrealistic 95%+ accuracy.

5. **Data Integration**: Engineered join keys successfully linked 95%+ of records across datasets, demonstrating effective data integration from disparate sources.

## 🛠️ Technologies Used

- **Python 3.11+**
- **Data Processing**: Pandas, NumPy
- **Machine Learning**: Scikit-learn, XGBoost, CatBoost
- **Visualization**: Matplotlib
- **Development**: Jupyter Notebook

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

**What this means:**
- ✅ Free to use, modify, and distribute
- ✅ Can be used commercially
- ✅ Must include original license and copyright
- ✅ No warranty provided

## 👤 Author

**Anders Strand**
- GitHub: [@Anders-T-Strand](https://github.com/Anders-T-Strand)
- LinkedIn: [Anders Strand](https://www.linkedin.com/in/anders-strand/)

## 🙏 Acknowledgments

- Dataset generated with synthetic data for educational purposes
- Inspired by real-world multi-source data integration challenges
- Built as part of Principles and Techniques for Data Science at University of North Texas

---
