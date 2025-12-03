# Advanced Data Mining for Data-Driven Insights and Predictive Modeling 
## Final Project - Superstore Sales Analysis

### 📌 Project Overview

This project applies the full data mining pipeline to the **Superstore Sales** dataset from Kaggle.  
Across four deliverables, the analysis covers:

- Data collection, cleaning, and exploratory data analysis (EDA)
- Regression modeling to predict profit
- Classification of high-profit versus low-profit orders
- Clustering of transactions
- Association rule mining to identify product affinity patterns
- Final business insights, ethical considerations, and recommendations

The goal is to demonstrate how data mining techniques can support **data-driven decision making** in a retail environment.

---

## 📘 Dataset Summary and Justification

**Dataset:** Superstore Sales Dataset (Kaggle)  
**Rows:** ~9,994  
**Attributes:** 21

Key fields include:

- Customer and geographic attributes  
- Product details (`Category`, `Sub-Category`)  
- Transaction features (`Sales`, `Quantity`, `Discount`, `Profit`)  
- Order metadata (`Order Date`, `Ship Date`, `Region`, `Segment`)

This dataset was chosen because:

- It meets the project requirements (more than 8 attributes and more than 500 records).  
- It includes both numeric and categorical variables that support **regression, classification, clustering, and association rule mining**.  
- It represents a realistic retail scenario where insights about profitability, discounting, and product relationships are useful for business decisions.

**Source:**  
Vivek. (2019). *Superstore Sales Dataset*. Kaggle.  
File used: `Sample - Superstore.csv`

---

## 🧩 Summary of Project Steps

### Deliverable 1 - Data Collection, Cleaning, and Exploration

- Loaded the dataset and inspected data types, structure, and summary statistics.
- Removed duplicate rows and parsed date fields.
- Treated `Postal Code` as a string to preserve leading zeros.
- Implemented logic for imputing numeric missing values with the median and categorical values with the mode.
- Detected outliers in `Sales`, `Profit`, `Discount`, and `Quantity` using the IQR method and clipped them to reduce noise.
- Performed EDA with histograms, boxplots, a correlation heatmap, and grouped summaries of `Profit` by Category, Sub-Category, Region, and Segment.

**Key findings:**

- Higher discounts are strongly associated with lower profit.  
- Technology products tend to be more profitable than some furniture categories.  
- Tables and Bookcases often appear as low-margin or negative-profit items.  

A cleaned version of the dataset was saved as `data/cleaned_superstore.csv` for use in later deliverables.

---

### Deliverable 2 - Regression Modeling and Performance Evaluation

**Objective:** Predict `Profit` using numeric and categorical features.

**Models:**

- Linear Regression  
- Ridge Regression (L2 regularization)  
- Lasso Regression (L1 regularization)

**Features used:**

- Numeric: `Sales`, `Discount`, `Quantity`  
- Categorical: `Category`, `Sub-Category`, `Region`, `Segment`  

Preprocessing was handled with a `ColumnTransformer` and pipeline:

- Standardization for numeric features  
- One-hot encoding for categorical features  

**Evaluation metrics:**

- R²  
- Mean Absolute Error (MAE)  
- Mean Squared Error (MSE)  
- Root Mean Squared Error (RMSE)  
- 5-fold cross-validation for R² and RMSE

All three models produced similar performance (test R² around 0.57). Regularization did not significantly change the fit, suggesting that the feature representation is relatively stable.

Coefficient analysis (using the Ridge model) showed:

- Positive contributions to profit: Technology category, Copiers, Binders, higher Sales.  
- Negative contributions to profit: high Discount levels, Tables, Bookcases, Storage.  

These results confirm that profit is highly sensitive to discounting and product mix.

---

### Deliverable 3 - Classification, Clustering, and Pattern Mining

#### Classification

To create a classification problem, a binary label `HighProfit` was defined:

- `1` if `Profit` ≥ median  
- `0` otherwise  

This produced a nearly balanced dataset.

**Models:**

- Decision Tree classifier  
- k-Nearest Neighbors (k-NN, k = 5)

