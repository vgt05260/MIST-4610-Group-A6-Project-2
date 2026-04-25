# MIST-4610-Group-A6-Project-2

# Group Information
Group A6
- Group Leader: Victoria Taylor
- Conceptual Modeler: Yahia Abdelkarim
- Database Designer: Rishi Patel
- Data Wrangler: Enaz Tewfik
- SQL Writer: Kevin Serna

# Case Summary
Northline Outfitters is a small online retail company that sells lifestyle and tech accessories, including products such as hoodies, water bottles, desk lamps, phone cases, keyboards, mouse pads, and backpacks. The company purchases merchandise from outside vendors and sells directly to customers in the United States and Canada. Because the business is still growing, its operational records were originally stored in Excel rather than in a normalized relational database. The project instructions specifically describe the source files as messy, partially duplicated, unnormalized, and inconsistent, with issues such as mixed date formats, country indicators embedded in IDs, metric and imperial measurements, combined customer information, inconsistent tax/discount formats, and duplicate-looking product rows.

The initial data was provided in two spreadsheets: Sales_Dump and Product_Supplier_Master. Sales_Dump combined order, line item, customer, employee, manager, product, discount, tax, shipping, and return information into a single table. Product_Supplier_Master combined product, vendor, vendor representative, pricing, measurement, and product-family information into one table. Because these files mixed multiple business concepts into flat spreadsheets, the same information appeared repeatedly across many rows, which increased the risk of update errors, inconsistent values, and inaccurate query results.

To resolve these issues, our final model separates the data into distinct relational entities: customer, order, lineitem, product, vendor, vendor_rep, employee, manager, and discount. This structure reduces redundancy and clarifies the relationships between the business objects. For example, each order is tied to one customer and one employee, each line item connects an order to a product, each product is supplied by a vendor, and vendor representatives are separated from vendor records. This final design keeps the project compact while still addressing the main data quality problems in the original spreadsheets.

# Conceptual Model

The PNG is attached under Project Model.png. The final model separates the original spreadsheet data into normalized entities for products, vendors, vendor representatives, customers, orders, line items, discounts, employees, and managers.

The `product` entity is central to the model because each order line references a specific product SKU. Each product is supplied by one vendor, and each vendor may have one or more vendor representatives. Product details such as description, category, cost, price, pack size, weight, length, discontinued status, and reorder level are stored in the product table rather than being repeated across sales rows.

Orders are stored separately from line items. Each `order` records order-level information such as sale date, employee, customer, payment method, shipping country, shipping destination, return flag, notes, tax rate, unit price, and currency. The `lineitem` table connects orders to products and stores line-specific details such as quantity, line total, and size or weight.

Each order is associated with one customer and one employee. Customers are stored separately with first name, last name, customer type, and email. Employees are connected to managers through the manager reference field. Managers are modeled separately rather than recursively because the manager records in this dataset are not also used as order-processing employee records.

Discounts are also separated into their own table. Each order may reference a discount by name when a discount applies, allowing discount percentages to be stored once instead of repeated inconsistently across many sales rows.

# Data Quality Assessment
The initial data had several major quality issues that needed to be fixed before it could be imported into a relational database.

First, the source files were highly unnormalized. In Sales_Dump, each sales line repeated order-level, customer-level, employee-level, manager-level, and product-level data. For example, the same customer name, email, employee reference, manager reference, payment method, and shipping information could appear on multiple rows. This made the data inefficient and created the possibility that the same real-world entity could be recorded differently in different rows. Our final model fixed this by separating order-level data into order, customer data into customer, employee and manager data into employee and manager, and transaction details into lineitem.

Second, customer information was embedded in a single text field instead of being stored in separate attributes. The customer_info column included names mixed with notes such as student status, loyalty status, or guest checkout status. Examples included formats such as Grace Hall | Student | US, Mason Rivera; Loyalty? Y, and Zoe Garcia / guest order. This made it difficult to query customers consistently. In the final model, customer first name, last name, type, and email were separated into the customer table.

Third, many fields used inconsistent formats. Dates appeared in multiple formats such as 10-11-2025, Oct 17 25, October 5 25, and 10 Sep 2025. Payment methods also varied, with values such as VISA, visa, MC, Mastercard, Debit, debit, Interac, AMEX, Apple Pay, and Cash. Country values were also inconsistent, including US, USA, CA, and Canada. These inconsistencies would make grouping and filtering unreliable unless standardized.

Fourth, numeric fields were frequently stored as text or mixed with currency symbols, percent signs, or words. In the sales data, unit_price values included entries such as USD 18.99 and CAD 46.99, while line_total sometimes included dollar signs and sometimes appeared as a plain number. Discounts appeared as 10%, student 10%, promo5, 5, 5%, and 0. Tax values appeared as 13%, 8.25%, HST 13%, 0.13, and 0.0825. In the product master file, cost and list price also mixed numeric values with currency-prefixed strings. These issues were fixed by separating currency into its own attribute and storing cost, price, tax rate, discount percentage, and totals in numeric decimal fields.

