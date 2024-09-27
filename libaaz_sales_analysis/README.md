# **LIBAAZ - The Couture - Sales Analysis**
# by Nabeel Ghalib


## Objective:

To analyze and provide detailed insights into the sales data for LIBAAZ - The Couture, a bespoke apparel studio specializing in custom-made dresses for women and girls, covering the years 2013-2024.


## Key Stakeholders:

Owner and Designer of LIBAAZ - The Couture.


## Data used:

The data for this analysis was provided to me by the stakeholder.


## Tools Used:

- **Excel:** Data entry and cleaning
- **MySQL:** Data preparation and analysis
- **Tableau:** Data visualization
- **Powerpoint:** Reporting


## Key Tasks and Approach:

### 1.Data Entry & Cleaning (Excel):

- Received raw sales data from the stakeholder, which was handwritten in pencil and pen. Transcribed this data manually into Excel.
- Formatted the data by creating a structured table, ensuring data consistency and standardization.
- Applied **data validation** to automate the entry for fields such as sales channels (e.g., Online, Offline) and referral sources (e.g., Instagram, Facebook), improving accuracy and efficiency.
- Created unique Customer IDs using the **TEXT and MATCH** functions to ensure no duplicates and a uniform format for identifying each customer.
```

="cust"&TEXT(MATCH(R2,UNIQUE($R$2:$R$76),0),"000")

```
- Automated calculations for fields like profit, discounted price, total cost and more, using formulas in Excel.
- Created a **form** in Excel to streamline data entry, ensuring consistent and accurate input for each sale.
- Consistently maintained proper formatting across dates, dress types, and other fields to ensure data was easy to analyze.

### 2.Data Preparation & Transformation (MySQL):

- Converted the cleaned Excel file to CSV and imported it into MySQL, creating a database and a table for the sales data.
- Used SQL commands to create and alter tables, ensuring the structure fit the analysis needs.

```sql

CREATE TABLE sales_copy AS
SELECT * FROM sales;

```
- Addressed the issue of order dates stored as strings by converting them into date formats. Then, I extracted year, month, and day from the order date into separate columns to enable time-based analysis.

```sql

-- creating new column and its data type --

ALTER TABLE sales_copy
ADD COLUMN formatted_order_date DATE;

-- converting the columns data type and copying it and paste the values the newly created columns --

UPDATE sales_copy
SET formatted_order_date = STR_TO_DATE(order_date, '%Y-%m-%d');

-- creating columns for year, month and day --

ALTER TABLE sales_copy
ADD COLUMN order_year INT,
ADD COLUMN order_month INT,
ADD COLUMN order_day INT;

-- extracting year, month and day from order date --

UPDATE sales_copy
SET 
    order_year = YEAR(order_date),
    order_month = MONTH(order_date),
    order_day = DAY(order_date);

```

- Checked the data for errors and inconsistencies post-import and ensured it was ready for deeper analysis.

 
### 3.Sales Data Analysis (MySQL):

- Analyzed sales across different time periods: 2013-2024, 2013-2017, and 2022-2024, providing insights on:
- **Total Orders:** Overall sales volume in each time period.

```sql

-- 2013 - 2024 --

select count(*) as total_sales from sales_copy;

-- 2013 - 2017 --

select count(*) as total_sales from sales_copy
where order_year between 2013 and 2017;

-- 2022 - 2024 --

select count(*) as total_sales from sales_copy
where order_year between 2022 and 2024;

```

- **Top-Selling Products:** Identified the top products by quantity sold and profit.


```sql

-- Highest Sold product by quantity --

select dress_type, count(dress_type) as total from sales_copy 
group by dress_type
order by total desc
limit 5 ;

-- Highest sold product by Profit --

select dress_type, sum(profit) as total from sales_copy 
group by dress_type
order by total desc
limit 5;

```
- **Revenue, Profit, and Costs:** Conducted a detailed analysis of the financial metrics, including total revenue, total profit, total costs, and income.

