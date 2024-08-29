# Croma India Product Wise Sales Report

## Project Overview

The Product owner requires a generated report of Individual Product Sales (aggregated on a monthly basis at the Product Code Level) for Croma India customer for Fiscal Year 2021. This is to track individual product sales and run further product analysis on it.

 
AtliQ Hardware has experienced significant growth in recent years and decided to adopt data analytics using Power BI to outpace competitors and enhance decision-making. This initiative aims to address stakeholder queries across various domains such as finance, sales, marketing, and supply chain.

I worked on this project by following the Codebasics SQL Course, Link to the bootcamp is [here](https://codebasics.io/bootcamps/data-analytics-bootcamp-with-practical-job-assistance?utm_campaign=dataanalyticsbootcamp&utm_id=googleadspaid&gad_source=1&gclid=Cj0KCQjwztOwBhD7ARIsAPDKnkDs79RdFwkyz6S1qzIfPIK4qzgba2Pq3bfgK-eGVtLDO8a5vIDMR74aAvyUEALw_wcB)

## Technologies Used

- SQL (Structured Query Language) - MySQL (used)

## MySQL Skills Acquired

- SQL Joins and Subquery
- Creating calculated columns
- CTE (Common Table Expressions)
- User Defined Functions
- Stored Procedures
- And more 😅

## Key Business Concepts

- Fiscal Year
- Gross Price Per Item
- Gross Price Final
- Customer


## Company Background

AtliQ Hardware, a rapidly expanding global company, specializes in selling computers and accessories through various channels, including retail, direct sales, and distributors. The company recently faced a setback after opening stores in America based on limited data and intuition rather than comprehensive analysis. Meanwhile, competitors are leveraging extensive analytics teams for data-driven decisions. As a result, AtliQ Hardware is prioritizing the development of its own analytics team to remain competitive.


## Understanding the Dataset

A thorough understanding of the available data is crucial before beginning any analysis. 

### 1. dim_customer
       - 27 distinct markets (ex India, USA, spain)
       - 75 distinct customers thorough out the market
       - 2 types of platforms
            - Brick & Motors - Physical/offline store
            - E-commerce - Online Store (Amazon, Croma)
        - Three channels
            - Retailer
            - Direct
            - Distributors

### 2. dim_product
        - Divisions
            - P & A
                - Peripherals
                - Accessories
            - PC
                - Notebook
                - Desktop
            - N & S
                - Networking
                - Storage
        - There are 14 different categories, Like Internal HDD, keyboard
        - There are different variants available for the same product
### 3. fact_sales_monthly
        - The table is denormalized by data engineering team, as it is a data warehouse which is aimed to be used for analytical work.
        - All the date of the month will be replaced by the start date of the month
        - It will have all the column names and in the end it will have the value of sold quantity for a combination of date (start of month), product_code(indicating the product) and 
          customer_code(to identify the customer)
### 4.  fact_gross_price
        - Has the details of gross prices with product code

## Usage of CTE's and Stored Procedures

**Common Table Expressions (CTEs):**
1. **Readability:** CTEs simplify complex queries by breaking them into smaller, manageable parts, making the SQL code easier to read and understand.
2. **Reusability:** CTEs allow reuse of subquery results within the same query, reducing redundancy and improving query performance.
3. **Modularity:** CTEs enhance modularity in SQL code, allowing developers to write complex logic in a step-by-step manner.
4. **Debugging:** Easier to debug and maintain compared to nested subqueries.

**Stored Procedures:**
1. **Performance Optimization:** Stored procedures are precompiled, which can lead to faster execution times compared to running multiple individual SQL queries.
2. **Security:** They allow for better control of data access, as users can execute procedures without direct access to the underlying tables.
3. **Consistency:** Promotes code reuse and ensures consistency in logic and business rules across the application.
4. **Maintenance:** Centralized SQL logic within stored procedures makes maintenance and updates easier, reducing the likelihood of errors.

Both CTEs and stored procedures contribute to better performance, maintainability, and security in database operations. 

## Project Walkthrough:

### 1. Creating Fiscal Date User Defined Function

<img src="https://github.com/SandeepanBhattacharyya/Croma-India-Product-Wise-Sales-Report/blob/main/fiscal_date.png" class="center">

- The function takes in a value of `DATE` type and ultimately return a `DATE` type value.
- We declare the temporary variable called `Fiscal_date`.
- Atliq's Fiscal Year starts from the month of September. So in terms of Fiscal Year, September will be considered as month one (1). So we will add four (4) month to September to convert it into the state of January. We use the following MySQL approach.
`date_add(calendar_date, INTERVAL 4 MONTH)`
- Then we finaly retun the value.
-This Function is a DETERMINISTIC function. This indicates that for the same input the output will always remain the same.

### 2. Creating Fiscal Quarter User Defined Function

<img src="https://github.com/SandeepanBhattacharyya/Croma-India-Product-Wise-Sales-Report/blob/main/fiscal_quarter.png" class="center">

- The function takes in a value of `DATE` type and ultimately return a `Char` type value with max 2 characters. This is helpful as we are dealing with Quarters (Q1, Q2, Q3, Q4)
- We declare the temporary variable called `fiscal_quarter char` with max length of 2 characters.
- We use a `SWITCH Case` statement to tackle multiple IF/ELSE Scenarios for better code readability.
- The `month` function will retun the value of the month in number format, post which we can sort them into their respective Groups(Quarters).
- The input to the `get_fiscal_quarter` function will be the output of `get_fiscal_date` function, in order for our calculations to yield proper results.

### 3. Code Explanation:

<img src="https://github.com/SandeepanBhattacharyya/Croma-India-Product-Wise-Sales-Report/blob/main/main_query.png" class="center">

- This SQL code is designed to extract and calculate sales data for a specific customer, focusing on the Fiscal Year 2021 and the first fiscal quarter (Q4). Here's a breakdown:

a. `Sales Record Extraction (WITH sales_record_2021 AS...)`:

- A Common Table Expression (CTE) named sales_record_2021 is created to select sales data for Fiscal Year 2021 and Fiscal Quarter Q4.
- It retrieves the fiscal month, fiscal quarter, fiscal year, product code, and quantity sold from the fact_sales_monthly table.
- The sales data is filtered for customers containing "croma" in their names.
- The results are grouped by product code and the month, adjusted by adding 4 months to align with the fiscal year.

b. `Main Query (SELECT...)`:

- The main query selects data from the sales_record_2021 CTE.
- It joins the product information from the dim_product table and gross price data from the fact_gross_price table.
- The query outputs the fiscal month, quarter, product name, variant, sold quantity, gross price per item, and the total gross price (sold quantity multiplied by gross price) for each product sold.


### 4. Output Received:

<img src="https://github.com/SandeepanBhattacharyya/Croma-India-Product-Wise-Sales-Report/blob/main/main_query.png" class="center">
____________________________________________________________________________________________

## Enhancement to handle multiple customers:

Created a Stored Procudere where we can supply multiple customer codes (as comma separated values), and it will provide us the result for total monthly sales combined.

This will give results when we pass comma separated values for the FIND_IN_SET function as it will give the position by comparing. 
Stored procs return multiple formats like 1 value, multiple value or tables (like any another SQL statement). This is different from functions (user defines) as they return a single value.

## Code explanation
<img src="https://github.com/SandeepanBhattacharyya/Croma-India-Product-Wise-Sales-Report/blob/main/stored_Procedure.png" class="center">

### 1. Procedure Definition:

- The procedure is defined by the user devuser on the localhost server.
- It takes one input parameter, in_customer_codes, which is expected to be a text string containing customer codes.

### 2. Main Query:

- The query selects the date and the total monthly sales (monthly_sales) for each date in the fact_sales_monthly table.
- The total sales for each date are calculated by multiplying the sold_quantity with the gross_price from the fact_gross_price table, then rounding the result to 2 decimal places, and summing it up.
- A join is performed between the fact_sales_monthly and fact_gross_price tables using the fiscal year (calculated from the date field) and product_code.

### 3. Filtering:

- The WHERE clause filters the sales data based on the customer codes provided in in_customer_codes. The `FIND_IN_SET` function checks if the customer_code exists within the input string.

### 4. Grouping and Ordering:

- The results are grouped by the date, so the sales are aggregated by each date.
- The final results are ordered in descending order by date (ORDER BY s.date DESC), so the most recent sales data appears first.
- This procedure returns the gross monthly sales for the specified customers.

### 4. Stored Procedure output:
<img src = "https://github.com/SandeepanBhattacharyya/Croma-India-Product-Wise-Sales-Report/blob/main/stored%20proc%20output.png" class ="center">

- ## Project Outcome

This report enables data-driven decision-making, allowing stakeholders to answer various "why" questions based on different scenarios.
