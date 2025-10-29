# Carbon-emission-analyst
This report aims to analyze carbon emissions to examine the carbon footprint across various industries.
## Overview
<img width="640" height="427" alt="image" src="https://github.com/user-attachments/assets/0993571c-7a3e-4daf-a695-966af35f0684" />\
<b>What is the project about?<b>\
We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends
Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.

## Data structal
###Table 'product_emissions'
```sql
SELECT * FROM product_emissions pe  
LITMIT 5;
```
Resurt



### Table Industry_group
```sql
SELECT * FROM companies
LIMIT 5;
```
Resurt
|id|company_name|
|--|------------|
|1|"Autodesk, Inc."|
|2|"Casio Computer Co., Ltd."|
|3|"Cisco Systems, Inc."|
|4|"CNX Coal Resources, LP"|
|5|"Coca-Cola Enterprises, Inc."|

## Detail Analysts
### Check duplicate for Products
Query
```sql
SELECT COUNT(product_name) AS 'Total number of products'
       COUNT(DISTINCT (product_name)) AS 'Number of distinct products'
FROM product_emissions pe 
```
Resurt
|Total number of products|Number of distinct products|
|------------------------|---------------------------|
|1037|661|
There are a lot of duplicate rows for product, it's needed to group by product when calculating  carbon emissions

### 1
Query
```sql
SELECT pe.product_name,
ROUND(AVG (carbon_footprint_pcf),2) AS 'Average_carbon_footprint'
FROM product_emissions pe;
GROUP BY pe.product_name 
ORDER BY Average_carbon_footprint DESC
LIMIT 10;
```
<b>Resurlt: Top 10 products with highest countribution to Carbon Emissions <b>
|product_name|Average_carbon_footprint|
|------------|------------------------|
|Wind Turbine G128 5 Megawats|3718044.00|
|Wind Turbine G132 5 Megawats|3276187.00|
|Wind Turbine G114 2 Megawats|1532608.00|
|Wind Turbine G90 2 Megawats|1251625.00|
|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|191687.00|
|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|167000.00|
|TCDE|99075.00|
|Mercedes-Benz GLE (GLE 500 4MATIC)|91000.00|
|Mercedes-Benz S-Class (S 500)|85000.00|
|Mercedes-Benz SL (SL 350)|72000.00|
