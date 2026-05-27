# Telecom Churn Analysis: SQL Backend & Power BI Dashboard

This project follows a complete data analytics workflow using MySQL and Power BI. I used SQL to inspect the dataset, check for data quality issues, and explore churn patterns before connecting the database to Power BI for visualization and reporting.

The final dashboard is divided into two pages to separate operational insights from financial risk analysis.

---

## 🏗️ Project Workflow

### 1. Data Preparation & SQL Analysis
I first stored the telecom dataset in MySQL and used SQL queries to:
- Check for duplicates
- Identify missing or blank values
- Validate customer records
- Explore churn related trends

This step helped ensure the dataset was clean before building the dashboard.

### 2. Data Transformation (Power Query)
After connecting MySQL to Power BI, I used Power Query to:
- Fix data types
- Handle null or inconsistent values
- Prepare the dataset for reporting and visualization

### 3. Dashboard Development (Power BI)
I designed a two page dashboard focused on readability and usability. I also removed unnecessary labels and legends to keep the visuals cleaner and easier to scan.

---

# 📖 Dashboard Overview

## 1. Executive Overview

This page provides a general overview of customer activity and business performance.

<img width="1012" height="571" alt="image" src="https://github.com/user-attachments/assets/75bd113a-ae20-43de-833e-645ffdc4ee7c" />



### Key Metrics
- Total Customers: 7.04K
- Total Revenue: $2.86M
- Churn Rate: 26.54%

### Main Insights
- Fiber Optic customers showed higher churn compared to DSL users.
- Customers without Tech Support were more likely to leave the service.
- Paperless billing users appeared more frequently among churned customers.

I used funnel charts and distribution visuals to make churn patterns easier to compare.

---

## 2. Financial Risk Analysis

This page focuses on the financial impact of customer churn and revenue risk.

<img width="1014" height="568" alt="image" src="https://github.com/user-attachments/assets/8a9e5103-8a8b-4313-9939-6e9934779c94" />

### Key Metrics
- Average Customer Tenure: 32.37 months
- Average Monthly Charges of Churned Customers: $74.44/month

### Main Insights
- Month to Month contracts had the highest churn levels.
- Customers with high monthly charges and low tenure formed a high risk churn group.
- Longer contract terms showed more stable revenue patterns.

The scatter plot helped identify customers who leave early despite having relatively high monthly payments.

---

# 💻 SQL Exploratory Data Analysis


Below are some of the SQL queries used during the initial data auditing process and risk trend analysis.

## 1. Data Quality Checks
```sql
-- Checking for missing or blank values in TotalCharges
SELECT COUNT(*) 
FROM telecom_churn.`wa_fn-usec_-telco-customer-churn - copy`
WHERE TotalCharges = ' ' OR TotalCharges IS NULL;

-- Checking for duplicate customer IDs
SELECT customerID, COUNT(*) 
FROM telecom_churn.`wa_fn-usec_-telco-customer-churn - copy`
GROUP BY customerID
HAVING COUNT(*) > 1;
```
## 2. Churn Trend Analysis
```sql
-- Churn rate by contract type
SELECT 
    Contract, 
    COUNT(*) AS Total_Customers,
    SUM(CASE WHEN Churn = 'Yes' THEN 1 ELSE 0 END) AS Churned_Customers,
    ROUND(100.0 * SUM(CASE WHEN Churn = 'Yes' THEN 1 ELSE 0 END) / COUNT(*), 2) AS Churn_Rate_Pct
FROM telecom_churn.`wa_fn-usec_-telco-customer-churn - copy`
GROUP BY Contract;

-- Monthly charges comparison between churned and retained customers
SELECT 
    Churn, 
    COUNT(*) AS Customer_Count,
    ROUND(SUM(MonthlyCharges), 2) AS Total_Monthly_Revenue,
    ROUND(AVG(MonthlyCharges), 2) AS Avg_Monthly_Bill
FROM telecom_churn.`wa_fn-usec_-telco-customer-churn - copy`
GROUP BY Churn;

-- Churn rate by internet service type
SELECT 
    InternetService, 
    COUNT(*) AS Total_Customers,
    SUM(CASE WHEN Churn = 'Yes' THEN 1 ELSE 0 END) AS Churned_Customers,
    ROUND(100.0 * SUM(CASE WHEN Churn = 'Yes' THEN 1 ELSE 0 END) / COUNT(*), 2) AS Churn_Rate_Pct
FROM telecom_churn.`wa_fn-usec_-telco-customer-churn - copy`
GROUP BY InternetService;
```
## 🚀 Project Files & Setup

### 📊 Files
- Dataset: [WA_Fn-UseC_-Telco-Customer-Churn.csv](WA_Fn-UseC_-Telco-Customer-Churn.csv)
- Power BI Dashboard: [Telecom Customer Churn Analysis.pbit](Telecom%20customer%20Churn%20Analysis.pbit)

### 🛠️ Running the Project

1. Clone this repository.
2. Open the .pbit file in Power BI Desktop.
3. Import the .csv dataset into MySQL if you want to run the SQL queries locally.


