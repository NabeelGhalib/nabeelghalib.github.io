# **Bank Loan Risk Prediction**
# by Nabeel Ghalib


## Objective:
 
The main objective of this project was to design a database, perform data analysis, and build a predictive model to identify borrowers who are at risk of defaulting on their loans. 
The specific goals were:
1. Clean and preprocess the data.
2. Conduct descriptive analysis to uncover key patterns.
3. Build and evaluate a predictive model with high accuracy.

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

- Converting text values in date columns.

```sql

UPDATE loan_data
SET issue_date = STR_TO_DATE(issue_date, '%d-%m-%Y')
WHERE issue_date IS NOT NULL;

ALTER TABLE loan_data
MODIFY issue_date DATE;

```

### 3. Descriptive Analysis (MySQL):

In this project, I performed a comprehensive Descriptive Analysis of loan data, aiming to extract meaningful insights to support decision-making and performance tracking in the loan business. The analysis covers key performance indicators (KPIs) such as loan applications, funded amounts, received payments, and loan status.

Here are few Key insights:

**Loan Applications & Trends:**

```sql

select month(issue_date) as month_no,
monthname(issue_date) as month,
count(id) as total_applications
from loan_data
group by month
order by month;

```

![Loan Applications Trend](loan_app_trend.png)


![Loan Applications Trend](loan_applications_trend.png)



**Insight:** 
- The total number of loan applications has steadily increased over time, indicating growing demand for loans.

**Actionable Insight:** 
- This upward trend suggests that the bank can plan for higher volumes of loan applications, adjusting resources and processing systems accordingly.





**Loan Status Distribution:**

``` sql

SELECT 
    loan_status,
    COUNT(id) AS total_applications,
    (COUNT(id) * 100.0 / (SELECT COUNT(id) FROM loan_data)) AS percentage
FROM loan_data
GROUP BY loan_status;

```


![Loan Status Distribution](loan_dist_sql.png)


![Loan Status Distribution](loan_status_distribution.png)


**Insights:** 
- The majority of loans (83.3%) have been fully paid, indicating that most borrowers are able to repay their loans successfully.
- A relatively smaller portion (13.8%) of loans has been charged off, indicating some level of loan defaults.
- Only 2.8% of loans are currently active and unpaid, suggesting effective loan management.

**Actionable Insights:**
- This high repayment rate suggests that the bank can continue offering loans with confidence. However, it may consider implementing targeted marketing strategies to further incentivize borrowers and maintain this trend.
- The bank should focus on enhancing its risk management strategies, such as improving credit assessments and collection processes, to reduce charge-offs and mitigate potential losses.
- The bank could maintain its current operational processes and focus on maintaining low levels of unpaid loans, while exploring ways to further streamline collections or recovery on these loans.




**Loan Amount Funded by Purpose:**


![Loan Amount Funded by Purpose](loan_amt_purpose.png)


**Insights:**

**Top 5 Loan Categories:**

- **Debt Consolidation (232.5M):** The largest category, reflecting efforts to consolidate debt for simpler repayment terms or lower interest rates.
- **Credit Card (58.9M):** A significant portion of loans is directed towards credit card debt, indicating people are managing high-interest balances.
- **Home Improvement (33.4M):** Strong investment in home renovations, likely driven by rising home values or market trends.
- **Other (31.2M):** Miscellaneous loans for unspecified purposes, representing a range of financial needs.
- **Small Business (24.1M):** Loans for small businesses, likely indicating growth or support during recovery from economic challenges.

**Bottom 3 Loan Categories:**

- **Renewable Energy (0.8M):** Low investment, suggesting potential for growth in renewable energy loans given the global push for sustainability.
- **Vacation (2.0M):** Low demand for vacation loans, likely due to travel restrictions or reduced discretionary spending.
- **Educational (2.2M):** Limited amount for educational loans, possibly due to alternative funding options or rising education costs discouraging borrowing.




**4. Predictive Modeling:**

**Data Preprocessing:**
- Converted the categorical variables into numerics by one hot encoding, label encoding.
- Since using Random Forest and XGB, normalizing numerical variables is not neccessary.

**Model Selection:**
- Random Forest and XGBoost models were selected due to their effectiveness in handling tabular data and their ability to provide high accuracy.

**Model Training:** 
- Both models were trained using the preprocessed data, with hyperparameters tuned to optimize performance.

**Feauture Selection:**
- Selected important features using the important feature plot of XGB to select the most important features.

**Evaluation:** 
- The models were evaluated using accuracy as the primary metric, and the final XGB model achieved a 97% accuracy score in predicting loan risks.

## 5. Conclusion:
This project successfully applied machine learning techniques to predict the risk of loan defaults, achieving an accuracy of 97%. The findings suggest that Random Forest and XGBoost are effective models for this type of prediction. Future work could focus on enhancing the model with more data and exploring other advanced algorithms to further improve the accuracy.








