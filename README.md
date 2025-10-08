# Capstone_Project
**Price Optimization for E-commerce — Final Report**

**Author:** Yashaswi Raval

---

### Executive Summary
Forecast demand and estimate price sensitivity to suggest safe, modest price changes that lift profit without hurting customer trust.

### Rationale
Price is a lever we control. Data-driven guidance prevents under/over-pricing and keeps prices fair and stable.

### Research Question
How can an e-commerce store adjust prices to increase profit while keeping customers satisfied and loyal?

### Data Sources
Kaggle – Predict Future Sales  
https://www.kaggle.com/competitions/competitive-data-science-predict-future-sales/data

---

### 1) Define the Problem Statement
Retailers risk under-pricing (lost profit) or over-pricing (lost customers and trust). The goal is to **recommend small, guardrailed price changes** that increase **profit** while keeping prices **stable and fair**. Challenges include sparse demand at the `(shop_id, item_id)` level, seasonality, returns, and price–quantity confounding. Benefits include higher contribution margin, faster inventory turns, and improved customer perception.

### 2) Model Outcomes or Predictions
- **Learning type:** **Regression** (supervised).  
- **Target:** `item_cnt_month` (monthly demand per `(shop_id, item_id)`), used to simulate revenue/profit under candidate prices.  
- **Algorithms compared:**  
  - Seasonal Average **baseline** (supervised; simple rule-based regression baseline).  
  - **Random Forest** (supervised, tree-based).  
  - **Gradient Boosting** (supervised, tree-based; second technique for comparison).

### 3) Data Acquisition
- **Primary:** Kaggle *Predict Future Sales*: `sales_train.csv`, `items.csv`, `item_categories.csv`, `shops.csv`, `test.csv`.  
- **Joins:** items → categories; shops; build monthly panel by `date_block_num`.  
- **EDA visualizations:**  
  - Monthly units over time (seasonality / holiday effects).  
  - Price distribution (heavy tail).  
  - Top categories/shops by volume.  
These visuals confirm seasonality, skewed prices, and concentration—supporting the need for lags and calendar features.

### 4) Data Preprocessing/Preparation
a. **Cleaning:**  
   - Parse dates; clip outliers for counts (e.g., 99.9th percentile) and nonnegative prices.  
   - Aggregate **daily → monthly**: `item_cnt_month = sum(item_cnt_day)`, `avg_item_price = mean(item_price)`.  
b. **Split:**  
   - **Time-based**: train ≤ `val_block - 1`, validate on `val_block = 1` (from executed runs).  
c. **Feature engineering / encoding:**  
   - Calendar: `month = date_block_num % 12`, `quarter`.  
   - Popularity proxies: shop/item monthly averages.  
   - **Lags**: `item_cnt_month_lag{1,2,3,6}`, `avg_item_price_lag{1,2,3}`.  
   - Numeric IDs kept as integers; tree models handle them as categorical-like splits without one-hot.

### 5) Modeling
- **Baseline:** Seasonal average by `(shop_id, item_id, month)` with back-offs (item mean → global mean).  
- **Model 1:** **Random Forest** (lags + calendar + popularity + IDs).  
- **Model 2:** **Gradient Boosting** (same features) to provide a second, competitive benchmark.  
- **Rough Elasticities (category-level):** log–log regression of `log(1+qty)` on `log(price)` with month controls (via time aggregation).  
- **Scenario Calculator:** For ±5% price moves, approximate revenue impact; apply guardrails (cap ±5%, avoid frequent swings, use friendly endings).

### 6) Model Evaluation
- **Validation month:** `val_block = 1` (time-based).  
- **Metrics:** Mean Absolute Error (MAE), Root Mean Squared Error (RMSE).  
- **Executed results:**  
  - **Seasonal baseline:** MAE **1.9981**, RMSE **1.9981**  
  - **Random Forest:** MAE **1.0000**, RMSE **0.7542**  
  - **Gradient Boosting:** MAE **1.0000**, RMSE **0.7866**  
- **Conclusion:** **Random Forest** achieved the lowest RMSE, indicating that **lagged demand**, **calendar**, and **shop/item context** capture key demand drivers.  
- **Pricing implications:**  
  - Use modest (±5%) and infrequent changes.  
  - Favor increases for categories with **near-zero elasticities** and stable/rising demand.  
  - Avoid increases in categories with **strongly negative elasticities** or declining momentum.  

---

### Next Steps
Multi-month backtests; add margin & inventory for profit lift; extend features (promo/holiday flags);  refine guardrails; package an operations-friendly price playbook.

### Outline of Project
- **EDA & Aggregation:** `Ecommerce_Price_Optimization_EDA_Aggregation.ipynb`  
- **Modeling & Scenarios:** `Ecommerce_Price_Optimization_Modeling.ipynb`

### Contact
Email: ravalyashaswi@gmail.com
[LinkedIn](https://www.linkedin.com/in/yashaswi-raval/)


