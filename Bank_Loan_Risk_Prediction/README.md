# **Bank Loan Risk Prediction**
# by Nabeel Ghalib


## Objective:
 
The main objective of this project was to design a database, perform data analysis, and build a predictive model to identify borrowers who are at risk of defaulting on their loans. 
The specific goals were:
1. Clean and preprocess the data.
2. Conduct descriptive analysis to uncover key patterns.
3. Build and evaluate a predictive model with high accuracy.


## Key Stakeholders:

Owner and Designer of LIBAAZ - The Couture.


## Data used:

The data for this project was provided to me by the swapnajeet.


## Tools Used:

- **Jira**: Project management
- **MySQL:** Data preparation and Descriptive analysis
- **Jupyter Notebook:** Predictive Analysis (Model building, Evaluating and Tuning)


## Key Tasks and Approach:

### 1. Project Planning (Kanban Methodology):

Kanban workflow: Managed tasks using a Kanban board to track progress across phases:
- To Do: Data cleaning, analysis, feature engineering, and model building.
- In Progress: Current tasks prioritized by deadlines.
- Done: Completed milestones for project monitoring.
Set deadlines for each phase to ensure smooth execution and timely delivery.

### 2. Data Cleaning & Transformation (MySQL):

Identified and Handled Missing Values:
- The emp_title column had empty spaces (not NULL values) and whitespace characters.
- Used the TRIM function to remove unnecessary whitespace from the emp_title values.
- Converted empty strings and spaces into NULL values to ensure consistency.
- Replaced the NULL values in emp_title with the string 'Unknown' to handle missing data appropriately.
- Converted the columns with dates stored as text into date data type.

```sql

-- counting null, empty strings and white spaces --

SELECT 
    COUNT(*) AS total_rows,
    SUM(CASE WHEN emp_title IS NULL THEN 1 ELSE 0 END) AS null_values,
    SUM(CASE WHEN emp_title = '' THEN 1 ELSE 0 END) AS empty_strings,
    SUM(CASE WHEN TRIM(emp_title) = '' THEN 1 ELSE 0 END) AS whitespace_only
FROM loan_data;
```
- There were no null values just empty spaces and whitespaces.

```sql

-- updating the empty spaces with null and then to unknown --

UPDATE loan_data
SET emp_title = NULL
WHERE emp_title = '' OR TRIM(emp_title) = '';

update loan_data
Set emp_title = 'Unknown'
where emp_title is null;

```
- Converted the null to unknown instead leaving it as null because it was appropriate for the upcoming model building.

- Converting text values in date

```sql

UPDATE loan_data
SET issue_date = STR_TO_DATE(issue_date, '%d-%m-%Y')
WHERE issue_date IS NOT NULL;

ALTER TABLE loan_data
MODIFY issue_date DATE;

```

### 3.Descriptive Analysis (MySQL):

In this project, I performed a comprehensive Descriptive Analysis of loan data, aiming to extract meaningful insights to support decision-making and performance tracking in the loan business. The analysis covers key performance indicators (KPIs) such as loan applications, funded amounts, received payments, and loan status.

Here are few Key insights:

Loan Applications & Trends:

Insight: The total number of loan applications has steadily increased over time, indicating growing demand for loans.
Actionable Insight: This upward trend suggests that the bank can plan for higher volumes of loan applications, adjusting resources and processing systems accordingly.

```sql

select month(issue_date) as month, count(id) as total_applications
from loan_data
group by month
order by month;

```
![Loan Applications Trend](loan_applications_trend.png)

Loan Status Distribution:


``` sql

SELECT 
    loan_status,
    COUNT(id) AS total_applications,
    (COUNT(id) * 100.0 / (SELECT COUNT(id) FROM loan_data)) AS percentage
FROM loan_data
GROUP BY loan_status;

```
![Loan Status Distribution](loan_status_distribution.png)






