# SQL Customer Churn Analysis Project
## Project Title

### Customer Churn Analysis Using SQL

### Project Summary

##### This project analyzes customer churn data using SQL to identify the main factors that cause customers to leave a company. The analysis focuses on customer demographics, contract types, internet services, monthly charges, and support services. By using SQL queries, the project finds patterns in churn behavior and provides recommendations to improve customer retention.

### Project Description

The goal of this project was to help the business understand why customers are leaving and which customer groups have the highest churn rates.

### The dataset contains the following columns:

- CustomerID
- Age
- Gender
- Tenure
- MonthlyCharges
- ContractType
- InternetService
- TotalCharges
- TechSupport
- Churn

### Using SQL, the project performs:

- Data cleaning
- Exploratory data analysis
- Customer segmentation
- Churn rate analysis
- Revenue impact analysis
- Customer behavior analysis

##### The project helps stakeholders make data-driven decisions to reduce customer churn.

### My Responsibilities:

- Cleaned and validated customer data using SQL
- Analyzed churn trends and customer behavior
- Created SQL queries for business reporting
- Identified high-risk customer groups
- Measured churn percentages across different services
- Generated insights and recommendations for retention strategies
- Optimized SQL queries for faster reporting

### Challenges Faced :

- Handling missing or inconsistent customer records
- Calculating churn percentages accurately
- Identifying hidden churn patterns across multiple customer segments
- Managing duplicate customer records
- Analyzing large datasets efficiently using SQL queries



## SQL Analysis
```sql

CREATE TABLE CustomerChurn (
    CustomerID INT PRIMARY KEY,
    Age INT,
    Gender VARCHAR(10),
    Tenure INT,
    MonthlyCharges DECIMAL(10,2),
    ContractType VARCHAR(50),
    InternetService VARCHAR(50),
    TotalCharges DECIMAL(10,2),
    TechSupport VARCHAR(10),
    Churn VARCHAR(10)
);


 1. Total Customers

SELECT COUNT(*) AS TotalCustomers
FROM CustomerChurn;

 2. Total Churned Customers

SELECT COUNT(*) AS ChurnedCustomers
FROM CustomerChurn
WHERE Churn = 'Yes';

 3. Overall Churn Rate

SELECT 
    ROUND(
        (SUM(CASE WHEN Churn = 'Yes' THEN 1 ELSE 0 END) * 100.0)
        / COUNT(*),
    2) AS ChurnRate
FROM CustomerChurn;

 4. Churn Rate by Contract Type

  SELECT 
    ContractType,
    COUNT(*) AS TotalCustomers,
    SUM(CASE WHEN Churn = 'Yes' THEN 1 ELSE 0 END) AS ChurnedCustomers,
    ROUND(
        (SUM(CASE WHEN Churn = 'Yes' THEN 1 ELSE 0 END) * 100.0)
        / COUNT(*),
    2) AS ChurnRate
FROM CustomerChurn
GROUP BY ContractType
ORDER BY ChurnRate DESC;

 5. Average Monthly Charges by Churn

SELECT 
    Churn,
    ROUND(AVG(MonthlyCharges),2) AS AvgMonthlyCharges
FROM CustomerChurn
GROUP BY Churn;

 6. Customers Without Tech Support More Likely to Churn

SELECT 
    TechSupport,
    COUNT(*) AS TotalCustomers,
    SUM(CASE WHEN Churn = 'Yes' THEN 1 ELSE 0 END) AS ChurnedCustomers
FROM CustomerChurn
GROUP BY TechSupport;


 7. Churn by Internet Service

SELECT 
    InternetService,
    COUNT(*) AS TotalCustomers,
    SUM(CASE WHEN Churn = 'Yes' THEN 1 ELSE 0 END) AS ChurnedCustomers,
    ROUND(
        (SUM(CASE WHEN Churn = 'Yes' THEN 1 ELSE 0 END) * 100.0)
        / COUNT(*),
    2) AS ChurnRate
FROM CustomerChurn
GROUP BY InternetService
ORDER BY ChurnRate DESC;


 8. High Value Customers Who Churned

SELECT 
    CustomerID,
    TotalCharges,
    MonthlyCharges,
    ContractType
FROM CustomerChurn
WHERE Churn = 'Yes'
AND TotalCharges > 5000
ORDER BY TotalCharges DESC;


 9. Tenure Analysis

SELECT 
    CASE
        WHEN Tenure BETWEEN 0 AND 12 THEN '0-1 Year'
        WHEN Tenure BETWEEN 13 AND 24 THEN '1-2 Years'
        WHEN Tenure BETWEEN 25 AND 48 THEN '2-4 Years'
        ELSE '4+ Years'
    END AS TenureGroup,
    
    COUNT(*) AS TotalCustomers,
    
    SUM(CASE WHEN Churn = 'Yes' THEN 1 ELSE 0 END) AS ChurnedCustomers,
    
    ROUND(
        (SUM(CASE WHEN Churn = 'Yes' THEN 1 ELSE 0 END) * 100.0)
        / COUNT(*),
    2) AS ChurnRate

FROM CustomerChurn
GROUP BY TenureGroup
ORDER BY ChurnRate DESC;


 10. Top 10 Customers with Highest Charges

SELECT TOP 10
    CustomerID,
    TotalCharges
FROM CustomerChurn
ORDER BY TotalCharges DESC;


 11. Rank Customers by Total Charges

SELECT 
    CustomerID,
    TotalCharges,
    RANK() OVER(ORDER BY TotalCharges DESC) AS ChargeRank
FROM CustomerChurn;


 12. Duplicate Record Check

SELECT 
    CustomerID,
    COUNT(*) AS DuplicateCount
FROM CustomerChurn
GROUP BY CustomerID
HAVING COUNT(*) > 1;
```



## Key Findings / Results

- Customers with month-to-month contracts had the highest churn rate
- Customers without tech support were more likely to leave
- Higher monthly charges increased churn probability
- Customers with shorter tenure had higher churn rates
- Fiber internet users showed higher churn compared to other services


## Recommendations

- Offer discounts for long-term contracts
- Improve customer support services
- Create loyalty programs for new customers
- Reduce service issues for high-paying customers
- Provide personalized retention offers for high-risk customers
- Monitor customers with high monthly charges more closely


### Tools Used
- SQL Server / MySQL
- Window Functions
- Aggregate Functions
- CASE Statements
- GROUP BY and JOINs
- Data Cleaning Techniques