Fifth, product and SKU data had inconsistent capitalization and duplicate-looking entries. Some SKUs appeared in uppercase, such as SKU-C-1012, while others appeared in lowercase, such as sku-c-1012. The product master also contained alternate SKUs, parent SKUs, and variant rows such as “Mini,” “Student Edition,” or color-specific versions. Without cleaning, these values could cause the same product family or product variant to be treated inconsistently. The final product table standardizes product identifiers, keeps alt_sku and product notes where useful, and links each product to its vendor.

Sixth, category values were not standardized. Some rows used single categories such as Tech, School, or Accessories, while others used combined categories such as Desk Setup / Student, Accessories / Student, Tech & Student, Lifestyle , Student, and Student and apparels. These inconsistent labels would make category-based reporting inaccurate. Our final model stores a cleaned product category value in the product table so that category queries can be run more reliably.

Seventh, measurement fields mixed units and wording. Product and sales fields included values such as 11", 11 inches, 11 oz, 272 grams, 499 g, 0.22 kilograms, 1.1 pound, 25.4cm, and 26 centimetres. These values were not directly comparable. The final model resolves this by converting key measurements into standard numeric fields such as weight_g and length_cm.

Eighth, vendor and vendor representative data were repeated and inconsistently formatted. Vendor phone numbers appeared in multiple formats, such as 404-555-0181, (404)555-0181, and 604.555.0190. Vendor representative values sometimes included extra notes, such as Jason Wu / email missing or Anika Roy / email missing. In the final model, vendor information was separated into vendor, while representative names were split into first and last name fields in vendor_rep.

Ninth, several fields had missing values. In the sales data, some rows were missing customer email, tax, line total, return flag, ship country, size/weight, or notes. In the product master file, some rows were missing alternate SKU, reorder level, pack size, weight, length, discontinued status, parent SKU, or notes. Some missing values were acceptable because they represented optional information, but others required cleaning or default handling before import. For example, missing return flags needed to be interpreted carefully so they would not be confused with confirmed non-returned items.

Overall, the initial spreadsheets were useful as raw business exports but were not reliable enough for direct relational database use. The major improvements in the final model were normalization, standardization of identifiers and categories, conversion of text-based numeric values into proper decimal or integer fields, separation of embedded customer and vendor representative information, and clearer foreign-key relationships among orders, line items, products, vendors, employees, managers, customers, and discounts.

# Data Cleaning Process
# Data Cleaning Process

To transform the original Northline Outfitters spreadsheets into a database-ready format, our group focused on standardization, attribute splitting, data type conversion, and normalization.

We first addressed date and location inconsistencies. Sale dates appeared in multiple formats, including text-based dates and numeric date formats, so we converted them into a consistent SQL-compatible date format. We also standardized country values so that variations such as `USA`, `US`, `Canada`, and `CA` could be stored consistently and queried reliably.

Next, we split fields that originally contained multiple pieces of information. The `customer_info` field often combined customer names with notes such as student, loyalty, or guest status. We separated this information into customer first name, last name, and customer type. We also cleaned vendor representative information by separating representative names into first and last name fields and moving vendor contact information into the vendor-related tables.

We then cleaned financial and numeric fields so they could be used in calculations. Prices, costs, taxes, discounts, and line totals sometimes included currency labels, symbols, percent signs, or text descriptions. We removed non-numeric text where needed, stored currency separately, and converted fields such as cost, unit price, list price, tax rate, discount percentage, and line total into numeric formats. Quantity values were also converted into integers.

Product records required additional standardization. SKU values were made consistent, duplicate-looking product rows were reviewed, and product descriptions, categories, vendor names, prices, reorder levels, discontinued flags, and measurement fields were cleaned before import. Category values were standardized so that similar labels would not be treated as separate categories during reporting.

Finally, we standardized measurement fields. Weight and length values appeared in mixed metric and imperial formats, so we converted the main product measurements into consistent numeric fields such as `weight_g` and `length_cm`. These cleaning steps allowed the original flat spreadsheet data to be imported into a structured relational model with fewer redundancies and more reliable query results.

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
The revenue per product metric is particularly valuable because it shows vendor efficiency. Vendors with high revenue per product may have stronger product selection and better market fit, while vendors with low revenue per product may be supplying too many slow-moving items.

### Technical Justification
This query demonstrates aggregation using COUNT(DISTINCT) for counting unique products and orders, along with SUM(), AVG(), and ROUND() functions for revenue calculations. It includes a computed column (revenue_per_product) that divides total revenue by product count, showing understanding of arithmetic operations within SELECT statements. The query uses a four-table join (vendor, product, lineitem, order) to combine supplier information with actual sales data, and applies GROUP BY to aggregate results at the vendor level. It reinforces sorting by computed columns and demonstrates how to analyze business performance across different suppliers.
