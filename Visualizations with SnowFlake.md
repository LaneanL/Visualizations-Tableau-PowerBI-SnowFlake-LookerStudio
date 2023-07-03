
# RareBurger Market Analysis
RareBurger is a fictional chain restuarant seeking to understand and ultimately increase its market share in the U.S market. 
Load Daily Sales into a SlowFlake Table and Update the Client's RareBurger Dashboard

### Task 1:
#### 1. Analyze the market shares per region compared to their main competitors and build a dashboard that will be used by the management.
*  Create tables MONTHLY_REVENUE from RareBurger's montly revenue data  and SPEND_CROSS_SHOPPING_SAMPLE SnowFlakes Marketplace from SafeGraph
Data contains consumer spening insights for restaurants in New York City.
* The table includes Location Name, Address, Category, Total Spend, Number of Transactons,
 Number of unique customers, median spend  per customer, related physical brand, related online merchants, related delievery service, related payment platform.
      
#### 2. Create a new table called DAILY-SALES which contains 4 columns- Brand, Region, Date, and Sales.
* Load DAILY_SALES.csv data into the DAILY_SALES Table and uodate the dashboard to reflect daily sales by region.

##### Datasets:  
MONTHLY_REVENUE.csv  
DAILY_SALES_by_Region.csv

#### Create Database
> USE ROLE ACCOUNTADMIN;
> CREATE DATABASE IF NOT EXISTS THERAREBURGER;  
> USE DATABASE THERAREBURGER;  
> USE SCHEMA PUBLIC;

#### Create Data Table THERAREBURGER>PUBLIC>Tables  MonthlyRevenue
> CREATE OR REPLACE TABLE MONTHLY_REVENUE (  
> "LOCATION_NAME" varchar(50),  
>  "REGION" varchar2(50),  
> "POSTAL_CODE" varchar2(255),  
>  "STREET_ADDRESS" varchar2(255),  
>  "RAW_TOTAL_SPEND" FLOAT,  
>  "RAW_NUM_TRANSACTIONS" number,  
>  "RAW_NUM_CUSTOMERS" number,  
>  "ONLINE_SPEND" FLOAT,  
> "MEDIAN_SPEND_PER_TRANSACTION" FLOAT,  
> "MEDIAN_SPEND_PER_CUSTOMER" FLOAT  
>);

