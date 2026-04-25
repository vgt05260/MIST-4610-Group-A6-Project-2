# MIST-4610-Group-A6-Project-2

# Group Information
Group A6
- Group Leader: Victoria Taylor
- Conceptual Modeler: Yahia Abdelkarim
- Database Designer: Rishi Patel
- Data Wrangler: Enaz Tewfik
- SQL Writer: Kevin Serna

# Case Summary
brief explanation of the company and data challenges.

# Conceptual Model
PNG of the model plus a short English explanation of the entities & relationships.

# Data Quality Assessment
identify and explain the main data quality issues in the source file.

# Data Cleaning Process
explain how you resolved the data quality issues identified previously.
Include any SQL statements used to standardize, split, convert, or update the imported data.

# Queries
## Query 1
<img width="1181" height="607" alt="Screenshot 2026-04-24 at 10 27 15 PM" src="https://github.com/user-attachments/assets/4b621a0a-3c1d-4310-8371-3d615caa3cf9" />

## Query 2
<img width="1180" height="473" alt="Screenshot 2026-04-24 at 10 28 02 PM" src="https://github.com/user-attachments/assets/64ce5502-d6d1-4763-b329-86e239947f21" />

## Query 3
<img width="1179" height="399" alt="Screenshot 2026-04-24 at 10 28 56 PM" src="https://github.com/user-attachments/assets/e1200012-a499-4125-8933-749c5a377459" />


## Query 4
Query 4 lists each product category along with the number of distinct products sold, total orders, total revenue, and average order value, ordered from highest to lowest revenue.
<img width="1181" height="445" alt="Screenshot 2026-04-24 at 10 29 09 PM" src="https://github.com/user-attachments/assets/6e6b99d8-9270-4564-807b-8249e0a335ae" />


### Managerial Justification
This query is useful for identifying which product categories are the primary revenue drivers for Northline Outfitters and understanding how each category performs in terms of sales volume and order value. Management can use this information to make strategic decisions about inventory investment, allocate marketing budgets more effectively, and identify underperforming categories that may need promotional support or re-evaluation. Additionally, understanding the relationship between number of products sold and revenue helps assess whether category success is driven by product variety or by strong sales of specific items, which can inform future product sourcing and category expansion strategies.

### Technical Justification
This query demonstrates aggregation using COUNT(DISTINCT), COUNT(), SUM(), AVG(), and ROUND() functions combined with GROUP BY to summarize data at the category level. It reinforces understanding of multiple aggregate functions within a single query, a three-table join (lineitem, order, product) to combine transactional and product data, and sorting by a computed column (total_revenue). This reflects practical SQL use for sales analysis and business reporting.

## Query 5
Query 5 lists each customer type (Student, Loyalty, Guest) along with the total number of orders, number of unique customers, total revenue, and average order value, ordered from highest to lowest revenue.
<img width="1179" height="410" alt="Screenshot 2026-04-24 at 10 29 52 PM" src="https://github.com/user-attachments/assets/93029391-725e-4642-a3b7-109dc49d9544" />

### Managerial Justification
This query is useful for understanding which customer segments generate the most revenue for Northline Outfitters and how different customer types behave in terms of purchasing patterns. Management can use this information to tailor marketing campaigns, develop targeted loyalty programs, and determine where to invest in customer acquisition versus retention efforts. For example, if student customers generate high volume but lower average order values, the company might focus on increasing basket size through bundling or minimum purchase promotions. Additionally, comparing unique customers to total orders reveals which segments have higher repeat purchase rates, informing customer lifetime value strategies.

### Technical Justification
This query demonstrates aggregation functions including COUNT(DISTINCT) for counting unique customers, COUNT() for total orders, SUM() for revenue totals, AVG() for average calculations, and ROUND() for formatting. It uses a three-table join (customer, order, lineitem) to combine customer attributes with transactional data, and applies GROUP BY to aggregate results by customer type. It also reinforces sorting by computed columns and handling multiple aggregations to provide comprehensive segment analysis.

## Query 6
Query 6 lists each vendor along with their total number of products, total orders, total revenue, average order value, and revenue generated per product, ordered from highest to lowest total revenue.
<img width="1568" height="616" alt="image" src="https://github.com/user-attachments/assets/ed78c04c-e2a0-4093-bb3e-021124d02d7f" />

### Managerial Justification
This query is useful for evaluating vendor partnerships and identifying which suppliers are generating the most revenue for Northline Outfitters. Management can use this information to prioritize vendor relationships, negotiate better terms with high-performing vendors, and identify underperforming suppliers that may need strategic review. The revenue per product metric is particularly valuable as it reveals vendor efficiency vendors with high revenue per product indicate strong product selection and market fit, while those with low revenue per product may be offering too many slow-moving items. This analysis supports decisions about vendor contract renewals, product sourcing strategies, and inventory investment allocation across different suppliers.

### Technical Justification
This query demonstrates aggregation using COUNT(DISTINCT) for counting unique products and orders, along with SUM(), AVG(), and ROUND() functions for revenue calculations. It includes a computed column (revenue_per_product) that divides total revenue by product count, showing understanding of arithmetic operations within SELECT statements. The query uses a four-table join (vendor, product, lineitem, order) to combine supplier information with actual sales data, and applies GROUP BY to aggregate results at the vendor level. It reinforces sorting by computed columns and demonstrates how to analyze business performance across different suppliers.
