# Subqueries

## SQL Task Links

| Task Number | Task Description                                                 | Link                                   |
|-------------|-----------------------------------------------------------------|----------------------------------------|
| Task 1      | Calculate the average number of orders for all users           | [View Task 1](#task-1)                |
| Task 2      | Repeat the previous task using the WITH operator                | [View Task 2](#task-2)                |
| Task 3      | Output all products except the cheapest one                     | [View Task 3](#task-3)                |
| Task 4      | Output products priced more than the average price + 20        | [View Task 4](#task-4)                |
| Task 5      | Count unique clients who placed an order in the last week      | [View Task 5](#task-5)                |
| Task 6      | Determine the age of the youngest male courier                 | [View Task 6](#task-6)                |
| Task 7      | Select non-canceled orders by ID                                 | [View Task 7](#task-7)                |
| Task 8      | Calculate the number of orders and average orders for users     | [View Task 8](#task-8)                |
| Task 9      | Apply discounts to products based on price                      | [View Task 9](#task-9)                |
| Task 10     | Count orders accepted but not created                            | [View Task 10](#task-10)              |
| Task 11     | Count delivered orders that were accepted but not delivered     | [View Task 11](#task-11)              |
| Task 12     | Count canceled and delivered orders                              | [View Task 12](#task-12)              |
| Task 13     | Count undelivered orders and canceled orders                    | [View Task 13](#task-13)              |
| Task 14     | Select male users older than all female users                   | [View Task 14](#task-14)              |
| Task 15     | Output the last 100 delivered orders                             | [View Task 15](#task-15)              |
| Task 16     | Output couriers who delivered 30 or more orders                 | [View Task 16](#task-16)              |
| Task 17     | Calculate average order size for canceled orders                 | [View Task 17](#task-17)              |
| Task 18     | Calculate the age of each user                                   | [View Task 18](#task-18)              |
| Task 19     | Calculate delivery time for orders with more than 5 products    | [View Task 19](#task-19)              |
| Task 20     | Count first orders made by users by date                        | [View Task 20](#task-20)              |
| Task 21     | Select all columns from orders and unnest product IDs           | [View Task 21](#task-21)              |
| Task 22     | Determine the 10 most popular products                          | [View Task 22](#task-22)              |
| Task 23     | Select orders containing the 5 most expensive products          | [View Task 23](#task-23)              |


## Task 1

### Description:
Using data from the `user_actions` table, calculate the average number of orders made by all users of our service. First, count how many orders each user made in a subquery, then use this result in the main query to average the number of orders across all users. Round the resulting average to two decimal places. Name the column with this value as `orders_avg`.

### Resulting Table Fields:
- **orders_avg**

### SQL Query:
```sql
SELECT ROUND(AVG(orders_count), 2) AS orders_avg
FROM   (SELECT user_id,
               COUNT(order_id) AS orders_count
        FROM   user_actions
        WHERE  action = 'create_order'
        GROUP BY user_id) AS t1;
```
### Explanation:
This query calculates the average number of orders made by users by first counting the orders per user and then averaging those counts, rounded to two decimal places.

## Task 2
### Description:
Repeat the previous query but now use the WITH operator and a common table expression (CTE). Calculate the average number of orders made by all users of our service. Round the resulting average to two decimal places. Name the column with this value as orders_avg.

### Resulting Table Fields:
orders_avg
### SQL Query:
```sql
Copy code
WITH t1 AS (SELECT user_id,
                   COUNT(order_id) AS orders_count
            FROM   user_actions
            WHERE  action = 'create_order'
            GROUP BY user_id)
SELECT ROUND(AVG(orders_count), 2) AS orders_avg
FROM   t1;
```
### Explanation:
This query achieves the same goal as Task 1 but uses a common table expression to simplify the subquery. It calculates the average number of orders made by users, rounded to two decimal places.

## Task 3
### Description:
Output information about all products except for the cheapest one from the products table. Sort the result by product ID in descending order.

### Resulting Table Fields: 
product_id
name
price
### SQL Query:
```sql
SELECT product_id,
       name,
       price
FROM   products
WHERE  price != (SELECT MIN(price)
                 FROM   products)
ORDER BY product_id DESC;
```
### Explanation:
This query retrieves all products from the products table while excluding the product with the lowest price. The results are sorted by product ID in descending order.

## Task 4
### Description:
Output information about products from the products table whose price exceeds the average price of all products by 20 rubles or more. Sort the result by product ID in descending order.

### Resulting Table Fields:
product_id
name
price
### SQL Query:
```sql
SELECT product_id,
       name,
       price
FROM   products
WHERE  price >= (SELECT AVG(price)
                 FROM   products) + 20
ORDER BY product_id DESC;
```
### Explanation:
This query retrieves products whose prices are at least 20 rubles higher than the average price of all products. The results are sorted by product ID in descending order.

## Task 5
### Description:
Count the number of unique clients in the user_actions table who placed at least one order in the last week. Name the resulting column as users_count. Use the latest date from the user_actions table as the reference date to calculate the week.

### Resulting Table Fields:
users_count
### SQL Query:
```sql
SELECT COUNT(DISTINCT user_id) AS users_count
FROM   user_actions
WHERE  action = 'create_order'
   AND time >= (SELECT MAX(time)
                FROM   user_actions) - INTERVAL '1 week';
```
### Explanation:
This query counts the distinct user IDs from the user_actions table that have placed orders in the last week, using the latest timestamp in the table to define the week.



## Task 6

### Description:
Using data from the `couriers` table, determine the age of the youngest male courier. In this calculation, use the latest date from the `courier_actions` table as the reference date. Convert the last date to the DATE format before applying the AGE function. Measure the age in years, months, and days, converting it to VARCHAR. Name the resulting column `min_age`.

### Resulting Table Fields:
- **min_age**

### SQL Query:
```sql
SELECT MIN(AGE((SELECT MAX(time)::DATE
                FROM   courier_actions), birth_date))::VARCHAR AS min_age
FROM   couriers
WHERE  sex = 'male';
```
### Explanation:
This query calculates the age of the youngest male courier by comparing their birth date with the latest delivery date from the courier_actions table. The result is formatted as a string showing years, months, and days.

## Task 7
### Description:
From the user_actions table, select all orders that were not canceled by users. Output the column with the IDs of these orders. Sort the result by order ID in ascending order. Include a LIMIT clause to display only the first 1000 rows of the resulting table.

### Resulting Table Fields:
order_id
### SQL Query:
```sql
SELECT order_id
FROM   user_actions
WHERE  order_id NOT IN (SELECT order_id
                        FROM   user_actions
                        WHERE  action = 'cancel_order')
ORDER BY order_id LIMIT 1000;
```
### Explanation:
This query retrieves the IDs of all orders from the user_actions table that were not canceled, excluding any order IDs associated with cancellation actions. The results are sorted in ascending order by order ID.

## Task 8
### Description:
Using data from the user_actions table, calculate how many orders each user has made and reflect this in the column orders_count. In a separate column orders_avg, indicate the average number of orders made by all users, rounded to two decimal places. Also, calculate the deviation of each user's order count from the average. Name the deviation column orders_diff. Sort the result by user ID in ascending order and include a LIMIT clause to display only the first 1000 rows of the resulting table.

### Resulting Table Fields:
user_id
orders_count
orders_avg
orders_diff
### SQL Query:
```sql
WITH t1 AS (SELECT user_id,
                   COUNT(order_id) AS orders_count
            FROM   user_actions
            WHERE  action = 'create_order'
            GROUP BY user_id)
SELECT user_id,
       orders_count,
       ROUND((SELECT AVG(orders_count) FROM t1), 2) AS orders_avg,
       orders_count - ROUND((SELECT AVG(orders_count) FROM t1), 2) AS orders_diff
FROM   t1
ORDER BY user_id LIMIT 1000;
```
### Explanation:
This query counts the number of orders per user and calculates the average order count for all users, rounding the average to two decimal places. It also computes the deviation of each user's count from the average, providing a comprehensive view of user activity.

## Task 9
### Description:
Apply a 15% discount to products priced more than the average price of all products by 50 rubles or more, and a 10% discount to products priced less than the average by 50 rubles or more. Leave the prices of products within the range (average - 50; average + 50) unchanged. Round the average price to two decimal places.

Output information about all products, including their old and new prices. Name the column with the new price new_price. Sort the results first by the old price in descending order, then by product ID in ascending order.

### Resulting Table Fields:
product_id
name
price
new_price
### SQL Query:
```sql
WITH avg_price AS (SELECT ROUND(AVG(price), 2) AS price
                   FROM   products)
SELECT product_id,
       name,
       price,
       CASE 
           WHEN price >= (SELECT * FROM avg_price) + 50 THEN price * 0.85 
           WHEN price <= (SELECT * FROM avg_price) - 50 THEN price * 0.90 
           ELSE price 
       END AS new_price
FROM   products
ORDER BY price DESC, product_id;
```
### Explanation:
This query applies different discounts to products based on their prices relative to the average price. It computes new prices according to specified conditions and provides both old and new prices for comparison.

## Task 10
### Description:
Determine if there are any orders in the courier_actions table that were accepted by couriers but were not created by users. Count the number of such orders and name the resulting column orders_count.

### Resulting Table Fields:
orders_count
### SQL Query:
```sql
Copy code
SELECT COUNT(DISTINCT order_id) AS orders_count
FROM   courier_actions
WHERE  order_id NOT IN (SELECT order_id
                        FROM   user_actions);
```
### Explanation:
This query counts distinct order IDs in the courier_actions table that do not exist in the user_actions table, indicating orders that were accepted by couriers without a corresponding creation record from users.



## Task 11

### Description:
Determine if there are orders in the `courier_actions` table that were accepted by couriers but were not delivered to users. Count the number of such orders and name the resulting column `orders_count`.

### Resulting Table Fields:
- **orders_count**

### SQL Query:
```sql
SELECT COUNT(order_id) AS orders_count
FROM   courier_actions
WHERE  order_id NOT IN (SELECT order_id
                        FROM   courier_actions
                        WHERE  action = 'deliver_order');
```
### Explanation:
This query counts distinct order IDs in the courier_actions table that do not appear in the same table with a 'deliver_order' action, indicating orders that were accepted but not delivered.

## Task 12
### Description:
Determine the number of canceled orders in the courier_actions table and check if there are any orders that were canceled by users but were still delivered. Count the number of such orders.

### Resulting Table Fields:
orders_canceled
orders_canceled_and_delivered
### SQL Query:
```sql
SELECT COUNT(DISTINCT order_id) AS orders_canceled,
       COUNT(order_id) FILTER (WHERE action = 'deliver_order') AS orders_canceled_and_delivered
FROM   courier_actions
WHERE  order_id IN (SELECT order_id
                    FROM   user_actions
                    WHERE  action = 'cancel_order');
```
### Explanation:
This query counts the total number of canceled orders and the number of those canceled orders that were subsequently delivered. It uses the FILTER clause for conditional counting.

### Task 13
### Description:
From the courier_actions and user_actions tables, determine the number of undelivered orders. Among these, count the number of canceled orders and the number of orders that have not been canceled (and are still in transit).

### Resulting Table Fields:
orders_undelivered
orders_canceled
orders_in_process
### SQL Query:
```sql
SELECT COUNT(DISTINCT order_id) AS orders_undelivered,
       COUNT(order_id) FILTER (WHERE action = 'cancel_order') AS orders_canceled,
       COUNT(DISTINCT order_id) - COUNT(order_id) FILTER (WHERE action = 'cancel_order') AS orders_in_process
FROM   user_actions
WHERE  order_id IN (SELECT order_id
                    FROM   courier_actions
                    WHERE  order_id NOT IN (SELECT order_id
                                            FROM   courier_actions
                                            WHERE  action = 'deliver_order'));
```
### Explanation:
This query counts the number of undelivered orders, along with how many of those were canceled and how many are still in process. It provides insight into the status of orders in the system.

## Task 14
### Description:
Select users of male gender from the users table who are older than all female users. Output their ID and birth date, sorting the result by user ID in ascending order.

### Resulting Table Fields:
user_id
birth_date
### SQL Query:
```sql
SELECT user_id,
       birth_date
FROM   users
WHERE  sex = 'male'
   AND birth_date < (SELECT MIN(birth_date)
                     FROM   users
                     WHERE  sex = 'female')
ORDER BY user_id;
```
### Explanation:
This query retrieves male users who are older than the youngest female user by comparing their birth dates. The results are sorted by user ID.

## Task 15
### Description:
Output the ID and contents of the last 100 delivered orders from the orders table. The contents of the orders are defined as the lists of product IDs included in the orders. Sort the result by order ID in ascending order.

### Resulting Table Fields:
order_id
product_ids
### SQL Query:
```sql
SELECT order_id,
       product_ids
FROM   orders
WHERE  order_id IN (SELECT order_id
                    FROM   courier_actions
                    WHERE  action = 'deliver_order'
                    ORDER BY time DESC LIMIT 100)
ORDER BY order_id;
```
### Explanation:
This query retrieves the last 100 delivered orders from the orders table by checking against the courier_actions for delivery actions, and it outputs the relevant product IDs.

## Task 16
### Description:
From the couriers table, output all information about couriers who delivered 30 or more orders in September 2022. Sort the results by courier ID in ascending order.

### Resulting Table Fields:
courier_id
birth_date
sex
### SQL Query:
```sql
SELECT courier_id,
       birth_date,
       sex
FROM   couriers
WHERE  courier_id IN (SELECT courier_id
                      FROM   courier_actions
                      WHERE  DATE_PART('month', time) = 9
                         AND DATE_PART('year', time) = 2022
                         AND action = 'deliver_order'
                      GROUP BY courier_id HAVING COUNT(DISTINCT order_id) >= 30)
ORDER BY courier_id;
```
### Explanation:
This query filters couriers based on their delivery count, only including those who delivered 30 or more orders in September 2022. The results are sorted by courier ID.

## Task 17
### Description:
Calculate the average size of orders canceled by male users. Round the average size to three decimal places. Name the resulting column avg_order_size.

### Resulting Table Fields:
avg_order_size
### SQL Query:
```sql
SELECT ROUND(AVG(ARRAY_LENGTH(product_ids, 1)), 3) AS avg_order_size
FROM   orders
WHERE  order_id IN (SELECT order_id
                    FROM   user_actions
                    WHERE  action = 'cancel_order'
                       AND user_id IN (SELECT user_id
                                       FROM   users
                                       WHERE  sex = 'male'));
```
### Explanation:
This query computes the average size of orders that were canceled by male users, filtering the relevant orders and calculating the average size of the product arrays.

## Task 18
### Description:
Calculate the age of each user in the users table. Measure age in full years based on the latest date in the user_actions table. For users without a birth date, provide the average age of the other users, rounded to the nearest whole number. Include user ID and age in the result, sorting by user ID in ascending order.

### Resulting Table Fields:
user_id
age
### SQL Query:
```sql
WITH users_age AS (SELECT user_id,
                          DATE_PART('year', AGE((SELECT MAX(time)
                                                  FROM   user_actions), birth_date)) AS age
                   FROM   users)
SELECT user_id,
       COALESCE(age, (SELECT ROUND(AVG(age))
                      FROM   users_age))::INTEGER AS age
FROM   users_age
ORDER BY user_id;
```
### Explanation:
This query calculates the age of each user based on the last action date, while substituting the average age for users lacking a birth date.

## Task 19
### Description:
For each order that contains more than 5 products, calculate the delivery time taken. Include the order ID, time the order was accepted by the courier, time the order was delivered, and the delivery time in minutes. Name the respective columns time_accepted, time_delivered, and delivery_time. Sort the results by order ID in ascending order.

### Resulting Table Fields:
order_id
time_accepted
time_delivered
delivery_time
### SQL Query:
```sql
SELECT order_id,
       MIN(time) AS time_accepted,
       MAX(time) AS time_delivered,
       (EXTRACT(EPOCH FROM MAX(time) - MIN(time)) / 60)::INTEGER AS delivery_time
FROM   courier_actions
WHERE  order_id IN (SELECT order_id
                    FROM   orders
                    WHERE  ARRAY_LENGTH(product_ids, 1) > 5)
   AND order_id NOT IN (SELECT order_id
                        FROM   user_actions
                        WHERE  action = 'cancel_order')
GROUP BY order_id
ORDER BY order_id;
```
### Explanation:
This query calculates the delivery time for orders with more than 5 products by determining the earliest and latest timestamps in the courier_actions table and calculating the difference in minutes.

## Task 20
### Description:
Count the number of first orders made by users for each date in the user_actions table. First orders are defined as the initial orders made by users on the service. Only include non-canceled orders in the calculations. Include two columns in the result: the date and the count of first orders on that date. Name the date column date and the first orders column first_orders. Sort the results by date in ascending order.

### Resulting Table Fields:
date
first_orders
### SQL Query:
```sql
SELECT first_order_date AS date,
       COUNT(user_id) AS first_orders
FROM   (SELECT user_id,
               MIN(time)::DATE AS first_order_date
        FROM   user_actions
        WHERE  order_id NOT IN (SELECT order_id
                                FROM   user_actions
                                WHERE  action = 'cancel_order')
        GROUP BY user_id) t
GROUP BY first_order_date
ORDER BY date;
```
### Explanation:
This query identifies the first order date for each user and counts how many first orders occurred on each date, providing a clear overview of user engagement over time.

## Task 21
### Description:
Select all columns from the orders table and additionally, specify the UNNEST function applied to the product_ids column as the last column, naming it product_id. There are no additional calculations required. Add a LIMIT clause to display only the first 100 rows of the resulting table.

### Resulting Table Fields:
creation_time
order_id
product_ids
product_id
### SQL Query:
```sql
SELECT creation_time,
       order_id,
       product_ids,
       UNNEST(product_ids) AS product_id
FROM   orders LIMIT 100;
```
### Explanation:
This query expands the product_ids array into individual rows, allowing for detailed inspection of each product associated with an order, and limits the output to the first 100 records for brevity.

## Task 22
### Description:
Using the UNNEST function, determine the 10 most popular products in the orders table. The most popular products are defined as those appearing most frequently in orders. If a product appears multiple times in one order (indicating multiple units purchased), this is also counted. Only include non-canceled orders in the calculations.

### Resulting Table Fields:
product_id
times_purchased
### SQL Query:
```sql
SELECT product_id,
       times_purchased
FROM   (SELECT UNNEST(product_ids) AS product_id,
               COUNT(*) AS times_purchased
        FROM   orders
        WHERE  order_id NOT IN (SELECT order_id
                                FROM   user_actions
                                WHERE  action = 'cancel_order')
        GROUP BY product_id
        ORDER BY times_purchased DESC LIMIT 10) t
ORDER BY product_id;
```
### Explanation:
This query calculates the frequency of each product across non-canceled orders and identifies the top 10 products based on their total purchase count.

## Task 23
### Description:
From the orders table, select the order ID and the contents of orders that include at least one of the five most expensive products available in the service. Sort the result by order ID in ascending order.

### Resulting Table Fields:
order_id
product_ids
### SQL Query:
```sql
WITH top_products AS (SELECT product_id
                      FROM   products
                      ORDER BY price DESC LIMIT 5),
     unnest AS (SELECT order_id,
                       product_ids,
                       UNNEST(product_ids) AS product_id
                FROM   orders)
SELECT DISTINCT order_id,
                product_ids
FROM   unnest
WHERE  product_id IN (SELECT product_id
                      FROM   top_products)
ORDER BY order_id;
```
### Explanation:
This query identifies orders containing any of the top 5 most expensive products and outputs their IDs along with the associated product IDs, offering insight into high-value transactions.










