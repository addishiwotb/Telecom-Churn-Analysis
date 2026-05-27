# 📊 Telecom Customer Churn Analysis

## 📌 Project Overview
I built this project to figure out exactly why customers are leaving this telecom company. By analyzing customer behavior patterns, my goal was to pinpoint the specific pain points driving churn so the business can take proactive steps to improve customer retention.

The final project connects a custom MySQL backend to an interactive two-page Power BI dashboard.

---

## 🎯 The Core Questions I Explored
Instead of just looking at the surface-level numbers, I used data to answer a few specific business questions:
- Which customer profiles face the highest risk of leaving?
- How do contract types and payment methods influence a customer's decision to stay?
- At what point in the customer lifecycle (tenure) does churn usually happen?
- What structural changes can the company make to protect its monthly revenue?

---

## 📁 Dataset & Tools Used
- SQL (MySQL): Used for initial data auditing, quality checks, and querying raw customer trends.
- Power BI & Power Query: Used to transform data types, clean up null values, and build interactive report pages.
- DAX: Used to write custom calculations for key performance indicators (KPIs) and churn percentages.
- Dataset: The classic *Telco Customer Churn dataset*, which tracks customer attributes like contract details, service types, monthly bills, and tenure.

---

# 📖 Dashboard Overview & Analysis

## 1. Executive Overview
This page provides a general overview of customer activity, service preferences, and overall business performance metrics.

<img width="1012" height="571" alt="image" src="https://github.com/user-attachments/assets/75bd113a-ae20-43de-833e-645ffdc4ee7c" />


### Key Metrics & Visual Highlights
- Total Customers: 7.04K
- Total Revenue: $2.86M
- Overall Churn Rate: 26.54%

### Core Insights
- The Internet Service Gap: Fiber optic users are showing significantly higher churn trends compared to standard DSL users. 
- Missing Support Features: Customers who skipped adding Tech Support or Online Security features to their plans left the company at a much higher rate.
- Paperless Billing Friction: A large concentration of churned accounts were tied to paperless billing and manual payment setups.

---

## 2. Financial Risk Analysis
This page shifts focus toward the financial impact of lost accounts and highlights where current recurring revenue is most exposed to risk.

<img width="1014" height="568" alt="image" src="https://github.com/user-attachments/assets/8a9e5103-8a8b-4313-9939-6e9934779c94" />

### Key Metrics & Visual Highlights
- Average Customer Tenure: 32.37 months
- Average Monthly Bill (Churned Users): $74.44/month

### Core Insights
- The Contract Trap: Customers on Month-to-Month contracts are overwhelmingly the most likely to drop the service. Longer-term contracts (1 and 2 years) show highly stable revenue retention.
- The Onboarding Window: The first 0 to 6 months are highly critical. Accounts with high monthly bills paired with low tenure form our absolute highest-risk group.
- Payment Methods: Users utilizing manual electronic checks churned at notably higher frequencies than those utilizing automated billing features.

---

# 💻 SQL Exploratory Data Analysis

Below are the SQL queries I wrote during the data auditing and verification phase before visualizing the trends.

### 1. Data Quality Checks
```sql
-- Checking for missing or blank values in TotalCharges
SELECT COUNT(*) 
FROM telecom_churn.`wa_fn-usec_-telco-customer-churn - copy`
WHERE TotalCharges = ' ' OR TotalCharges IS NULL;

-- Checking for duplicate customer IDs to ensure data integrity
SELECT customerID, COUNT(*) 
FROM telecom_churn.`wa_fn-usec_-telco-customer-churn - copy`
GROUP BY customerID
HAVING COUNT(*) > 1;
```
-- Calculating churn rates across different contract types
SELECT 
    Contract, 
    COUNT(*) AS Total_Customers,
    SUM(CASE WHEN Churn = 'Yes' THEN 1 ELSE 0 END) AS Churned_Customers,
    ROUND(100.0 * SUM(CASE WHEN Churn = 'Yes' THEN 1 ELSE 0 END) / COUNT(*), 2) AS Churn_Rate_Pct
FROM telecom_churn.`wa_fn-usec_-telco-customer-churn - copy`
GROUP BY Contract;

-- Comparing monthly billing metrics between churned and active customers
SELECT 
    Churn, 
    COUNT(*) AS Customer_Count,
    ROUND(SUM(MonthlyCharges), 2) AS Total_Monthly_Revenue,
    ROUND(AVG(MonthlyCharges), 2) AS Avg_Monthly_Bill
FROM telecom_churn.`wa_fn-usec_-telco-customer-churn - copy`
GROUP BY Churn;

-- Analyzing churn percentages across internet service categories
SELECT 
    InternetService, 
    COUNT(*) AS Total_Customers,
    SUM(CASE WHEN Churn = 'Yes' THEN 1 ELSE 0 END) AS Churned_Customers,
    ROUND(100.0 * SUM(CASE WHEN Churn = 'Yes' THEN 1 ELSE 0 END) / COUNT(*), 2) AS Churn_Rate_Pct
FROM telecom_churn.`wa_fn-usec_-telco-customer-churn - copy`
GROUP BY InternetService;

