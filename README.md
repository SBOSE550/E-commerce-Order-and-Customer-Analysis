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
- **sellers:** Information about the sellers who salse the products.
### Dataset Schema:
![image](https://github.com/SBOSE550/E-commerce-Order-and-Customer-Analysis/assets/98967373/5c3ffad9-5891-4a5c-9958-4dab06ca648d)
For more information about the dataset go to this link:[https://www.kaggle.com/datasets/devarajv88/target-dataset?select=sellers.csv](url)
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
#### Problem:
Identify the top 10 products by sales revenue. For each product, calculate the total sales, average sales per order, and the number of orders.

#### SQL Steps:

- Joined `order_items` and `products` tables.
- Grouped by **product ID** and **category**.
- Calculated **total sales, average sales, and number of orders**.
- Ordered by **total sales** in descending order and selected the top 10 products.

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
| product_id                           | product_category        | total_sales | avg_sales | num_orders |
|--------------------------------------|-------------------------|-------------|-----------|------------|
| bb50f2e236e5eea0100680137654686c     | HEALTH BEAUTY           | 63885       | 327.62    | 195        |
| 6cdd53843498f92890544667809f1595     | HEALTH BEAUTY           | 54730.2     | 350.83    | 156        |
| d6160fb7873f184099d9bc95e30376af     | PCs                     | 48899.34    | 1397.12   | 35         |
| d1c427060a0f73f6b889a5c7c61f2ac4     | computer accessories    | 47214.51    | 137.65    | 343        |
| 99a4788cb24856965c36a24e339b6058     | bed table bath          | 43025.56    | 88.17     | 488        |
| 3dd2a17168ec895c781a9191c1e95ad7     | computer accessories    | 41082.6     | 149.94    | 274        |
| 25c38557cf793876c5abdd5931f922db     | babies                  | 38907.32    | 1023.88   | 38         |
| 5f504b3a1c75b73d6151be81eb05bdc9     | Cool Stuff              | 37733.9     | 598.95    | 63         |
| 53b36df67ebb7c41585e8d54d6772e08     | Watches present         | 37683.42    | 116.67    | 323        |
| aca2eb7d00ea1a7b8ebd4e68314663af     | Furniture Decoration    | 37608.9     | 71.36     | 527        |


#### Insight:
The top 10 products by sales revenue were identified, providing insight into which products catagory are driving the most revenue.

### 2. Customer Segmentation
#### Problem:
Segment customers based on their purchase frequency and total spending.

#### SQL Steps:

- Joined `orders` and `order_items` tables.
- Calculated **total spending** and **purchase frequency** for each customer.
- Classified customers into `'Low', 'Medium', and 'High'` segments based on **total spending**.

#### SQL Query:
```sql

WITH spending_frequency as (
SELECT o.customer_id,ROUND(SUM(oi.price),2) as Total_spending,Count(oi.order_id) as purchase_frequency
FROM orders as o
JOIN order_items as oi
ON o.order_id=oi.order_id
Group by o.customer_id
)
SELECT TOP 10 *,
Case 
WHEN Total_spending<=100 Then 'Low'
WHEN Total_spending BETWEEN 101 AND 500 THEN 'Medium'
WHEN Total_spending >500  THEN 'High'
END as Customer_segment
FROM spending_frequency
```
#### Results:
| customer_id                        | Total_spending | purchase_frequency | Customer_segment |
|------------------------------------|----------------|--------------------|------------------|
| 00012a2ce6f8dcda20d059ce98491703   | 89.8           | 1                  | Low              |
| 000161a058600d5901f007fab4c27140   | 54.9           | 1                  | Low              |
| 0001fd6190edaaf884bcaf3d49edf079   | 179.99         | 1                  | Medium           |
| 0002414f95344307404f0ace7a26f1d5   | 149.9          | 1                  | Medium           |
| 000379cdec625522490c315e70c7a9fb   | 93             | 1                  | Low              |
| 0004164d20a9e969af783496f3408652   | 59.99          | 1                  | Low              |
| 000419c5494106c306a97b5635748086   | 34.3           | 1                  | Low              |
| 00046a560d407e99b969756e0b10f282   | 120.9          | 1                  | Medium           |
| 00050bf6e01e69d5c0fd612f1bcfb69c   | 69.99          | 1                  | Low              |
| 000598caf2ef4117407665ac33275130   | 1107           | 1                  | High             |

  
#### Insight:

Customers were segmented into different spending categories, allowing for targeted marketing and personalized offers.

### 3. Payment Method Analysis

#### Problem:
Analyze the preferred payment methods of customers.

#### SQL Steps:

- Joined `order_items` and `payments` tables.
- Grouped by **payment type**.
- Calculated **total orders, total payment value, average order value, and the range of order values**.

#### SQL Query:
```sql
SELECT payment_type,COUNT(p.order_id) as Total_orders,
ROUND(SUM(payment_value),2) as Total_payment_value,
ROUND(AVG(oi.price),2) as avg_order_value,
ROUND(MAX(oi.price),2) as max_order_value,
ROUND(MIN(oi.price),2) as min_order_value
FROM order_items as oi
JOIN payments as p
ON oi.order_id=p.order_id
GROUP BY p.payment_type
ORDER BY  Total_orders DESC
```
#### Results:
| payment_type   | Total_orders | Total_payment_value | avg_order_value | max_order_value | min_order_value |
|----------------|--------------|---------------------|-----------------|-----------------|-----------------|
| credit_card    | 86769        | 15589028.22         | 126.48          | 6735            | 0.85            |
| UPI            | 22867        | 4059699.6           | 104.58          | 6729            | 0.85            |
| voucher        | 6274         | 405873.03           | 105.11          | 3124            | 2.2             |
| debit_card     | 1691         | 253533.86           | 108.67          | 4059            | 5.99            |

#### Insight:

The most used payment methods were identified along with their correlation to order values, helping to understand customer payment preferences.

### 4. Shipping Performance
#### Problem:
Calculate the average shipping time for each seller and identify the sellers with the fastest and slowest shipping times.

#### SQL Steps:

- Joined `order_items` and `orders` tables.
- Calculated the **average shipping time** for each seller and store it in `avg_shipping` CTE.
- Ranked sellers based on their **average shipping time** to identify the fastest and slowest performers.

#### SQL Query:
```sql
WITH avg_shipping as(
SELECT seller_id,
AVG(DATEDIFF(day,order_approved_at,shipping_limit_date)) as avg_shipping_time
FROM order_items as oi
JOIN orders as o
ON oi.order_id = o.order_id
WHERE order_approved_at IS NOT NULL 

GROUP BY seller_id
), rank_seller as (
SELECT seller_id,avg_shipping_time,
RANK() OVER (ORDER BY avg_shipping_time ASC) AS rank_fastest,
RANK() OVER (ORDER BY avg_shipping_time DESC) AS rank_slowest
FROM avg_shipping

)
SELECT seller_id,avg_shipping_time,
CASE
WHEN rank_fastest = 1 THEN 'Fastest'
WHEN rank_slowest = 1 THEN 'Slowest'
END as shipping_performance
FROM rank_seller
WHERE rank_fastest=1 OR rank_slowest=1
```
#### Results:
| seller_id                          | avg_shipping_time | shipping_performance |
|------------------------------------|-------------------|----------------------|
| 88af55b4a7ca402b27df16f7c7c9b5d2   | 148               | Slowest              |
| 2bdb95a56a36ebbc6640337ac5eac174   | 0                 | Fastest              |
| 96e5dc09087bad639b4ee193104ec2e5   | 0                 | Fastest              |

#### Insight:

Sellers with the best and worst shipping performance were identified, highlighting areas for potential improvement in the supply chain.

### 5. Sales Trend Analysis
#### Problem:
Analyze monthly sales trends for the last two years.

#### SQL Steps:

- Extracted month and year from the **order_approved_at** column and storing it in `month_CTE`.
- Joining `month_CTE` with the `payments` table to Calculate **total orders, total sales, average sales, and the range of sales values** for each month.

#### SQL Query:
```sql
WITH month_CTE AS(
SELECT order_id,
MONTH(order_approved_at) as month_perchase,
YEAR(order_approved_at) as year_salse
FROM orders
WHERE order_status ='delivered' OR order_status ='shipped'
)
SELECT m.year_salse,m.month_perchase,
COUNT(m.order_id) as Total_orders,
ROUND(SUM(p.payment_value),2) as Total_salse,
ROUND(AVG(p.payment_value),2) as avg_salse,
ROUND(MAX(p.payment_value),2) as max_salse,
ROUND(MIN(p.payment_value),2) as min_salse
FROM month_CTE as m
JOIN payments as p
ON m.order_id=p.order_id
WHERE m.year_salse IS NOT NULL
GROUP BY m.year_salse,month_perchase
ORDER BY m.year_salse,month_perchase
```
#### Results:
| year_salse | month_perchase | Total_orders | Total_salse | avg_salse | max_salse | min_salse |
|------------|----------------|--------------|-------------|-----------|-----------|-----------|
| 2016       | 10             | 292          | 47949.69    | 164.21    | 1423.55   | 0.74      |
| 2016       | 12             | 1            | 19.62       | 19.62     | 19.62     | 19.62     |
| 2017       | 1              | 779          | 124912.05   | 160.35    | 3016.01   | 0.96      |
| 2017       | 2              | 1759         | 275072.64   | 156.38    | 6929.31   | 0.28      |
| 2017       | 3              | 2753         | 415381.98   | 150.88    | 4016.91   | 0.13      |
| 2017       | 4              | 2491         | 394376.55   | 158.32    | 4950.34   | 0         |
| 2017       | 5              | 3844         | 577282.02   | 150.18    | 6726.66   | 0         |
| 2017       | 6              | 3377         | 500424.98   | 148.19    | 3048.27   | 0         |
| 2017       | 7              | 4159         | 568845.01   | 136.77    | 3041.73   | 0.2       |
| 2017       | 8              | 4461         | 651800.6    | 146.11    | 2480.58   | 0.01      |
| 2017       | 9              | 4432         | 700424.16   | 158.04    | 2154.82   | 0.23      |
| 2017       | 10             | 4694         | 761767.01   | 162.29    | 13664.08  | 0         |
| 2017       | 11             | 7516         | 1143587.53  | 152.15    | 6081.54   | 0.03      |
| 2017       | 12             | 5962         | 875820.5    | 146.9     | 2734.66   | 0.17      |
| 2018       | 1              | 7351         | 1081929.63  | 147.18    | 3826.8    | 0         |
| 2018       | 2              | 6811         | 970286.21   | 142.46    | 3358.24   | 0.23      |
| 2018       | 3              | 7518         | 1152640.69  | 153.32    | 4175.26   | 0.14      |
| 2018       | 4              | 6995         | 1130030.36  | 161.55    | 3526.46   | 0.01      |
| 2018       | 5              | 7260         | 1163498.22  | 160.26    | 3979.55   | 0.03      |
| 2018       | 6              | 6391         | 1024204.87  | 160.26    | 4681.78   | 0.05      |
| 2018       | 7              | 6321         | 1014903.59  | 160.56    | 7274.88   | 0.01      |
| 2018       | 8              | 6740         | 1022396.76  | 151.69    | 4513.32   | 0.31      |
| 2018       | 9              | 1            | 166.46      | 166.46    | 166.46    | 166.46    |

#### Insight:
Monthly sales trends and seasonal patterns were identified, providing insights into sales performance over time.

### 6. Customer Demographics
#### Problem:
Identify the regions with the highest number of orders and the highest revenue.

#### SQL Steps:

- Joined `customers`, `orders`, and `order_items` tables.
- Grouped by **customer state and city**.
- Calculated **total orders and total revenue** for each region.

#### SQL Query:
```sql

WITH geo_city as (
SELECT c.customer_state,customer_city,
COUNT(o.order_id) as total_orders,ROUND(SUM(oi.price),2) as Total_revenue
FROM customers as c
JOIN orders as o
ON c.customer_id =o.customer_id
JOIN order_items as oi
ON o.order_id = oi.order_id
GROUP BY c.customer_state,customer_city
)
SELECT TOP 10 gl.*,gc.total_orders,Total_revenue
FROM geo_city as gc
JOIN geolocation as gl
ON gc.customer_state = gl.geolocation_state
AND gc.customer_city=gl.geolocation_city
ORDER BY total_orders DESC,Total_revenue DESC
```
#### Results:
| geolocation_zip_code_prefix | geolocation_lat      | geolocation_lng      | geolocation_city | geolocation_state | total_orders | Total_revenue |
|-----------------------------|----------------------|----------------------|------------------|-------------------|--------------|---------------|
| 1032                        | -23.5384181040741    | -46.6347783752667    | sao paulo        | SP                | 17808        | 1914924.54    |
| 1013                        | -23.5473251282244    | -46.6341837861389    | sao paulo        | SP                | 17808        | 1914924.54    |
| 1011                        | -23.5476395503206    | -46.6360316231549    | sao paulo        | SP                | 17808        | 1914924.54    |
| 1029                        | -23.5437690557691    | -46.6342778408513    | sao paulo        | SP                | 17808        | 1914924.54    |
| 1013                        | -23.5469232084367    | -46.6342636964915    | sao paulo        | SP                | 17808        | 1914924.54    |
| 1047                        | -23.5462731124127    | -46.6412251697155    | sao paulo        | SP                | 17808        | 1914924.54    |
| 1035                        | -23.5415779617115    | -46.6416072232961    | sao paulo        | SP                | 17808        | 1914924.54    |
| 1041                        | -23.5443921648681    | -46.6394993062784    | sao paulo        | SP                | 17808        | 1914924.54    |
| 1046                        | -23.5461289664147    | -46.6429514836114    | sao paulo        | SP                | 17808        | 1914924.54    |
| 1046                        | -23.5460811270355    | -46.6448202983716    | sao paulo        | SP                | 17808        | 1914924.54    |

#### Insight:
Regions with the highest number of orders and revenue were **Sao Paulo City in SP state**, helping to focus marketing and sales efforts in these areas.

### 7. Order Fulfillment Analysis
#### Problem:
Calculate the percentage of orders fulfilled on time.

#### SQL Steps:

- Calculated the **total number of orders** and the number of orders delivered on or before the estimated delivery date.
- Computed the percentage of **on-time deliveries**.
#### SQL Query:
```sql
WITH on_time_cte as (
SELECT COUNT(order_id) as Total_orders,
(SELECT COUNT(order_id)
FROM orders
WHERE order_estimated_delivery_date>order_delivered_customer_date
AND order_delivered_customer_date IS NOT NULL) as on_time_orders
FROM orders
WHERE order_delivered_customer_date IS NOT NULL
)
SELECT *,
on_time_orders*100/Total_orders as on_time_percentage
FROM on_time_cte
```
#### Results:
| Total_orders | on_time_orders | on_time_percentage |
|--------------|----------------|--------------------|
| 96476        | 88649          | 91                 |

#### Insight:
**91%** of total orders have been delivered on time, demonstrating the company's strong delivery performance.

## Conclusion
By using SQL to analyze the e-commerce dataset, we derived valuable insights into product sales, customer behavior, payment preferences, shipping performance, sales trends, regional sales distribution, and order fulfillment performance. These insights can help the business make informed decisions to improve operations, enhance customer satisfaction, and drive revenue growth.

## How to Use
- Clone this repository.
- Import the provided SQL script into your preferred SQL environment.
- Execute the queries step by step to generate the insights.
Feel free to explore and modify the queries to fit your specific needs and dataset.