##### Create Virtual Warehouse to load date
THERAREBURGER_WH (size Small)  
![image](https://github.com/LaneanL/Visualizations-Using-Tableau-Poweer-BI-SnowFlake-Looker-Studio/assets/132226337/795bae20-2382-4181-aef4-2ebc9d3678a5)

### SQL QUERY
#### Combine both tables / Filter Marketplace data of New York resturants containing  category matching pattern '%Burger%'
> SELECT LOCATION_NAME, REGION, POSTAL_CODE, STREET_ADDRESS, RAW_TOTAL_SPEND  
> FROM THERAREBURGER.PUBLIC.MONTHLY_REVENUE   
> UNION   
> SELECT LOCATION_NAME, REGION, POSTAL_CODE, STREET_ADDRESS, RAW_TOTAL_SPEND  
> FROM FREE_SAMPLE_CROSS_SHOPPING_INSIGHTS__NYC_RESTAURANTS.PUBLIC.SPEND_CROSS_SHOPPING_SAMPLE  
> WHERE CATEGORY_TAGS LIKE '%Burger%';  

Customer Spend By Locations versus Location  
![image](https://github.com/LaneanL/Visualizations-Using-Tableau-Poweer-BI-SnowFlake-Looker-Studio/assets/132226337/67f7481d-7a78-4c48-87e9-9b66dadfc042) \
![image](https://github.com/LaneanL/Visualizations-Using-Tableau-Poweer-BI-SnowFlake-Looker-Studio/assets/132226337/3e393cd2-d191-46e7-9f4c-b174fe4bf6b2)

Number of Customers  By Location versus Competition
![image](https://github.com/LaneanL/Visualizations-Using-Tableau-Poweer-BI-SnowFlake-Looker-Studio/assets/132226337/06a2472d-a8cb-45a4-ae2f-72a18989f67a) \
\
Online Spend By Location  versus Competition\
![image](https://github.com/LaneanL/Visualizations-Using-Tableau-Poweer-BI-SnowFlake-Looker-Studio/assets/132226337/95943a50-6209-47b0-bd8a-d6c32d16b131)

SPECIFIC AREA CODES for heat grid
> SELECT LOCATION_NAME, REGION, POSTAL_CODE, STREET_ADDRESS, MEDIAN_SPEND_PER_TRANSACTION
> FROM THERAREBURGER.PUBLIC.MONTHLY_REVENUE
> WHERE POSTAL_CODE in ('10036','10019','10003')
> UNION 
> SELECT LOCATION_NAME, REGION, POSTAL_CODE, STREET_ADDRESS, MEDIAN_SPEND_PER_TRANSACTION
> FROM FREE_SAMPLE_CROSS_SHOPPING_INSIGHTS__NYC_RESTAURANTS.PUBLIC.SPEND_CROSS_SHOPPING_SAMPLE
> WHERE CATEGORY_TAGS LIKE '%Burger%' AND POSTAL_CODE in ('10036','10019','10003');

![image](https://github.com/LaneanL/Visualizations-Using-Tableau-Poweer-BI-SnowFlake-Looker-Studio/assets/132226337/ebef392b-1985-4e6a-a6e3-951468f8ca02)
![image](https://github.com/LaneanL/Visualizations-Using-Tableau-Poweer-BI-SnowFlake-Looker-Studio/assets/132226337/6246436d-66e2-464b-b4b4-cba4f512420b)

### Task 2
#### Create a new table called DAILY-SALES 
> SELECT *
> FROM THERAREBURGER.PUBLIC.DAILY_SALES;
> 
![image](https://github.com/LaneanL/Visualizations-Using-Tableau-Poweer-BI-SnowFlake-Looker-Studio/assets/132226337/42ab2630-fce9-4d35-9288-01447fc4002d)
![image](https://github.com/LaneanL/Visualizations-Using-Tableau-Poweer-BI-SnowFlake-Looker-Studio/assets/132226337/15c420ef-e01b-4d32-8f14-041cfe5b325a)


## Dashboard

![image](https://github.com/LaneanL/Visualizations-Using-Tableau-Poweer-BI-SnowFlake-Looker-Studio/assets/132226337/a7b27b43-0aea-4945-9b89-b5b2ace68437)
--------------------------------------------------------------------------------------------------------------------------------------------------
Interpretation by Location
10036 zipcode:
Hard Rock Customer Spend by Location ($24575), Perfect Pint-$13241, other-$10256, and RareBurger- $9433 for zipcode 10036.  
RareBurger has the highest  Number of  Customers by Location - 1218,  Hard Rock- 329, Perfect Pint 175, and other- 93.  
Rareburger has the least Online Spend for zipcode 10036- $244,  West Bank- $1398, and other- $619.  
Heat grid: Median Spend per Transaction for Hard Rock- $60 versus Rareburger- $111.90  



10003 zipcode:  
Mark's Customer by Location- $20559, RareBurger- $3700, and other- $3896
Number of Customers by Location for RareBurger- 1105, Mark's- 549, East Village- 104, other- 43
Least Online Spend for RareBurger- $117 and other-$759
Heat grid: Median Spend per Tranaction for Mark's- $13 and RareBurger- $198.12 Rareburger  

10019 zipcode:
Rareburger's Customer Spend by Location- $16824, Juniors- $12785, and other -12305 
Rareburger's Customer by Location- 1440, Junior's- 229, and other- 186
Least Online Spend for RareBurger- $301 and other- $653
Heat grid: Median Spend per Transaction for Juniors- $75.72 and RareBurger- $392.24

## Summary
Rareburger has the highest number of customers for all three zip code (10003, 10019, and 10036). However, Rareburger does not capture the highest number of Customers for location 10003 and 10036.
*RareBurger should further promote their online presence to gain more customers by location.
