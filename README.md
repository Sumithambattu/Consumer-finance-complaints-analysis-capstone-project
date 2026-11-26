<div align="center">

# 🏦 Consumer Financial Complaints (CFPB) — Exploratory Data Analysis


![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-Numerical-013243?logo=numpy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-11557C)
![Seaborn](https://img.shields.io/badge/Seaborn-Statistical%20Plots-4C72B0)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)


</div>

---

## 📑 Table of Contents
- [Project Overview](#-project-overview)
- [Objectives](#-objectives)
- [Tools & Technologies](#-tools--technologies)
- [Dataset Information](#-dataset-information)
- [Project Workflow](#-project-workflow)
- [Key Insights](#-key-insights)
- [Recommendations](#-recommendations)
- [Project Status](#-project-status)
- [Author](#-author)

---

## 📌 Project Overview

Every year, thousands of consumers file complaints about banks and financial products with the U.S. **Consumer Financial Protection Bureau (CFPB)**. This project analyses **62,516 complaints (May 2017 – Aug 2023)** from the perspective of a bank's compliance and customer-experience analytics team.

The central business question:

> *Which products, issues and channels generate the most complaints, how has that changed over time, and how fairly and promptly are complaints being resolved?*

The dataset is almost entirely categorical and date-based, with no true numeric measures. 

---

## 🎯 Objectives

1. **Measure volume and trend** — how big is complaint volume, and is it growing? Is there seasonality?
2. **Find what dominates** — which products, issues, states and submission channels generate the most complaints?
3. **Understand company responses** — how often do consumers get an explanation vs. monetary or non-monetary relief, and does the outcome depend on product or channel?
4. **Assess timeliness** — how often do companies respond on time, and is it improving or worsening?
5. **Analyse routing delays** — how long does the CFPB take to forward a complaint to the company, and what drives delays?
6. **Turn findings into action** — give practical, evidence-backed recommendations.

---

## 🛠 Tools & Technologies

| Category | Tools |
|---|---|
| **Language** | Python 3 |
| **Data manipulation** | Pandas, NumPy |
| **Visualization** | Matplotlib, Seaborn |
| **Environment** | Jupyter Notebook |
| **Data format** | Excel (`.xlsx`) |

---

## 📊 Dataset Information

| Item | Details |
|---|---|
| **Source** | Consumer Complaints database, U.S. Consumer Financial Protection Bureau (CFPB) |
| **Files** | `Consumer_Complaints.xlsx` and `ConsumerComplaints_DataDictionary.csv` |
| **Size** | 62,516 rows × 12 columns |
| **Period** | May 2017 – Aug 2023 (2017 and 2023 are partial years) |
| **Granularity** | One row = one complaint about a financial product or service |
| **Duplicates** | None, on full rows or on `Complaint ID` |

**Column groups**

| Group | Columns |
|---|---|
| Identifiers | `Complaint ID` |
| Dates | `Date submitted`, `Date received` |
| Product hierarchy | `Product` (9 values) → `Sub-product` (46 values) |
| Issue hierarchy | `Issue` (76 values) → `Sub-issue` |
| Geography | `State` |
| Channel | `Submitted via` |
| Company outcome | `Company response to consumer`, `Company public response`, `Timely response?` |

**Missing values (all structural, so no rows were dropped)**

| Column | Missing | Treatment |
|---|---|---|
| `Timely response?` | 1,494 (2.4%) | Exactly the "In progress" complaints. Labelled `Pending` and **excluded** from timeliness rates rather than counted as failures |
| `Sub-issue` | 10,858 (~17%) | Not every issue has a sub-issue → `Not specified` |
| `Company public response` | 2,175 (~3.5%) | → `Not provided` |
| `Sub-product` | 7 | → `Not specified` |

---

## 🔄 Project Workflow

```
1. Problem Definition  →  2. Setup  →  3. Data Loading & Overview
                                                │
                                                ▼
                                   4. Data Pre-processing
        ┌─────────────────────────────────────────────────────────────┐
        │ Standardise column names · Check duplicates · Handle missing │
        │ values · Fix data types & logic checks · Feature engineering
          · Outlier review            │
        └─────────────────────────────────────────────────────────────┘
                                                │
                                                ▼
                               5. Exploratory Data Analysis
        ┌─────────────────────────────────────────────────────────────┐
        │ Univariate  →  Time series  →  Bivariate  →  Multivariate    │
        │ → Pivot tables & statistical tests                           │
        └─────────────────────────────────────────────────────────────┘
                                                │
                                                ▼
              6. Key Insights  →  7. Recommendations  →  8. Limitations
```

**Step-by-step**

1. **Problem definition** — business context, dataset selection and five guiding questions.
2. **Loading and overview** — shape, data types, summary statistics, data dictionary review.
3. **Pre-processing**
   - Standardised column names and confirmed there were no duplicates.
   - Investigated *why* each value was missing before choosing a treatment.
   - Harmonised near-duplicate categories (e.g. `Referral` vs `Web Referral`) and shortened long labels.
   - **Engineered features:** `routing_days`, `year`, `quarter`, `month`, `weekday`, `got_relief`, `got_monetary`, `is_closed`, `is_timely` and U.S. Census `region`.
   - Kept the heavy-tailed `routing_days` values (real business events, not errors) and used medians and non-parametric tests.
   - Reconciled the clean data against the raw data to confirm no rows were lost.
4. **EDA**
   - **Univariate:** product, channel, response outcome, top states and issues, routing-time distribution.
   - **Time series:** monthly volume with a 3-month rolling mean, yearly volume for complete years (2018â€“2022), a year Ã— month seasonality heatmap.
   - **Bivariate:** product mix over time, outcome by product, timeliness by product and channel, routing time by channel.
   - **Multivariate:** product Ã— channel relief heatmap, yearly small-multiples, state-level volume vs relief, Spearman and CramÃ©r's V association matrices.
   - **Statistics:**  Pivot table analysis.


---
##  Key Insights

| # | Insight | Evidence |
|---|---|---|
| 1 | **Complaints are highly concentrated** | 3 products make up **77.9%** of all complaints. One issue, *"Managing an account"*, is **24.2%**. Five states account for 51.6% (California alone 21.9%, raw counts) |
| 2 | **Volume is a growing trend, not seasonality** | **+65%** from 7,872 (2018) to 12,953 (2022). Months differ only modestly (7.2â€“9.2% each) |
| 3 | **The product mix is shifting** | Credit Reporting **9.7%  14.3%**. Money Transfer / Virtual Currency **3.9%  8.0%**. Mortgage **16.4%  6.0%** |
| 4 | **Relief depends strongly on the product** | Credit/Prepaid Card **40.7%** and Checking/Savings 38.5% vs Credit Reporting 19.3% and Debt Collection **11.9%**  |
| 5 | **Timeliness worsened after 2020** | 99.9% (2019) â†’ **89.0% (2021)**  95.6% (2022). Overall rate is 96.1% |
| 6 | **Referral is the slow channel** | Only **24.6%** of Referral complaints are routed same day vs **92.2%** for Web (Kruskal-Wallis p < 0.001) |
| 7 | **Channel and region matter less than product** | Fax (22.3%) and Postal mail (21.8%) get relief least often. Georgia is a low outlier at **26.0%** vs 32.7% nationally |
| 8 | **Relief rates move over time** | Mortgage 15.9% â†’ 40.0% â†’ 28.4%. Debt Collection fell steadily from 17.1% to 9.7% |

>  Correlation is not causation. For example, the 2021 timeliness drop coincides with a 25% volume jump, which suggests (but does not prove) a capacity problem.

---

##  Recommendations

1. **Target "managing an account" first** root-cause analysis on deposit, withdrawal and fee issues attacks the largest source of complaints.
2. **Plan capacity on the trend, not the season**  review resourcing against the run-rate and set an alert if the timely rate falls below 95%.
3. **Prioritise Credit Reporting and Debt Collection**  they have the lowest on-time rates (~93.6 93.7%) and the lowest relief rates.
4. **Review the Referral intake path**  automating the hand-off to the company is a quick win.
5. **Investigate outliers**  Georgia's low relief rate and the July 2023 volume spike.

**Future work:** add company names to see who drives late responses, add state population for per-capita rates, apply text analysis to complaint narratives, and build a logistic-regression model to predict relief or late responses.

---

## Limitations

- No complaint text, company name, consumer demographics or monetary amounts in the data.
- `routing_days` measures the CFPB's forwarding delay, not the company's resolution time.
- Counts are **not per-capita**, so large states naturally rank higher.
- 2017 and 2023 are partial years, so annual comparisons use 2018â€“2022 only.
- Complaints are self-reported and unverified. A high count may reflect high awareness of the CFPB rather than worse service.





---



---






## 📍 Project Status

| Phase | Status |
|---|---|
| Problem definition and dataset selection | ✅ Completed |
| Data cleaning and feature engineering | ✅ Completed |
| EDA and Visualizations | ✅ Completed |
| Insights and Reccomendations | ✅ Completed |


## Author

**Sumitha A M**  Aspiring Data Analyst | Cochin, Kerala
Skills: SQL · Power BI · Python · Excel



---

## 👩‍💻 Author
**Sumitha A M** , Aspiring Data Analyst | Cochin, Kerala
