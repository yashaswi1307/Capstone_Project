# Capstone_Project
**Price Optimization for E-commerce**

**Author:** Yashaswi Raval

#### Executive Summary
Forecast demand and estimate price sensitivity to suggest safe price changes that lift profit without hurting customer trust.

#### Rationale
Price is a lever we control. Data-driven guidance prevents under/over-pricing and keeps prices fair and stable.

#### Research Question
How can an e-commerce store adjust prices to increase profit while keeping customers satisfied and loyal?

#### Data Sources
Kaggle – Predict Future Sales  
https://www.kaggle.com/competitions/competitive-data-science-predict-future-sales/data

#### Methodology
Clean & aggregate to monthly → add lags & calendar features → seasonal baseline + tree model → category elasticities → ±5% scenario tests.

#### Results
- **Validation split (val_block = 1)**  
- **Seasonal baseline:** MAE **1.9981**, RMSE **8.1492**  
- **Random Forest (lags + calendar + popularity):** MAE **0.7556**, RMSE **1.9506**  
- **Takeaway:** The tree model substantially outperforms the seasonal baseline, indicating that lag features and simple context features capture important demand patterns.

#### Next Steps
Roll backtests, add margin/inventory for profit lift, refine guardrails, package a simple price playbook.

#### Outline of Project
- [Notebook: EDA, Modeling & Elasticity](solution.ipynb)

#### Contact
ravalyashaswi@gmail.com


