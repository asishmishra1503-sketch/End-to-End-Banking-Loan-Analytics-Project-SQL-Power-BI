# End-to-End-Banking-Loan-Analytics-Project-SQL-Power-BI
I recently built a complete Bank Loan Performance Analysis project covering the full data workflow — from data loading in MySQL → SQL analysis → interactive Power BI dashboard.  This project simulates how banks evaluate loan portfolios, monitor risk, and make data-driven decisions.

🔧 1. Data Engineering (MySQL)
• Created database: banking_analytics
• Loaded dataset using LOAD DATA INFILE
• Performed data validation using COUNT() and exploratory queries

🧠 2. SQL Analysis (Business Problem Solving)

📈 Loan Growth Trend
Analyzed year-wise loan disbursement
➡️ Loans grew significantly from 2M (2007) to 261M (2011)
➡️ Indicates rapid business expansion and increased credit demand

💳 Credit Risk Analysis (Grade & Sub-Grade)
• Higher revolving balances in Grade A & B
• Lower grades show reduced exposure but higher default risk
➡️ Helps banks prioritize low-risk, high-value customers

✔️ Verification Impact
• Verified: 58.88% (220M payments)
• Not Verified: 41.12% (154M payments)
➡️ Verified borrowers show stronger repayment behavior

🌍 Geographic & Seasonal Trends
• State-wise and month-wise loan distribution
➡️ Identifies high-performing regions and seasonal lending patterns

🏠 Customer Segmentation (Home Ownership)
• Mortgage: 50.41% (224M)
• Rent: 42.53% (189M)
• Own: 7.06% (31M)
➡️ Mortgage holders dominate → financially stable segment

🎯 Loan Purpose Analysis (Top Drivers)
• Major purposes: Other (32M), Small Business (25M), Wedding (9M)
➡️ Loan demand driven by business and personal financial needs

📊 3. Power BI Dashboard Insights

💰 Overall Portfolio
• Total Loan Amount: 446M
• Total Payments: 483M
• Total Accounts: 877K
➡️ Strong repayment performance across portfolio

📉 Loan Status Breakdown
• Fully Paid: 80.35%
• Charged Off: 15.29%
• Current: 4.36%
➡️ Majority loans are successfully closed, but charge-offs highlight risk areas

📊 Risk Distribution
• High exposure in Grade B & A customers
➡️ Useful for credit risk monitoring and policy decisions

💡 Final Insights: How Banks Use This Analysis

This project demonstrates how financial institutions:
✔️ Track loan growth and portfolio performance
✔️ Segment customers based on risk & behavior
✔️ Identify high-risk loans (charge-offs)
✔️ Optimize lending strategies using data

🚀 Tools & Skills Used:
SQL | MySQL | Power BI | Data Cleaning | Data Modeling | Business Analysis

📌 This project strengthened my ability to turn raw financial data into actionable business insights.






I have used all this query to retrieve all the data from the source data.

Select * From finance_data;
Select count(*) from finance_data;

-- 1. Year-wise Loan Amount Stats

SELECT 
    YEAR(STR_TO_DATE(issue_d, '%d-%m-%Y %H:%i')) AS year, 
    Concat(ROUND(SUM(loan_amnt) / 1000000, 2), ' M') AS total_loan_amount
FROM finance_data
GROUP BY year
ORDER BY year;

-- 2. Grade and Sub-Grade wise Revolving Balance

Select Grade, Sub_Grade,
Concat(Round(Sum(revol_bal)/1000000,2), ' M') as Total_Revolve_Balance From Finance_Data
Group By Grade, Sub_Grade
Order By Grade, Sub_Grade Desc;

-- 3.Total Payment: Verified vs. Non-Verified

Select verification_status, 
Concat(Round(Sum(total_pymnt)/1000000,2),' M') as Total_payment
From Finance_Data
Where verification_status IN ('Verified' ,'Not Verified')
Group By verification_status;

-- 4. State-wise and Month-wise Loan Status

SELECT 
    addr_state AS State,
    MONTHNAME(STR_TO_DATE(issue_d, '%d-%m-%Y %H:%i')) AS Month_Name, 
    loan_status, Concat(Round(Sum(loan_amnt)/100000,2), ' M') as Total_Loan_Amount
FROM finance_data
GROUP BY 
    addr_state,
    MONTHNAME(STR_TO_DATE(issue_d, '%d-%m-%Y %H:%i')), MONTH(STR_TO_DATE(issue_d, '%d-%m-%Y %H:%i')),
    loan_status
ORDER BY 
 MONTH(STR_TO_DATE(issue_d, '%d-%m-%Y %H:%i'));
 
-- 5. Home Ownership vs. Last Payment Date Stats

SELECT 
    home_ownership,
    last_pymnt_d AS LastPaymentDate,
    COUNT(id) AS Borrower_Count,
    concat(Round(SUM(total_pymnt)/ 1000000, 2), ' M') AS Total_Amount_Paid
FROM finance_data
WHERE last_pymnt_d IS NOT NULL
GROUP BY home_ownership, last_pymnt_d;
 

 -- 6. What are the Top 5 Reasons (Purposes) for Loans?
 -- Banks need to know why people are borrowing to tailor their marketing. Is it for debt consolidation, home improvement, or weddings?
 
 SELECT
    purpose,
    COUNT(id) AS Total_Applications,
   Concat(ROUND(SUM(loan_amnt) / 1000000, 2), ' M') AS Total_Loan_Amount,
    Concat(Round(AVG(int_rate) * 100,2), ' %') AS Avg_Int_Rate
FROM finance_data
GROUP BY purpose
ORDER BY Total_Applications DESC
Limit 5 ;





