Both models used the same preprocessed features as the regression step. Model performance was evaluated with accuracy, F1-score, confusion matrices, and ROC curves when probability estimates were available.

The baseline Decision Tree achieved accuracy of about 0.91 and an F1-score of about 0.91. Hyperparameter tuning with `GridSearchCV` (tuning `max_depth` and `min_samples_split`) increased performance further, with the tuned Decision Tree reaching accuracy and F1-score around 0.92.

From a practical perspective, this classifier could be used to flag low-profit orders in advance and suggest adjustments to pricing or discounts.

#### Clustering

K-Means clustering was applied to scaled numeric features: `Sales`, `Quantity`, `Discount`, and `Profit`. Three clusters were selected for interpretability.

Interpreting the cluster centers in the original scale suggested three order segments:

- Discount-heavy, low- or negative-profit transactions.  
- High-sales, high-profit transactions.  
- Low-sales, low-profit but relatively stable transactions.  

These segments can support differentiated strategies for pricing, promotions, and customer targeting.

#### Association Rule Mining

Association rules were generated from Category and Sub-Category combinations at the order level. A one-hot encoded basket matrix was created and the Apriori algorithm was used to find frequent itemsets, followed by rule generation with lift and confidence filtering.

The resulting rules revealed meaningful product affinities within Office Supplies and Technology, such as Binders frequently appearing with Paper, and some Technology items co-occurring with particular furniture or supply sub-categories. These patterns can be used to design bundles, cross-sell recommendations, and more effective catalog or store layouts.

---

## 🎯 Final Insights and Recommendations (Deliverable 4)

Bringing together all deliverables, the project suggests several key insights:

1. **Discount policies need careful control.**  
   Heavy discounting, especially on certain furniture items, consistently erodes profit. The business should review and tighten discount rules where orders are frequently unprofitable.

2. **High-margin product categories should be prioritized.**  
   Technology and specific office supply items (e.g., Copiers and Binders) have strong positive contributions to profit. These products can be emphasized in marketing, featured prominently in catalogs, and highlighted in promotions.

3. **Predictive models can support proactive decision making.**  
   The tuned Decision Tree classifier reliably distinguishes between high-profit and low-profit orders. Integrated into an order-entry system, this model could trigger alerts before a low-profit order is confirmed and recommend adjustments.

4. **Order segments call for tailored strategies.**  
   K-Means clustering reveals distinct transaction segments: discount-heavy loss-making orders, high-value profitable orders, and small, lower-risk orders. Each segment can be managed differently, with stricter controls on unprofitable segments and additional nurturing for the most profitable ones.

5. **Association rules highlight cross-sell opportunities.**  
   Frequently co-purchased combinations suggest natural bundles and recommendations. These can be used for targeted promotions, checkout suggestions, and inventory planning.

---

## ⚖️ Ethical Considerations

Even though the Superstore dataset does not include highly sensitive identifiers, several ethical issues remain important:

- **Data privacy:** Detailed geographic and transactional data should be handled carefully to avoid re-identification. Results should be reported in aggregate rather than exposing individual customers.
- **Fairness and bias:** Models might inadvertently learn that certain regions or segments are less profitable and drive systematically worse promotions or offers for those groups. Regular monitoring and fairness checks are needed.
- **Responsible use:** The models in this project are best viewed as **decision support tools**. Human decision-makers should interpret model outputs in context and consider long-term customer relationships and organizational values before taking action.

---

## 📂 Repository Structure

```text
MSCS_634_Project/
│
├── project.ipynb                   # Consolidated notebook for Deliverables 1–4
├── README.md                       # This file
├── report/
│   └── final_project_report.pdf    # Final written report 
├── presentation/
│   └── final_presentation.mp4      # Final Project Presentation
├── src/
│   └── utils.py
└── data/
    ├── Sample - Superstore.csv
    └── cleaned_superstore.csv
```

---

## ▶️ How to Run

```bash
python -m venv .venv
source .venv/bin/activate           # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter lab
```

Then open `project.ipynb` and run all cells.
