# Hokkaido Rice Yield Forecast with Machine Learning

*An end-to-end Machine Learning project to forecast agricultural yield using a data-driven, domain-knowledge approach and tree-based models for time-series analysis.*

---

### Table of Contents
1. [Project Overview](#1-project-overview)
2. [Data Sources](#2-data-sources)
3. [Methodology](#3-methodology)
4. [Model Challenges](#4-model-challenges)
5. [Results](#5-results)
6. [Author](#6-author)

---

## 1. Project Overview

This data science project develops a machine learning model to predict **rice yield** in Hokkaido, Japan. It leverages a combination of satellite data (NDVI), daily climate data, and historical harvest statistics, enhanced by **domain-knowledge-based feature engineering**.

The workflow is structured across six Jupyter notebooks, covering the entire process from data cleaning and integration to final modeling. A key challenge addressed was **data scarcity**, given that each location yields a harvest only once per year, making standard time-series approaches non-trivial.

---

## 2. Data Sources

Three primary data sources were used, covering the period from 2014 to 2023:
* **Monthly NDVI Data:** A satellite-derived vegetation index indicating the health and density of plant life.
* **Daily Climate Data:** Including temperature (`T2M`), humidity (`RH2M`), and solar radiation (`ALLSKY_SFC_SW_DWN`).
* **Historical Harvest Data:** Annual statistics on planted area and total harvest volume per municipality.

---

## 3. Methodology

The project followed a logical sequence, with each notebook dedicated to a specific stage of the data science pipeline.

| Notebook | Stage | Description & Key Actions |
| :--- | :--- | :--- |
| **01** | `NDVI EDA` | Time-series data processing. Missing monthly values were handled using **forward fill** and **linear interpolation (LI)** to ensure data continuity, with LI yielding the best results. |
| **02** | `Weather EDA` | **Advanced Feature Engineering**. Over 40 features were engineered based on agronomic knowledge, including calculating indices like **GDD, VPD, ET₀, and PTU**, which significantly improved model performance. |
| **03** | `Harvest EDA` | **Critical Target Variable Analysis**. The target variable was changed from "total harvest" to **"yield per area"** to prevent data leakage and focus on the true crop efficiency. |
| **04** | `Merge & Final EDA` | All datasets were merged, and a correlation analysis was performed to validate the engineered features and identify the most promising candidates. |
| **05** | `Feature Validation`| Formal feature selection was conducted using **Random Forest feature importances**, resulting in the selection of the **top 20 most predictive features**. |
| **06** | `Modeling` | Training and Evaluation. Data was split using a **chronological (time-based) split** (training up to 2021, testing on 2022-2023). Multiple models were compared against a baseline, with the **LGBM Regressor** selected as the best performer. |

---

## 4. Model Challenges

Several challenges were overcome to ensure the model's robustness and validity:

* **Data Quality and Gaps:**
    * **Challenge:** The NDVI dataset had missing months, breaking the time-series continuity. The harvest dataset contained zero-value entries and summary rows that distorted statistics.
    * **Solution:** Time-series imputation techniques were applied to the NDVI data. Inconsistent entries and summary rows were carefully filtered from the harvest data.

* **Target Variable Definition:**
    * **Challenge:** The initial target, "total harvest" (`収穫量`), was highly correlated with "planted area" (`作付面積`), which would lead to **data leakage**.
    * **Solution:** The problem was reframed to predict **"yield per area"** (`10a当たり収量`). This metric removes size bias and reflects the true crop efficiency influenced by external factors.

* **Feature Engineering Complexity:**
    * **Challenge:** Raw daily climate data showed low predictive power.
    * **Solution:** Extensive research was conducted to apply agronomic domain knowledge. Over 40 features were engineered to quantify the climate's impact on specific rice growth stages using indices like **GDD (Growing Degree Days)** and **VPD (Vapor Pressure Deficit)**.

* **Validation in a Time-Series Context:**
    * **Challenge:** A random train-test split would provide an unrealistic and overly optimistic performance evaluation due to the look-ahead problem.
    * **Solution:** A **chronological (time-based) split** was implemented, training the model on past data to predict future years, simulating a real-world forecasting scenario.

---

## 5. Results

The final LightGBM model demonstrated a significant improvement over the baseline, validating the effectiveness of the feature engineering and modeling strategy. The model achieved an **R² of 0.87**, indicating it explains 87% of the variance in rice yield on the test set. And the MedianAE shows that the model's predictions are more stable than the baseline.

| Model | RMSE | MAE | R² | MedianAE | MAPE (%) |
| :--- | ---: | ---: | ---: | ---: | ---: |
| **LGBM Regressor (Final Model)** | **18.17** | **12.32** | **0.87** | **7.51** | **2.33** |
| Baseline (Mean) | 20.10 | 16.33 | 0.84 | 14.44 | 2.98 |

## 6. Author
www.linkedin.com/in/othon-kiemon-tsutiya-paris <br>
github.com/othonkie
