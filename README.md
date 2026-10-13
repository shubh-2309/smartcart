# SmartCart: E-Commerce Customer Segmentation

SmartCart groups e-commerce customers into segments by their demographics and purchase behaviour. The project uses unsupervised machine learning: PCA for dimensionality reduction, then K-Means and Agglomerative Clustering. A business can use the segments to target marketing campaigns and plan retention and personalization.

> Minor project: customer segmentation with Python, pandas and scikit-learn.

---

## Table of Contents
- [Overview](#overview)
- [Dataset](#dataset)
- [Project Workflow](#project-workflow)
- [Customer Segments](#customer-segments)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [Future Improvements](#future-improvements)

---

## Overview

Customers differ in what they buy, how much they spend and which channel they shop through. If a business treats them all the same, its marketing reaches people it doesn't fit. This project:

1. Cleans and preprocesses raw customer data
2. Engineers features such as **Age**, **Customer Tenure**, **Total Spending** and **Total Children**
3. Reduces the feature space with **PCA (3 components)**
4. Finds the best number of clusters with the **Elbow Method** (`kneed`)
5. Segments customers with **K-Means** and **Agglomerative (Ward) Clustering**
6. Profiles each cluster and turns it into a business persona

---

## Dataset

**File:** `smartcart_customers.csv` (2,240 customers × 22 attributes)

| Category | Columns |
|---|---|
| **Demographics** | `Year_Birth`, `Education`, `Marital_Status`, `Income`, `Kidhome`, `Teenhome` |
| **Engagement** | `Dt_Customer` (enrolment date), `Recency` (days since last purchase), `Complain`, `Response` (accepted last campaign) |
| **Spending (2 yrs)** | `MntWines`, `MntFruits`, `MntMeatProducts`, `MntFishProducts`, `MntSweetProducts`, `MntGoldProds` |
| **Purchase channels** | `NumDealsPurchases`, `NumWebPurchases`, `NumCatalogPurchases`, `NumStorePurchases`, `NumWebVisitsMonth` |

---

## Project Workflow

### 1. Data Cleaning
- Filled the 24 missing `Income` values with the **median**
- Removed outliers: `Age ≥ 90` and `Income ≥ 600,000` (2,240 → **2,236** rows)

### 2. Feature Engineering
| New Feature | Derivation |
|---|---|
| `Age` | `2026 - Year_Birth` |
| `Customer_Tenure_Days` | Days between enrolment date and the latest enrolment date |
| `Total_Spending` | Sum of all six `Mnt*` product categories |
| `Total_Children` | `Kidhome + Teenhome` |
| `Education` | Grouped into `Undergraduate`, `Graduate`, `Postgraduate` |
| `Living_With` | `Marital_Status` mapped to `Couple` / `Single` |

Redundant raw columns (`ID`, `Year_Birth`, `Marital_Status`, `Kidhome`, `Teenhome`, `Dt_Customer`, the individual `Mnt*` columns) were dropped after the derived features were created.

### 3. Exploratory Data Analysis
- Pair plots to spot outliers and relationships
- Correlation heatmap of the numeric features

### 4. Preprocessing
- **One-Hot Encoding** for `Education` and `Living_With`
- **StandardScaler** on all features

### 5. Dimensionality Reduction
- **PCA** to 3 components (explained variance ≈ 23.2%, 11.4% and 10.4%)

### 6. Choosing K
- WCSS computed for k = 1–10. `KneeLocator` found the elbow at **k = 4**.

### 7. Clustering
- **K-Means** (k = 4) and **Agglomerative Clustering** (Ward linkage, k = 4)
- Clusters visualized in 3D PCA space
- The Agglomerative labels were used for the final cluster profiling

---

## Customer Segments

Mean values per cluster (Agglomerative Clustering):

| Cluster | Avg Income | Avg Total Spending | Avg Children | Living With | Campaign Response | Top Channel |
|---|---|---|---|---|---|---|
| **0** | ~$39.7K | ~$222 | 1.24 | Couple | 7.6% | Store / Web visits |
| **1** | ~$72.8K | ~$1,237 | 0.51 | Couple | 16.7% | Store & Catalog |
| **2** | ~$37.0K | ~$166 | 1.27 | Single | 14.2% | Store / Web visits |
| **3** | ~$70.7K | ~$1,190 | 0.46 | Single | **32.0%** | Store & Catalog |

### Personas & Business Insights
- **Cluster 0: Budget-Conscious Families (Couples).** Lower income, more children, low spending. They browse the website often but buy little and seldom respond to campaigns. *Strategy:* deals, family bundles and discounts.
- **Cluster 1: Affluent Couples.** High income and the highest spending, with few children. They buy heavily in stores and through catalogs. *Strategy:* premium products, loyalty programs and upselling.
- **Cluster 2: Budget-Conscious Singles.** Lowest spending, with children at home, and they visit the website often. *Strategy:* targeted promotions and turning web visits into purchases.
- **Cluster 3: Affluent Singles.** High income and high spending, and the **most responsive to marketing campaigns (32%)**. *Strategy:* the main target for new campaigns and exclusive offers.

---

## Tech Stack

- **Language:** Python 3
- **Libraries:** pandas, NumPy, Matplotlib, Seaborn, scikit-learn, kneed
- **Environment:** Jupyter Notebook

---

## Getting Started

### Prerequisites
- Python 3.9+
- Jupyter Notebook or JupyterLab

### Installation

```bash
git clone https://github.com/<your-username>/SmartCart.git
cd SmartCart
pip install pandas numpy matplotlib seaborn scikit-learn kneed jupyter
```

### Run

```bash
jupyter notebook smartcart.ipynb
```

Run the cells from top to bottom. `smartcart_customers.csv` must be in the same directory as the notebook.

---

## Project Structure

```
SmartCart/
├── smartcart.ipynb            # Main notebook: cleaning, EDA, PCA, clustering, profiling
├── smartcart_customers.csv    # Customer dataset
└── README.md
```

---

## Future Improvements

- Evaluate cluster quality with **Silhouette Score** and **Davies–Bouldin Index**
- Compare against **DBSCAN** and **Gaussian Mixture Models**
- Add **RFM (Recency, Frequency, Monetary)** analysis
- Build an interactive dashboard (e.g. **Streamlit**) to explore segments
- Package the pipeline as a reusable script or model for new customers

---

## Author

**Shubh Mohanka**

If you found this project useful, consider giving it a ⭐ on GitHub!
