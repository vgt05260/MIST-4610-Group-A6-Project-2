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
## Query 4
Query 4 lists each product category along with the number of distinct products sold, total orders, total revenue, and average order value, ordered from highest to lowest revenue.

### Managerial Justification
This query is useful for identifying which product categories are the primary revenue drivers for Northline Outfitters and understanding how each category performs in terms of sales volume and order value. Management can use this information to make strategic decisions about inventory investment, allocate marketing budgets more effectively, and identify underperforming categories that may need promotional support or re-evaluation. Additionally, understanding the relationship between number of products sold and revenue helps assess whether category success is driven by product variety or by strong sales of specific items, which can inform future product sourcing and category expansion strategies.

### Technical Justification
This query demonstrates aggregation using COUNT(DISTINCT), COUNT(), SUM(), AVG(), and ROUND() functions combined with GROUP BY to summarize data at the category level. It reinforces understanding of multiple aggregate functions within a single query, a three-table join (lineitem, order, product) to combine transactional and product data, and sorting by a computed column (total_revenue). This reflects practical SQL use for sales analysis and business reporting.

## Query 5
Query 5 lists each customer type (Student, Loyalty, Guest) along with the total number of orders, number of unique customers, total revenue, and average order value, ordered from highest to lowest revenue.

### Managerial Justification
This query is useful for understanding which customer segments generate the most revenue for Northline Outfitters and how different customer types behave in terms of purchasing patterns. Management can use this information to tailor marketing campaigns, develop targeted loyalty programs, and determine where to invest in customer acquisition versus retention efforts. For example, if student customers generate high volume but lower average order values, the company might focus on increasing basket size through bundling or minimum purchase promotions. Additionally, comparing unique customers to total orders reveals which segments have higher repeat purchase rates, informing customer lifetime value strategies.

### Technical Justification
This query demonstrates aggregation functions including COUNT(DISTINCT) for counting unique customers, COUNT() for total orders, SUM() for revenue totals, AVG() for average calculations, and ROUND() for formatting. It uses a three-table join (customer, order, lineitem) to combine customer attributes with transactional data, and applies GROUP BY to aggregate results by customer type. It also reinforces sorting by computed columns and handling multiple aggregations to provide comprehensive segment analysis.

## Query 6
Query 6 lists customers who have made multiple purchases, showing their email, customer type, total number of orders, total amount spent, average order value, and a computed customer segment classification (VIP, Loyal, Returning, or One-Time), filtered to show only repeat customers and ordered from highest to lowest total spending.

### Managerial Justification
This query is useful for identifying Northline Outfitters' most valuable repeat customers and understanding the characteristics of customer loyalty. Management can use this information to develop targeted retention programs, create tiered loyalty rewards, and allocate personalized marketing resources to high-value customers. By segmenting customers based on purchase frequency (VIP for 5+ orders, Loyal for 3-4, Returning for 2), the company can implement differentiated engagement strategies such as exclusive promotions for VIP customers or re-engagement campaigns for returning customers. Additionally, analyzing spending patterns across segments helps quantify the financial impact of customer retention and justifies investment in customer service and loyalty initiatives.

### Technical Justification
This query demonstrates the use of a CASE statement to create a derived attribute (customer_segment) based on aggregate purchase frequency, combining conditional logic with aggregation. It uses COUNT(DISTINCT), SUM(), AVG(), and ROUND() functions along with GROUP BY to summarize customer behavior. The query also employs a HAVING clause to filter aggregated results (showing only customers with more than 1 order), a three-table join (customer, order, lineitem), and an IS NOT NULL condition to ensure data quality. It reinforces sorting by computed columns and demonstrates practical application of filtering at both the row level (WHERE) and aggregate level (HAVING).
