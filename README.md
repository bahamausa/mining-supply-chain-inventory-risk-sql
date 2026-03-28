# Mining Supply Chain Inventory Risk Analysis
This SQL project analyzes inventory risk in a simulated mining supply chain environment using PostgreSQL. The analysis identifies materials below reorder point, measures shortage severity, highlights high-risk categories, and evaluates whether supplier lead times exceed expected days until stockout.

### Key Finding
- 237 inventory items were below reorder point
- Several products were projected to stock out before replenishment could arrive
- Category-level analysis showed concentrated risk in specific material groups
- The findings suggest a need for tighter reorder planning, higher safety stock for critical items, and closer monitoring of supplier lead times

## Tools Used
- PostgreSQL
- GitHub (Project Documentation)
- Simulated Supply Chain Dataset

## Project Structure
- `*.png`- Query result visualizations
- `README.md` - Project documentation and insights
- `SQL` queries for each analysis
- 01_stockout_risk.sql
- 02_shortage_severity.sql
- 03_category_risk.sql
- 04_leadtime_stockout_risk.sql

## Dataset Schema
The project uses a simulated table called `warehouse_inventory`.
Key columns:
- `item_id` – unique material identifier
- `category` – material or spare-parts category
- `stock_level` – current inventory on hand
- `reorder_point` – threshold at which replenishment should be triggered
- `lead_time_days` – supplier delivery lead time in days
- `daily_demand` – estimated daily consumption rate

### Example Business Meaning
If a material has:
- stock_level = 20
- daily_demand = 5
- lead_time_days = 7

Then it will stock out in 4 days, but replenishment takes 7 days to arrive. That creates a 3-day supply risk gap.

## Key Analysis
1. Materials at risk of stockout
2. Supplier lead time impact on inventory
3. Inventory planning optimization

## Key Insights
## 1️⃣ Inventory Items at Risk of Stockout
### Explanation
This analysis identifies inventory items where the current stock level has fallen below the reorder threshold. These items are considered at risk of stockout because the remaining inventory may not be sufficient to sustain operations until new stock arrives.

### SQL Query
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
### Explanation
This analysis measures the severity of inventory shortages by calculating the difference between the reorder point and the current stock level. The larger the gap between these values, the more critical the shortage becomes.

### SQL Query
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

## 3️⃣ Category Inventory Risk Analysis
### Explanation
This analysis identifies which product categories contain the highest number of inventory items below their reorder point. By grouping at-risk items by category, the query highlights broader patterns in inventory management rather than focusing only on individual items.

### SQL Query
```sql
SELECT
    category,
    COUNT(*) AS items_at_risk
FROM warehouse_inventory
WHERE stock_level < reorder_point
GROUP BY category
ORDER BY items_at_risk DESC;
```
### Result Visualisation
![Category Risk Result](category_risk_result.png)
### Result Insight
The results show which product categories have the greatest concentration of inventory risk. Categories with the highest number of at-risk items may indicate weaknesses in replenishment planning and should be prioritized for closer inventory monitoring.

## 4️⃣ Lead Time vs Stockout Risk Analysis
### Explanation
This analysis evaluates supply chain risk by comparing the estimated days until inventory stockout with supplier lead times. 

### SQL Query
```sql
SELECT
    item_id,
    category,
    stock_level,
    reorder_point,
    lead_time_days,
    daily_demand,
    ROUND(stock_level / daily_demand, 2) AS days_until_stockout,
    ROUND(lead_time_days - (stock_level / daily_demand), 2) AS risk_gap
FROM warehouse_inventory
WHERE daily_demand > 0
  AND lead_time_days > stock_level / daily_demand
ORDER BY risk_gap DESC;
```
### Result Visualisation 
![Leadtime Stockout Risk Result](leadtime_stockout_risk_result.png)
### Result Insight
The results highlight products that may run out of stock before new inventory can arrive. These items require urgent replenishment planning, supplier acceleration, or increased safety stock levels to prevent supply chain disruptions.

## Sample Stakeholder Update

**Subject:** Inventory Risk Analysis Summary

Hi Operations Manager,

I completed a review of inventory risk across the warehouse dataset. The analysis identified 237 items currently below reorder point, with several products projected to stock out before supplier replenishment arrives. The highest-risk items showed large shortage gaps and lead-time exposure, indicating a need for urgent action.

I recommend prioritizing replenishment for the most critical shortages, reviewing reorder point settings in the most affected categories, and increasing safety stock for items where supplier lead times exceed expected stock coverage.

Please let me know if you would like a category-by-category breakdown or a recommended priority list for replenishment actions.

Best regards,  
[Bahama Usa]

## Skills Demonstrated
- SQL querying and data analysis in PostgreSQL
- Inventory risk assessment
- Lead-time and stockout exposure analysis
- Business-focused metric calculation
- Category-level aggregation and risk identification
- Translation of operational data into business recommendations
- Stakeholder-style communication of findings

## How to Run This Project
1. Load the simulated inventory dataset into PostgreSQL
2. Create the `warehouse_inventory` table
3. Run the SQL scripts in the `sql/` folder in numerical order
4. Review the screenshots and findings in the README

## Final Recommendations
Based on the analysis, the following actions are recommended:

1. **Prioritize urgent replenishment**
   - Focus first on items with the largest shortage amounts and lowest days until stockout.

2. **Increase safety stock for critical materials**
   - Items where supplier lead time exceeds expected days until stockout should carry additional buffer stock.

3. **Review reorder point logic**
   - Reassess reorder thresholds for categories with frequent risk exposure to ensure they reflect actual demand patterns.

4. **Monitor high-risk categories more closely**
   - Categories with a high concentration of at-risk items may require tighter planning and more frequent review.

5. **Engage suppliers on lead-time performance**
   - For items with persistent risk gaps, supplier lead times should be reviewed to reduce replenishment delays.
