# Mining Supply Chain Inventory Risk Analysis

This project analyzes inventory risks in mining supply chains using SQL.

## Objectives

- Identify materials at risk of stockout
- Analyze supplier lead time impact
- Evaluate reorder point effectiveness

## Tools Used

- SQL
- PostgreSQL
- GitHub

## Dataset

Simulated mining supply chain dataset including:

- mine sites
- inventory levels
- supplier lead times
- reorder thresholds

## Key Analysis

1. Materials at risk of stockout
2. Supplier lead time impact on inventory
3. Inventory planning optimization

## Key Insights
## 1️⃣ Inventory Items at Risk of Stockout
Explanation
This analysis identifies inventory items where the current stock level has fallen below the reorder threshold. These items are considered at risk of stockout because the remaining inventory may not be sufficient to sustain operations until new stock arrives.

## SQL Query
```sql
SELECT
    item_id,
    category,
    stock_level,
    reorder_point
FROM warehouse_inventory
WHERE stock_level < reorder_point
ORDER BY stock_level ASC;
```
### Result Visualization
![Stockout Risk Result](stockout_risk_result.png)
### Result Insight
The analysis identified **237 inventory items** with stock levels below their reorder threshold. These items represent potential stockout risks and require immediate replenishment planning to avoid supply chain disruptions.

## 2️⃣ Inventory Shortage Severity Analysis
Explanation
This analysis measures the severity of inventory shortages by calculating the difference between the reorder point and the current stock level. The larger the gap between these values, the more critical the shortage becomes.

## SQL Query
```sql
SELECT
    item_id,
    category,
    stock_level,
    reorder_point,
    reorder_point - stock_level AS shortage_amount
FROM warehouse_inventory
WHERE stock_level < reorder_point
ORDER BY shortage_amount DESC;
```
### Result Visualisation
![Shortage Severity Result](shortage_severity_result.png)
### Result Insight
The analysis highlights inventory items with the largest gaps between stock levels and reorder thresholds. These items represent the most urgent replenishment priorities.
