# Advanced 8-Dimension Customer Segmentation (A Validated Model)

This repository contains a complete, end-to-end data science project that performs an advanced segmentation of customers from the **Online Retail II dataset**.

The core of this project is a departure from traditional 3-feature RFM (Recency, Frequency, Monetary) analysis. It proposes, implements, and **validates** a custom **8-dimension "Customer DNA" profile** designed to provide a richer, more holistic view of customer behavior.

The model successfully segments customers into four distinct, actionable personas. The validity of these clusters is proven using a "surrogate" classifier, which achieved **94.6% accuracy** in predicting cluster membership, confirming the segments are mathematically sound and well-defined.

The analysis, methodology, and code in this notebook were developed by **Anuj**.

---

## 📋 Table of Contents

* [The Advanced Approach: Beyond RFM](#-the-advanced-approach-beyond-rfm)
* [The 8-Feature "Customer DNA" Profile](#-the-8-feature-customer-dna-profile)
* [Key Methodologies & Validation](#-key-methodologies--validation)
* [The 4 Final Customer Personas](#-the-4-final-customer-personas)
* [Project Workflow](#-project-workflow)
* [How to Run This Project](#-how-to-run-this-project)
* [Technologies Used](#-technologies-used)
* [Author](#-author)

---

## 🚀 The Advanced Approach: Beyond RFM

Traditional RFM models are simple but **underfit** the data. They fail to capture true business value and misclassify critical segments, such as:
* Lumping a "high-value customer" and a "high-return/low-profit customer" into the same 'M' (Monetary) cluster.
* Treating a brand-new customer with 5 orders the same as a 6-year-old customer with 5 orders (Frequency).

This 8-feature model was designed to solve these problems by engineering a profile that captures not just transactional habits but also **long-term loyalty** and **true profitability**.

---

## 🔬 The 8-Feature "Customer DNA" Profile

This model is built on 8 custom-engineered features derived from two different data sources (`df_clean` for valid purchases, `df_raw` for full historical context).

### Transactional Metrics (Purchase Style)
1.  **`Recency`**: Days since the last valid purchase.
2.  **`TotalMonetary`**: Total lifetime value from all valid purchases.
3.  **`TotalOrders`**: Total number of unique, valid orders.
4.  **`AverageOrderValue (AOV)`**: The average monetary value per order.
5.  **`AverageBasketSize`**: The average number of items per order.
6.  **`ProductDiversity`**: The number of unique products the customer has purchased.

### Behavioral Metrics (Loyalty & Profitability)
7.  **`CustomerTenure`**: Days since the customer's *first-ever* transaction (a true loyalty metric).
8.  **`CancellationRate`**: The percentage of a customer's total invoices that were cancelled (a direct profitability proxy).

---

## 💡 Key Methodologies & Validation

This project's key differentiator is its rigorous, multi-step validation process.

### 1. Advanced Modeling (K-Means vs. DBSCAN)
Two separate models were built to find the best approach:
* **K-Means:** An Elbow Plot suggested 4 clusters. The final model (Block 8) successfully segmented the 3,125 purchasing customers into 4 distinct groups.
* **DBSCAN:** This density-based model was systematically tuned (Block 11, Knee Plot) and identified **6 dense clusters** along with **462 (14.8%) "Outlier" customers**—a powerful segment that K-Means cannot find.

### 2. Validation: The "Shuffle Test" (94.6% Accuracy)
This is the most critical validation step in the project (Block 13).
* **Problem:** Are the 4 K-Means clusters "real" or just an artificial result of the algorithm?
* **Method:** A "surrogate" **Random Forest Classifier** was trained to predict the K-Means cluster labels based on the 8 features.
* **Result 1 (The "Accuracy"):** The surrogate model achieved **94.62% accuracy** on the test set. This *proves* the 4 clusters are well-defined, mathematically separable, and not a random illusion.
* **Result 2 (The "Shuffle Test"):** A **Permutation Importance** test (based on Monte Carlo logic) was run to find the most important features.

    | Feature | Importance (Drop in Accuracy) |
    | :--- | :--- |
    | **`CustomerTenure`** | **0.17** |
    | `Recency` | 0.11 |
    | `TotalMonetary` | 0.11 |
    | `TotalOrders` | 0.09 |
    | ... | ... |

* **Conclusion:** The test proved that **`CustomerTenure`** (our custom, non-RFM feature) was the **#1 most important feature** in defining the segments. This validates the 8-feature model and confirms it provides richer insights than a 3-feature RFM model.

---

## 🏆 The 4 Final Customer Personas

The 4-cluster K-Means model was selected for its balance of interpretability and statistical validity. A "Snake Plot" and statistical analysis (Block 9) were used to create the following business-ready personas:

1.  **Cluster 2: "Champions" (1,212 customers)**
    * *Profile:* High `TotalMonetary`, high `TotalOrders`, high `CustomerTenure`, low `Recency`.
    * *Persona:* Our most loyal, high-spending VIPs.
    * *Strategy:* **Retain & Reward.**

2.  **Cluster 3: "New & Promising" (790 customers)**
    * *Profile:* Low `CustomerTenure`, low `Recency`, low `CancellationRate`.
    * *Persona:* New, active, and well-behaved customers.
    * *Strategy:* **Nurture & Grow.**

3.  **Cluster 0: "High-Value Churn Risk" (1,185 customers)**
    * *Profile:* High `TotalMonetary`, high `CancellationRate`, high `Recency`.
    * *Persona:* High-spending customers who are fading away and have a history of high returns.
    * *Strategy:* **Win-Back & Investigate.**

4.  **Cluster 1: "Lost Customers" (1,151 customers)**
    * *Profile:* Very high `Recency`, low `TotalMonetary`, low `TotalOrders`.
    * *Persona:* Low-value, churned customers.
    * *Strategy:* **Do Not Invest.**

---

## 🔄 Project Workflow

The notebook `Anuj_Customer_Segmentation.ipynb` is a single, complete pipeline (14 blocks):

1.  **Data Acquisition:** Installs `gdown` and downloads both source datasets (CSV and XLSX).
2.  **Feature Engineering:** Builds the 8-feature "Customer DNA" profile from the two data sources (Blocks 3-4).
3.  **Exploratory Data Analysis (EDA):** A 4-part EDA (Blocks 6-9) on the *raw* features, culminating in a 3D t-SNE visualization to confirm "clusterability."
4.  **Modeling (K-Means & DBSCAN):** Includes Elbow Method, K-Means clustering, DBSCAN first-run, Knee Plot hyperparameter tuning, and a final DBSCAN run (Blocks 7-12).
5.  **Validation & Profiling:** Runs the Permutation Importance "Shuffle Test" (Block 13), profiles the 4 K-Means clusters (Block 9), and generates a Snake Plot.
6.  **Export:** Saves the final list of `CustomerID`s and their named `Persona` to `customer_segments.csv` (Block 14).

---

## 🚀 How to Run This Project

1.  Open `Anuj_Customer_Segmentation.ipynb` in a Google Colab or local Jupyter environment.
2.  Run the notebook cells in order. The notebook is fully self-contained and will:
    * Install all dependencies.
    * Download both required data files.
    * Run the full analysis, modeling, and validation.
    * Generate all plots and the final `customer_segments.csv` file.

---

## 🛠️ Technologies Used

* **Python 3**
* **Data Manipulation:** `pandas`, `numpy`
* **Modeling & Preprocessing:** `scikit-learn` (KMeans, DBSCAN, StandardScaler, PCA, t-SNE, RandomForestClassifier, permutation_importance)
* **Data Visualization:** `matplotlib`, `seaborn`
* **Interactive Visualization:** `plotly`
* **Data Sourcing:** `gdown`

---

## 👤 Author

* **Anuj** - *Methodology, Feature Engineering, Modeling, Validation, and Implementation*
