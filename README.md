# Purchase Intent Prediction & Conversion Optimisation
### E-Commerce Session Analysis | Professional Cosmetics Platform

[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
[![LightGBM](https://img.shields.io/badge/Model-LightGBM-green.svg)](https://lightgbm.readthedocs.io/)
[![Tableau](https://img.shields.io/badge/Visualisation-Tableau-blue.svg)](https://www.tableau.com/)

---

## Project Overview

This project analyses **4.5 million e-commerce sessions** from a professional cosmetics supply platform serving the Russian market (Oct 2019 – Feb 2020). The goal is to identify where purchase decisions break down, predict purchase intent before a session ends, and propose a targeted recommendation system to improve conversion.

**Overall conversion rate: 3.43%** — with 86.49% of cart sessions failing to convert to purchase.

---

## Business Problem

> *How do we identify high-intent users before they leave without purchasing, and what can we recommend to convert them?*

**Audience:** Marketing and operations team of a professional cosmetics e-commerce platform  
**Persona:** E-Commerce Marketing Manager responsible for conversion rate optimisation  
**Core Need:** Identify users most likely to purchase before their session ends and intervene with targeted recommendations

---

## Dataset

- **Source:** [E-Commerce Events History in Cosmetics Shop — Kaggle](https://www.kaggle.com/datasets/mkechinov/ecommerce-events-history-in-cosmetics-shop)
- **Size:** 19.6M events → 4.5M sessions after aggregation
- **Period:** October 2019 – February 2020
- **Fields:** event_time, event_type, product_id, category_id, category_code, brand, price, user_id, user_session

> ⚠️ Raw data files are not included in this repository due to size. Please download directly from Kaggle using the link above.

---

## Methodology

```
Raw Events (19.6M)
      ↓
Data Cleaning & Session Aggregation (4.5M sessions)
      ↓
Exploratory Data Analysis (EDA)
      ↓
Funnel Analysis & Feature Engineering
      ↓
Predictive Modelling (LR → LR Optimised → Random Forest → LightGBM)
      ↓
Session Segmentation (High / Medium / Low Intent)
      ↓
Diagnosis of High Intent Non-Converting Sessions
      ↓
Recommendation System
```

---

## Key Findings

### Funnel Analysis
| Stage | Sessions | Drop-off Rate |
|---|---|---|
| View | 4,280,699 | — |
| Cart | 788,430 | 81.58% |
| Purchase | 106,526 | 86.49% |

**Cart → Purchase is the dominant drop-off point** (86.49% abandonment)

### Predictive Model
| Model | ROC-AUC | Recall | Precision |
|---|---|---|---|
| Logistic Regression (default) | 0.9143 | 0.84 | 0.16 |
| Logistic Regression (threshold 0.654) | 0.9143 | 0.70 | 0.23 |
| Random Forest | 0.9029 | 0.45 | 0.48 |
| **LightGBM** | **0.9764** | **0.99** | **0.21** |

**Final model: LightGBM** — ROC-AUC 0.9764, threshold 0.3, prioritising recall (0.99) for recommendation system use. High recall ensures 99% of true buyers are captured, justified by near-zero marginal cost of in-platform recommendations.

### Session Segmentation (LightGBM-based)
| Segment | Threshold | Sessions | % of Total | Conversion Rate |
|---|---|---|---|---|
| High Intent | prob ≥ 0.733 | 453,182 | 10.0% | 31.1% |
| Medium Intent | 0.023 ≤ prob < 0.733 | 682,463 | 15.0% | 2.1% |
| Low Intent | prob < 0.023 | 3,400,295 | 75.0% | 0.002% |

Separation ratio: **1,555x** between High and Low Intent segments.

### Diagnosis of High Intent Non-Converting Sessions
Top behavioural differences between converted vs non-converted High Intent users:

| Feature | Converted | Not Converted | Difference |
|---|---|---|---|
| view_depth | 4.10 | 3.02 | **+35.9%** |
| total_events | 27.59 | 20.59 | **+34.0%** |
| session_duration_min | 35.36 | 42.22 | -16.2% |
| avg_price | 7.41 | 7.12 | +4.1% |

> **Key Insight:** Converted users explore MORE products and pay MORE. Broader exploration builds purchase confidence — it does not suppress spending. The recommendation strategy is therefore designed to increase product exposure depth, not push cheaper alternatives.

### Recommendation System
- **Target:** 309,658 High Intent sessions (added to cart but did not purchase)
- **Logic:** Recommend unseen top-selling products in the user's cart categories
- **Validation:** 9,683 recommendations generated for 1,000 sessions (99.8% coverage)
- **Exposure Strategy:** Probability-ranked tiered exposure (High / Normal / No Exposure)

---

## Repository Structure

```
ecommerce-conversion-analysis/
│
├── notebooks/
│   ├── 1 cleaning & EDA.ipynb                # Data cleaning & exploratory analysis
│   ├── 2 funnel analysis& feature enginerring.ipynb   # Funnel analysis & feature engineering
│   ├── 3 modelling&segmentation.ipynb       # LightGBM modelling & segmentation
│   ├── 4 diagnosis.ipynb                    # High Intent session diagnosis
│   ├── 5 recommendation.ipynb               # Recommendation system
│   └── 6 visualisation.ipynb               # Python charts
│
├── data/
│   ├── sessions_featured.csv               # Session-level features
│   ├── sessions_modeled_lgb.csv            # Sessions with LightGBM probabilities
│   ├── recommendations_sample.csv          # Sample recommendations (1,000 sessions)
│   ├── diagnosis_behavioral.csv            # Behavioral comparison results
│   └── tableau/                            # Tableau-ready summary CSVs
│
├── reports/
│   └── Technical_Report.docx           # Full technical report
│
├── visualisations/
│   ├── rec_chart1_target_sessions.png
│   └── rec_chart2_exposure_tiers.png
│
└── README.md
```

---

## How to Run

### Requirements
```bash
pip install pandas numpy scikit-learn lightgbm matplotlib seaborn
```

### Run Order
```
1 cleaning & EDA.ipynb                
2 funnel analysis& feature enginerring.ipynb   
3 modelling&segmentation.ipynb       
4 diagnosis.ipynb                   
5 recommendation.ipynb               
6 visualisation.ipynb  
```

> Download the raw dataset from Kaggle and place it in a `data/raw/` folder before running notebook 1.

---

## Tools & Technologies

| Category | Tools |
|---|---|
| Language | Python 3.9+ |
| Data Processing | Pandas, NumPy |
| Modelling | Scikit-learn, LightGBM |
| Visualisation | Matplotlib, Seaborn, Tableau |
| Reporting | Jupyter Notebook, MS Word |

---

## Author

**Eva Sun**  
GitHub: [@EvaSun0406](https://github.com/EvaSun0406)  
Data: [Kaggle Dataset](https://www.kaggle.com/datasets/mkechinov/ecommerce-events-history-in-cosmetics-shop)

---

*This project was completed as a data analytics capstone, covering the full end-to-end workflow from raw data cleaning to predictive modelling and business recommendations.*
