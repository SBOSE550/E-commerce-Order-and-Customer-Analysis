# E-commerce-Order-and-Customer-Analysis
----------------------------------------------
## Overview
This project aims to analyze various aspects of an e-commerce business using SQL. The dataset includes tables related to orders, customers, products, payments, sellers, order items and locations. By analyzing this data, we aim to derive meaningful insights that can help in decision-making and improving business strategies.

## Dataset
The dataset consists of the following tables:

- **orders:** Information about each order placed, including order status and timestamps.
- **order_items:** Details of items included in each order, including prices and seller information.
- **products:** Information about the products available in the store.
- **customers:** Information about the customers who placed orders.
- **payments:** Details of payments made for each order.
- **geolocation:** Geographical information related to customer locations.

## Business Problem
The primary goal is to understand and improve various aspects of the e-commerce business, including:

1. Identifying top-selling products and analyzing sales revenue.
2. Segmenting customers based on their purchase behavior.
3. Analyzing preferred payment methods and their impact on order value.
4. Evaluating shipping performance and identifying areas for improvement.
5. Understanding sales trends over time.
6. Identifying regions with the highest sales and revenue.
7. Assessing order fulfillment performance.
   
## Solutions and Insights:
### 1. Order and Product Analysis
**Problem:** Identify the top 10 products by sales revenue. For each product, calculate the total sales, average sales per order, and the number of orders.

**SQL Steps:**

-Joined `order_items` and `products` tables.
-Grouped by `product ID` and `category`.
-Calculated `total sales`, `average sales`, and `number of orders`.
-Ordered by `total sales` in descending order and selected the top 10 products.

**SQL Query:**
```sql

SELECT TOP 10 P.product_id,P.product_category,Round(SUM(o.price),2) as total_salse,
ROUND(AVG(price),2) as avg_salse,COUNT(order_id) as num_orders
FROM order_items as O
JOIN products as P
ON o.product_id=P.product_id
Group By P.product_id,product_category
ORDER BY total_salse DESC
```
**Results**
product_id                                         product_category                                   total_salse            avg_salse              num_orders
-------------------------------------------------- -------------------------------------------------- ---------------------- ---------------------- -----------
bb50f2e236e5eea0100680137654686c                   HEALTH BEAUTY                                      63885                  327.62                 195
6cdd53843498f92890544667809f1595                   HEALTH BEAUTY                                      54730.2                350.83                 156
d6160fb7873f184099d9bc95e30376af                   PCs                                                48899.34               1397.12                35
d1c427060a0f73f6b889a5c7c61f2ac4                   computer accessories                               47214.51               137.65                 343
99a4788cb24856965c36a24e339b6058                   bed table bath                                     43025.56               88.17                  488
3dd2a17168ec895c781a9191c1e95ad7                   computer accessories                               41082.6                149.94                 274
25c38557cf793876c5abdd5931f922db                   babies                                             38907.32               1023.88                38
5f504b3a1c75b73d6151be81eb05bdc9                   Cool Stuff                                         37733.9                598.95                 63
53b36df67ebb7c41585e8d54d6772e08                   Watches present                                    37683.42               116.67                 323
aca2eb7d00ea1a7b8ebd4e68314663af                   Furniture Decoration                               37608.9                71.36                  527

#### Insight:
The top 10 products by sales revenue were identified, providing insight into which products are driving the most revenue.

2. Customer Segmentation
Problem: Segment customers based on their purchase frequency and total spending.

SQL Steps:

Joined orders and order_items tables.
Calculated total spending and purchase frequency for each customer.
Classified customers into 'Low', 'Medium', and 'High' segments based on total spending.
Insight:

Customers were segmented into different spending categories, allowing for targeted marketing and personalized offers.
3. Payment Method Analysis
Problem: Analyze the preferred payment methods of customers.

SQL Steps:

Joined order_items and payments tables.
Grouped by payment type.
Calculated total orders, total payment value, average order value, and the range of order values.
Insight:

The most used payment methods were identified along with their correlation to order values, helping to understand customer payment preferences.
4. Shipping Performance
Problem: Calculate the average shipping time for each seller and identify the sellers with the fastest and slowest shipping times.

SQL Steps:

Joined order_items and orders tables.
Calculated the average shipping time for each seller.
Ranked sellers based on their shipping times to identify the fastest and slowest performers.
Insight:

Sellers with the best and worst shipping performance were identified, highlighting areas for potential improvement in the supply chain.
5. Sales Trend Analysis
Problem: Analyze monthly sales trends for the last two years.

SQL Steps:

Extracted month and year from the order_approved_at column.
Calculated total orders, total sales, average sales, and the range of sales values for each month.
Insight:

Monthly sales trends and seasonal patterns were identified, providing insights into sales performance over time.
6. Customer Demographics
Problem: Identify the regions with the highest number of orders and the highest revenue.

SQL Steps:

Joined customers, orders, and order_items tables.
Grouped by customer state and city.
Calculated total orders and total revenue for each region.
Insight:

Regions with the highest number of orders and revenue were identified, helping to focus marketing and sales efforts in these areas.
7. Order Fulfillment Analysis
Problem: Calculate the percentage of orders fulfilled on time.

SQL Steps:

Calculated the total number of orders and the number of orders delivered on or before the estimated delivery date.
Computed the percentage of on-time deliveries.
Insight:

The percentage of orders fulfilled on time was calculated, providing a measure of the company's delivery performance.
Conclusion
By using SQL to analyze the e-commerce dataset, we derived valuable insights into product sales, customer behavior, payment preferences, shipping performance, sales trends, regional sales distribution, and order fulfillment performance. These insights can help the business make informed decisions to improve operations, enhance customer satisfaction, and drive revenue growth.

How to Use
Clone this repository.
Import the provided SQL script into your preferred SQL environment.
Execute the queries step by step to generate the insights.
Feel free to explore and modify the queries to fit your specific needs and dataset.
