![image alt](https://github.com/cbungvu963/Consumer-Behavior-and-Shopping-Habits---Data-Analysis---Power-BI/blob/dc9e81218fe2c9ff7a294ec6222ab33152d376cf/Overview%20Dashboard.png)

![image alt](https://github.com/cbungvu963/Consumer-Behavior-and-Shopping-Habits---Data-Analysis---Power-BI/blob/dc9e81218fe2c9ff7a294ec6222ab33152d376cf/Customer%20Detail.png)

![image alt](https://github.com/cbungvu963/Consumer-Behavior-and-Shopping-Habits---Data-Analysis---Power-BI/blob/dc9e81218fe2c9ff7a294ec6222ab33152d376cf/Map.png)
This project focuses on analyzing customer shopping behavior using the shopping_trends.csv dataset to uncover consumption patterns and trends. The process includes data import, cleaning, modeling using the Star Schema approach, creating DAX measures for analysis, identifying key insights, and visualizing data in Power BI. The goal is to deliver a professional report to support business decision-making, such as optimizing promotions, segmenting customers, and analyzing seasonal or regional trends.
1. Data Import and Cleaning
1.1 Data Import

Primary Source: shopping_trends.csv (3,900 records, 18 columns: Customer ID, Age, Gender, Item Purchased, Category, Purchase Amount (USD), Location, Size, Color, Season, Review Rating, Subscription Status, Payment Method, Shipping Type, Discount Applied, Promo Code Used, Previous Purchases, Frequency of Purchases).

1.2 Data Cleaning

Handling Missing Values/Nulls: Checked SalesDate in shopping_trends.xlsx for missing values, converted numeric dates (e.g., 42370) to date format (mm/dd/yyyy) using the Excel formula =IFERROR(DATEVALUE(K2), IF(ISNUMBER(K2), K2, "")), then formatted as Short Date to avoid #VALUE! errors.
Type Conversion:

Purchase Amount (USD), Review Rating, Previous Purchases: Converted to Decimal Number or Whole Number.
Frequency of Purchases: Encoded from strings ("Annually", "Every 3 Months", "Fortnightly", "Monthly", "Quarterly", "Weekly") to numbers (1, 4, 26, 12, 4, 52) using DAX for correlation analysis.
Subscription Status: Encoded from "Yes"/"No" to 1/0 using DAX.


Duplicate Handling: Verified no duplicates in Customer ID (each ID appears once).
Data Validation: Ensured no negative values in numeric columns, validated nulls in foreign keys post-modeling, and confirmed no duplicate Transaction Key.
Column Addition: Added SalesDate from shopping_trends.xlsx to shopping_trends via a 1:1 relationship using SalesDate = RELATED('shopping_trends'[SalesDate]).

Cleaning ensured data consistency, integrity, and readiness for modeling.
2. Data Modeling
The data was structured using the Star Schema methodology to optimize query performance and support multidimensional analysis.
2.1 Table Creation in Power Query

Fact Table: Fact_Transactions: Contains core transaction data with foreign keys linking to Dimension tables.
Dimension Tables:

Dim_Customer: Customer ID, Age, Gender, Subscription Status, Frequency of Purchases.
Dim_Product: Item Purchased, Category, Size, Color.
Dim_Location: Location.
Dim_Season: Season.
Dim_Payment: Payment Method.
Dim_Shipping: Shipping Type.
Dim_Promotion: Discount Applied, Promo Code Used.
Dim_Date (from Sales2-16.xlsx): SalesDate, with added Start_of_Month = STARTOFMONTH('Calendar'[Date]).



2.2 Relationship Establishment

One-to-Many Relationships: From Dimension tables to Fact_Transactions (e.g., Dim_Customer[Customer Key] -> Fact_Transactions[Customer Key]).
Unidirectionality: Relationships set as unidirectional (from Dimension to Fact) for performance optimization.
Data Integrity: Validated nulls in foreign keys, ensured no duplicate Transaction Keys, and filtered out negative values (if any) in Power Query.

The model ensures data integrity, minimizes redundancy, and supports flexible analysis.
3. DAX Measures and Calculated Columns
DAX measures and columns were developed to analyze promotions, reviews, seasonality, customer behavior, subscription impact, and geography. Key measures include:

Promotion Effectiveness: Avg Purchase with Promo, Avg Purchase without Promo, Percentage Sales with Discount, Promo Lift, Sales with Discount, Total Sales.
Product Reviews: Average Review Rating, Average Review Rating with Discount, Average Review Rating without Discount.
Seasonality: Average Seasonal Sales, Sales by Season, Seasonal Sales Index.
Customer Behavior: Average Purchase by Age Group, Average Purchase by Gender, High-Value Customers, Low-Value Customers (80/20 rule with a 90 USD threshold), One-time Customers, Percentage One-time Customers, Total Customer Spend, Total Customers.
Subscription Impact: Average Previous Purchases, Avg Previous Purchases Non-Subscribed, Avg Previous Purchases Subscribed, Sales from Non-Subscribed, Sales from Subscribed.
Geography: Total Sales by Location.
Additional: Quantity_of_items = COUNTROWS('shopping_trends'), Total_revenue = SUMX('shopping_trends', 1 * 'shopping_trends'[Purchased Amount (USD)]), One_Previous_Month_Revenue (corrected to calculate previous month revenue), Correlation_Freq_Subscription (fixed data type issues for correlation between Frequency of Purchases and Subscription Status).
Calculated Columns: Age Groups (age segmentation), Subscription_Status_Num (encoding "Yes"/"No" to 1/0), Frequency_of_Purchases_Num (encoding frequency to numeric values).

DAX formulas were tested and refined (e.g., handling string data types, using Calendar table for prior month calculations).
4. Key Insights Selection
Key insights were prioritized based on business relevance:

High promotion-driven revenue (Percentage Sales with Discount > 50%), indicating discounts boost sales.
Age group 30-39 shows the highest spending (Average Purchase by Age Group).
Subscribed customers (Subscription Status = "Yes") have higher purchase frequency (Average Previous Purchases Subscribed > Non-Subscribed).
Top-selling products: "Shirt" with Promo Code, "Pants" without Promo Code (from 1,000-sample analysis per set).
Correlation between purchase frequency and subscription (Correlation_Freq_Subscription ~0.5), suggesting subscriptions increase frequency.
Highest seasonal revenue in Spring (Seasonal Sales Index > 1).
80/20 Rule: 20% of customers generate 80% of revenue (High-Value Customers with a 90 USD threshold).

Insights were selected for actionable business outcomes, such as promotion optimization and customer segmentation.
5. Data Visualization
The report comprises 3 pages, designed for clarity and actionable insights:

Page 1: Overview Dashboard
- Visuals: Cards (Total Sales, Total Customers, Average Review Rating), Multi-Row Card (High-Value Customers, Low-Value Customers, Percentage One-time Customers), Donut Chart (Percentage Sales with Discount), KPI Card (Total Sales vs Target).
- Purpose: Provide a high-level summary of key metrics.


Page 2: Customer Detail
- Visuals: Bar Chart (Average Purchase by Age Group with Gender), Donut Chart (Percentage One-time Customers), Cards (High-Value Customers, Low-Value Customers), Table (Customer ID, Age, Gender, Total Customer Spend).
- Purpose: Deep dive into customer segmentation and spending behavior.


Page 3: Map
- Visuals: Map (Total Sales by Location), Bar Chart (Total Sales by Location), Gauge Chart (Average Review Rating by Location), Donut Chart (Sales from Subscribed vs Non-Subscribed).
- Purpose: Visualize geographic sales distribution and performance.


Features Integrated: Slicers, Conditional Formatting, Top N Filtering, Drillthrough Filters, Bookmarks, Custom Navigation Buttons, Slicer Panels, Numeric Range Parameters, Fields Parameters, Custom Tool Tips.

7. Conclusion
This project successfully analyzed shopping behavior data, from ingestion and cleaning to Star Schema modeling, DAX development, and visualization across 3 pages. The report delivers professional insights, supports for presentations, and aids business decisions. DAX and Power Query code are reusable for similar projects.

