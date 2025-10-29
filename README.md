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

### 1.Which products contribute the most to carbon emissions?
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

### 2.What are the industry groups of these products?
Query
```sql
SELECT pe.product_name, industry_group,
ROUND(avg(pe.carbon_footprint_pcf ),2) AS Average_carbon_footprint 
FROM product_emissions pe 
JOIN industry_groups ig 
ON pe.industry_group_id - ig.id 
GROUP BY pe.product_name 
ORDER BY Average_carbon_footprint DESC
LIMIT 10;
```
<b>Resurlt: Top 10 industry groups with products
|product_name|industry_group|Average_carbon_footprint|
|------------|--------------|------------------------|
|Wind Turbine G128 5 Megawats|"Consumer Durables, Household and Personal Products"|3718044.00|
|Wind Turbine G132 5 Megawats|"Consumer Durables, Household and Personal Products"|3276187.00|
|Wind Turbine G114 2 Megawats|"Consumer Durables, Household and Personal Products"|1532608.00|
|Wind Turbine G90 2 Megawats|"Consumer Durables, Household and Personal Products"|1251625.00|
|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|"Consumer Durables, Household and Personal Products"|191687.00|
|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|"Consumer Durables, Household and Personal Products"|167000.00|
|TCDE|"Consumer Durables, Household and Personal Products"|99075.00|
|Mercedes-Benz GLE (GLE 500 4MATIC)|"Consumer Durables, Household and Personal Products"|91000.00|
|Mercedes-Benz S-Class (S 500)|"Consumer Durables, Household and Personal Products"|85000.00|
|Mercedes-Benz SL (SL 350)|"Consumer Durables, Household and Personal Products"|72000.00|

### 3.What are the industries with the highest contribution to carbon emissions?
Query
```sql
SELECT industry_group,
ROUND(SUM(pe.carbon_footprint_pcf ),2) AS Sum_carbon_footprint 
FROM product_emissions pe 
JOIN industry_groups ig 
ON pe.industry_group_id - ig.id 
GROUP BY industry_group
ORDER BY Sum_carbon_footprint  DESC
LIMIT 10;
```
<b>Resurlt: 
|industry_group|Sum_carbon_footprint|
|--------------|--------------------|
|Household & Personal Products|13948951.00|
|Tobacco|13948950.00|
|Semiconductors & Semiconductors Equipment|13948948.00|
|Retailing|13948921.00|
|Semiconductors & Semiconductor Equipment|13948897.00|
|Gas Utilities|13948829.00|
|Food & Beverage Processing|13948810.00|
|Trading Companies & Distributors and Commercial Services & Supplies|13948712.00|
|Utilities|13948707.00|
|"Textiles, Apparel, Footwear and Luxury Goods"|13948564.00|

### 4.What are the companies with the highest contribution to carbon emissions?
Query
```sql
SELECT ig.company_name,
ROUND(SUM(pe.carbon_footprint_pcf), 2) AS Sum_carbon_footprint
FROM product_emissions pe 
JOIN companies ig 
ON pe.company_id = ig.id 
GROUP BY ig.company_name
ORDER BY Sum_carbon_footprint  DESC
LIMIT 10;
```
<b>Resurlt: 
|company_name|Sum_carbon_footprint|
|------------|--------------------|
|"Gamesa Corporación Tecnológica, S.A."|9778464.00|
|Daimler AG|1594300.00|
|Volkswagen AG|655960.00|
|"Mitsubishi Gas Chemical Company, Inc."|212016.00|
|"Hino Motors, Ltd."|191687.00|
|Arcelor Mittal|167007.00|
|Weg S/A|160655.00|
|General Motors Company|137007.00|
|"Lexmark International, Inc."|132012.00|
|"Daikin Industries, Ltd."|105600.00|

### 5.What are the countries with the highest contribution to carbon emissions?
Query
```sql
SELECT ig.country_name,
ROUND(SUM(pe.carbon_footprint_pcf), 2) AS Sum_carbon_footprint
FROM product_emissions pe 
JOIN countries ig 
ON pe.country_id = ig.id 
GROUP BY ig.country_name
ORDER BY Sum_carbon_footprint  DESC
LIMIT 10;
```
<b>Resurlt:
|country_name|Sum_carbon_footprint|
|------------|--------------------|
|Spain|9786130.00|
|Germany|2251225.00|
|Japan|653237.00|
|USA|518381.00|
|South Korea|186965.00|
|Brazil|169337.00|
|Luxembourg|167007.00|
|Netherlands|70417.00|
|Taiwan|62875.00|
|India|24574.00|

### 6.What is the trend of carbon footprints (PCFs) over the years?
Query
```sql
SELECT pe.year,
ROUND(SUM(pe.carbon_footprint_pcf), 2) AS Sum_carbon_footprint,
ROUND(AVG(pe.carbon_footprint_pcf), 2) AS Avg_carbon_footprint
FROM product_emissions pe 
GROUP BY pe.year
ORDER BY year
```
<b>Resurlt:
|year|Sum_carbon_footprint|Avg_carbon_footprint|
|----|--------------------|--------------------|
|2013|503857.00|2399.32|
|2014|624226.00|2457.58|
|2015|10840415.00|43188.90|
|2016|1640182.00|6891.52|
|2017|340271.00|4050.85|




