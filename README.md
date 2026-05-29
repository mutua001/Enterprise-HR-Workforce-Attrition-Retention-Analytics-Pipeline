
# HR Workforce Attrition Analytics

![Python](https://img.shields.io/badge/Python-Data%20Analytics-blue?style=for-the-badge&logo=python)
![SQL](https://img.shields.io/badge/SQL-Database-orange?style=for-the-badge&logo=mysql)
![PowerBI](https://img.shields.io/badge/PowerBI-Dashboard-yellow?style=for-the-badge&logo=powerbi)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Cleaning-black?style=for-the-badge&logo=pandas)

---

# Project Overview

This project presents a complete **HR Workforce Attrition Analytics Pipeline** built using **Python, SQL, Excel, and Power BI**. The analysis identifies key factors driving employee turnover including compensation, overtime, job roles, age demographics, and job satisfaction.

The goal is to help organizations transition from reactive HR reporting to proactive, data-driven retention strategies.

---

# Dashboard Preview

![HR Dashboard](https://github.com/mutua001/Enterprise-HR-Workforce-Attrition-Retention-Analytics-Pipeline/blob/main/Overview.png)

---

# Business Problem

Employee attrition increases:
- Recruitment costs
- Training expenses
- Productivity loss
- Institutional knowledge gaps

This project helps HR teams:
- Predict attrition trends
- Understand workforce patterns
- Improve retention strategies
- Optimize workforce planning

---

# Objectives

- Analyze employee attrition trends
- Identify high-risk departments and job roles
- Examine overtime impact on turnover
- Evaluate salary and satisfaction relationships
- Build an interactive HR analytics dashboard

---

# Tech Stack

| Tool | Purpose |
|---|---|
| Python | Data Cleaning & Processing |
| Pandas | Data Manipulation |
| SQL | Data Storage & Analysis |
| Excel | Source Data |
| Power BI | Dashboard Visualization |
| Jupyter Notebook | Analysis Environment |

---


---

# Step 1: Import Libraries

```python
import pandas as pd
import numpy as np
from sqlalchemy import create_engine
```

---

# Step 2: Load Data from CSV and Excel

## Load CSV File

```python
employees_csv = pd.read_csv('data/raw/employees.csv')
```

---

## Load Excel File

```python
employees_xlsx = pd.read_excel('data/raw/employees.xlsx')
```

---

## Preview Data

```python
print(employees_csv.head())
print(employees_xlsx.head())
```

---

# Step 3: Combine Datasets

```python
combined_df = pd.concat(
    [employees_csv, employees_xlsx],
    ignore_index=True
)
```

---

## Check Shape

```python
print(combined_df.shape)
```

---

## Save Combined Dataset

```python
combined_df.to_csv(
    'data/processed/combined_hr_data.csv',
    index=False
)
```

---

# Step 4: Data Cleaning Using Python

# Check Missing Values

```python
print(combined_df.isnull().sum())
```

---

# Remove Duplicate Rows

## Count Duplicates

```python
print(combined_df.duplicated().sum())
```

---

## Drop Duplicates

```python
combined_df = combined_df.drop_duplicates()
```

---

## Verify Removal

```python
print(combined_df.duplicated().sum())
```

---

# Standardize Column Names

```python
combined_df.columns = (
    combined_df.columns
    .str.lower()
    .str.replace(' ', '_')
)
```

---

# Fill Missing Values

## Numeric Columns

```python
combined_df['monthly_income'] = combined_df[
    'monthly_income'
].fillna(
    combined_df['monthly_income'].median()
)
```

---

## Categorical Columns

```python
combined_df['department'] = combined_df[
    'department'
].fillna('Unknown')
```

---

# Check Data Types

```python
print(combined_df.dtypes)
```

---

# Convert Data Types

```python
combined_df['age'] = combined_df['age'].astype(int)
```

---

# Save Cleaned Dataset

```python
combined_df.to_csv(
    'data/cleaned/hr_cleaned.csv',
    index=False
)
```

---

# Step 5: Load Data into SQL Database

## Create Database Connection

```python
engine = create_engine(
    'sqlite:///hr_attrition.db'
)
```

---

## Export Dataset to SQL

```python
combined_df.to_sql(
    'employees',
    con=engine,
    if_exists='replace',
    index=False
)
```

---

# SQL Schema

```sql
CREATE TABLE employees (
    employee_id INT,
    age INT,
    department VARCHAR(50),
    job_role VARCHAR(100),
    monthly_income FLOAT,
    overtime VARCHAR(10),
    attrition VARCHAR(10)
);
```

---

# SQL Data Cleaning

## Remove Duplicates

```sql
DELETE FROM employees
WHERE rowid NOT IN (
    SELECT MIN(rowid)
    FROM employees
    GROUP BY employee_id
);
```

---

## Check Missing Values

```sql
SELECT *
FROM employees
WHERE department IS NULL;
```

---

# SQL Analysis Queries

# Attrition Rate

```sql
SELECT
    attrition,
    COUNT(*) AS total
FROM employees
GROUP BY attrition;
```

---

# Average Salary by Department

```sql
SELECT
    department,
    AVG(monthly_income) AS avg_salary
FROM employees
GROUP BY department;
```

---

# Overtime vs Attrition

```sql
SELECT
    overtime,
    attrition,
    COUNT(*) AS total
FROM employees
GROUP BY overtime, attrition;
```

---

# Job Satisfaction Analysis

```sql
SELECT
    job_satisfaction,
    COUNT(*) AS employees
FROM employees
GROUP BY job_satisfaction;
```

---

# Key Performance Indicators (KPIs)

| KPI | Description |
|---|---|
| Total Attrition | Number of employees who left |
| Attrition Rate | Percentage turnover rate |
| Current Staff | Active employee count |
| Staff Satisfaction | Average satisfaction score |
| Overtime Impact | Attrition due to overtime |
| Salary Distribution | Income band analysis |
| Department Attrition | Turnover by department |
| Role Attrition | Turnover by job role |

---

# Power BI Dashboard

## Dashboard Features

- Attrition Overview
- Employee Demographics
- Overtime Analysis
- Income Analysis
- Satisfaction Heatmaps
- Department Trends
- Monthly Attrition Trends

---

# Power BI Workflow

```text
Raw Data
   ↓
Python Cleaning
   ↓
Duplicate Removal
   ↓
SQL Database
   ↓
SQL Analysis
   ↓
Power BI Dashboard
   ↓
Business Insights
```

---

# Key Insights

- Employees working overtime showed significantly higher attrition.
- Lower salary bands experienced higher turnover rates.
- Sales departments recorded the highest attrition.
- Job satisfaction strongly influenced retention.
- Younger employees had increased turnover probability.

---

# Sample Visualizations

## Recommended Charts

| Visualization | Purpose |
|---|---|
| Bar Chart | Department Attrition |
| Donut Chart | Gender Distribution |
| Heatmap | Satisfaction Scores |
| Column Chart | Monthly Trends |
| Pie Chart | Overtime Impact |

---



# Skills Demonstrated

```text
Python
SQL
Pandas
Power BI
Data Cleaning
Data Visualization
Business Intelligence
HR Analytics
Exploratory Data Analysis
Dashboard Design
```

---
---


# Key Performance Indicators (KPIs)

| KPI | Value |
|---|---|
| Total Attrition | 237 |
| Attrition Rate | 16.1% |
| Current Staff | 1,233 |
| Staff Satisfaction Score | 2.8 / 4.0 |
| Average Monthly Attrition | 18 |
| Sales Department Attrition | 20.6% |
| HR Department Attrition | 19.0% |
| R&D Department Attrition | 13.8% |

---

# Demographics Analysis

## Age Group Attrition

| Age Group | Attrition % |
|---|---|
| 18-24 | 39.2% |
| 25-34 | 20.2% |
| 35-44 | 10.1% |
| 45-54 | 10.2% |
| 55+ | 15.9% |

---

## Gender Distribution

| Gender | Attrition % |
|---|---|
| Male | 17.0% |
| Female | 14.8% |

---

## Marital Status

| Marital Status | Attrition % |
|---|---|
| Single | 25.5% |
| Married | 12.5% |
| Divorced | 10.1% |

---

# Working Conditions

## Overtime Analysis

| Overtime | Attrition % |
|---|---|
| Yes | 30.5% |
| No | 10.4% |

---

## Training Participation

| Training | Attrition % |
|---|---|
| Yes | 15.7% |
| No | 27.8% |

---

## Business Travel Frequency

| Travel Frequency | Attrition % |
|---|---|
| Frequent | 24.9% |
| Rare | 15.0% |
| None | 8.0% |

---

# Job Level Analysis

| Job Level | Attrition % |
|---|---|
| 1st Level | 26.3% |
| 2nd Level | 9.7% |
| 3rd Level | 14.7% |
| 4th Level | 4.7% |
| 5th Level | 7.2% |

---

# Income Level Analysis

## All Employees

| Monthly Income | Attrition % |
|---|---|
| < $2.5K | 34.4% |
| $2.5K - $5K | 16.4% |
| $5K - $7.5K | 9.7% |
| $7.5K - $10K | 14.6% |
| $10K+ | 8.9% |

---

## ESO Holders Only

| Monthly Income | Attrition % |
|---|---|
| < $2.5K | 25.0% |
| $2.5K - $5K | 9.4% |
| $5K - $7.5K | 4.8% |
| $7.5K - $10K | 9.7% |
| $10K+ | 6.2% |

---

## Without ESO

| Monthly Income | Attrition % |
|---|---|
| < $2.5K | 44.4% |
| $2.5K - $5K | 24.8% |
| $5K - $7.5K | 17.1% |
| $7.5K - $10K | 20.7% |
| $10K+ | 13.5% |

---

# Top Roles with Highest Attrition

| Job Role | Department | Attrition % |
|---|---|---|
| Sales Representative | Sales | 39.8% |
| Lab Technician | R&D | 23.9% |
| HR | HR | 23.1% |
| Sales Executive | Sales | 17.5% |
| R&D Scientist | R&D | 16.1% |

---

# Satisfaction Score Analysis

## Environment Satisfaction

| Score | Percentage |
|---|---|
| 1/4 | 25.4% |
| 2/4 | 15.0% |
| 3/4 | 13.7% |
| 4/4 | 13.5% |

---

## Relationship Satisfaction

| Score | Percentage |
|---|---|
| 1/4 | 20.7% |
| 2/4 | 14.9% |
| 3/4 | 15.5% |
| 4/4 | 14.8% |

---

## Job Satisfaction

| Score | Percentage |
|---|---|
| 1/4 | 22.8% |
| 2/4 | 16.4% |
| 3/4 | 16.5% |
| 4/4 | 11.3% |

---

## Job Involvement

| Score | Percentage |
|---|---|
| 1/4 | 33.7% |
| 2/4 | 18.9% |
| 3/4 | 14.4% |
| 4/4 | 9.0% |

---

## Work-Life Balance

| Score | Percentage |
|---|---|
| 1/4 | 31.3% |
| 2/4 | 16.9% |
| 3/4 | 14.2% |
| 4/4 | 17.6% |

---

# Overall Averaged Satisfaction Score

| Score Level | Percentage |
|---|---|
| 1/4 | 40.0% |
| 2/4 | 26.7% |
| 3/4 | 11.9% |
| 4/4 | 4.8% |

---

# Key Insights

- Employees working overtime recorded the highest attrition rate at 30.5%.
- Younger employees aged 18–24 showed the highest turnover at 39.2%.
- Lower salary bands experienced significantly higher attrition.
- Sales Representatives had the highest role-based attrition at 39.8%.
- Employees with poor work-life balance and low job involvement were more likely to leave.
- Single employees showed higher attrition compared to married and divorced employees.

---

## 🚀 Strategic Retention Recommendations

Based on the data analysis, here are the priority areas for investment:

### 1. Close the "Early Career" Gap
* **The Insight:** Employees aged 18–24 have a **39.2%** attrition rate.
* **The Action:** Implement a structured mentorship program and expand the **ESO (Equity Stock Option)** program to entry-level staff, as ESO holders show significantly lower turnover.

### 2. Optimize Working Conditions
* **The Insight:** Employees working overtime are **nearly 3x more likely to leave** (30.5%) than those who do not (10.4%).
* **The Action:** Conduct a workload capacity audit in Sales and R&D to reduce reliance on excessive overtime.

### 3. Invest in Professional Development
* **The Insight:** Employees who do *not* receive training have a **27.8%** attrition rate, compared to 15.7% for those who do.
* **The Action:** Institutionalize recurring professional development cycles for all departments.

### 4. Target High-Risk Roles
* **The Insight:** Sales Representatives have the highest attrition at **39.8%**.
* **The Action:** Create a "Retention Bonus" structure that vests after 18–24 months to incentivize long-term commitment.

---

## 📊 Summary of Data Drivers
| Driver | High Risk Indicator | Attrition % |
| :--- | :--- | :--- |
| **Overtime** | Yes | 30.5% |
| **Age** | 18-24 | 39.2% |
| **Salary** | < $2.5K | 34.4% |
| **Travel** | Frequent | 24.9% |
# Project Tags

```text
#Python
#SQL
#PowerBI
#Pandas
#HRAnalytics
#EmployeeAttrition
#BusinessIntelligence
#DataVisualization
#Dashboard
#Analytics
#EDA
---

# Author

## Peter Mutua

Data Analyst | Business Intelligence Analyst | HR Analytics Enthusiast

#WorkforceAnalytics
```


