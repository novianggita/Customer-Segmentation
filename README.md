# Customer-Segmentation using RFM Analysis

This project demonstrates how to perform customer segmentation using RFM (Recency, Frequency, Monetary) analysis in Microsoft Power BI. The goal is to categorize customers based on their purchase behavior and help businesses target the right customers with the right strategies.

## 📊 What is RFM Analysis?
RFM Analysis is a marketing technique used to quantitatively rank and segment customers based on their transaction history:
- Recency (R): How recently a customer made a purchase
- Frequency (F): How often they purchase
- Monetary (M): How much money they spend
By scoring each customer across these three dimensions, businesses can create targeted segments like:
- Champions
- Loyal Customers
- Potential Customers
- At Risk
- Need Attention


## 📑 Data Source
Kaggle: https://www.kaggle.com/datasets/vivek468/superstore-dataset-final


## ⚙️ How It Works in Power BI
1. Data Preparation: We use transactional data (order date, customer ID, sales amount) to create a summarized table (RFM_Table) using DAX functions like SUMMARIZE().
2. Metric Calculation:
   - Recency is calculated using DATEDIFF() from the last purchase date
   - Frequency is the count of unique transactions using DISTINCTCOUNT()
   - Monetary is the total spending calculated using SUM().
4. Scoring: Each metric is scored on a scale from 1 to 5 using PERCENTILE.INC() and SWITCH() functions in DAX.
5. Segment Classification: A combined RFM Score is created and used to group customers into specific segments using logical conditions.
6. Visualization: Power BI visuals are used to display the distribution of customer segments and key insights.


## 📁 Project Files
- Customer Segmentation Doc.pdf - Business insights and recommendations based on this analysis
- Customer Segmentation.ipynb - How to do this analysis using python
- Dashboard - Customer Segmentation.pbix – Power BI report file with all visuals and DAX calculations
- RFM DAX Reference.md – DAX formulas used in the report