```sql

select sum(profit) as total_profit, sum(total_cost) as total_cost, sum(discounted_price) as total_revenue, sum(income) as total_income
from sales_copy;

```
- **Customer Segmentation:** Analyzed top customers by sales volume and profit, segmented by age groups and location.

```sql

-- Age Group --

SELECT
    SUM(CASE WHEN age <= 12 THEN quantity ELSE 0 END) AS sales_age_0_12,
    SUM(CASE WHEN age between 13 and 19 THEN quantity ELSE 0 END) AS sales_age_13_19,
    SUM(CASE WHEN age between 20 AND 29 THEN quantity ELSE 0 END) AS sales_age_20_29,
    SUM(CASE WHEN age between 30 AND 39 THEN quantity ELSE 0 END) AS sales_age_30_39,
    SUM(CASE WHEN age between 40 AND 49 THEN quantity ELSE 0 END) AS sales_age_40_49,
    SUM(CASE WHEN age between 50 AND 60 THEN quantity ELSE 0 END) AS sales_age_50_60
FROM sales_copy;

```

- **Sales Channels:** Broke down sales by online vs. offline channels.
- **Referral Sources:** Evaluated the most successful marketing platforms such as Instagram, and Facebook.
- **Time-Based Analysis:** Yearly and monthly sales trends across different periods, identifying peak sales times.

```sql

select order_month, count(order_month) as total_sales from sales_copy
group by order_month
order by total_sales desc;

```


### 4.Visualization & Dashboards (Tableau):

- Imported the processed data from Excel to Tableau and created interactive dashboards to visualize key performance indicators (KPIs), including:
    - Total Sales, Top Products, and Top Customers.
    - Sales by Channel, Referral Source, and Age Group.
    - Yearly and Monthly Sales Trends across different periods.
- Created a calculated field to segment customer ages into groups for better analysis:
```
IF [age] <= 12 THEN "12 and below"
ELSEIF [age] >= 13 AND [age] <= 19 THEN "13-19"
ELSEIF [age] >= 20 AND [age] <= 29 THEN "20-29"
ELSEIF [age] >= 30 AND [age] <= 39 THEN "30-39"
ELSEIF [age] >= 40 AND [age] <= 49 THEN "40-49"
ELSEIF [age] >= 50 AND [age] <= 60 THEN "50-60"
ELSE "Above 60" 
END

```
- Utilized this calculated field to analyze sales data by age categories, enabling deeper insights into customer demographics.
- Enabled action filters in Tableau, allowing users to interactively explore the data and dynamically drill down into specific time periods (2013-2017, 2022-2024) to view corresponding metrics.

Tableau Dashboard - [Dashboard](https://github.com/NabeelGhalib/nabeelghalib.github.io/blob/main/libaaz_sales_analysis/Dashboard%201%20(2)%20(2).png)


### 5.Final Report (PowerPoint):

- Consolidated all findings and visualizations into a PowerPoint report.
- The report included all the answers to the key business questions asked by the stakeholder, along with detailed charts and tables created in Tableau.
- Presented the report to the stakeholder, ensuring it was actionable and easy to understand.

Report - [LIBAAZ - The Couture Report](https://github.com/NabeelGhalib/nabeelghalib.github.io/blob/main/libaaz_sales_analysis/LIBAAZ-Report.pdf)
  

## Conclusion:

- Delivered detailed insights on sales performance, customer behavior, and product trends across multiple time periods.
- Provided LIBAAZ - The Couture with actionable recommendations based on data analysis, helping the owner make informed business decisions regarding product offerings, sales channels, and marketing efforts.
- Demonstrated proficiency in data cleaning, SQL queries, dashboard creation, and presentation skills.

## Contact Information
To explore more about the brand and its offerings, you can follow the owner on Instagram: [libaaz_thecouture](https://www.instagram.com/libaaz_thecouture?igsh=MzkwaGk4NDUwYWp1).


