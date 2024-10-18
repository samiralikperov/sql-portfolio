## JOIN

## SQL Task Links

|               |                 |               |             |                |
|-----------------------|-----------------------|-----------------------|-----------------------|-----------------------|
| [Task 1](#task-1)     | [Task 2](#task-2)     | [Task 3](#task-3)     | [Task 4](#task-4)     | [Task 5](#task-5)     |
| [Task 6](#task-6)     | [Task 7](#task-7)     | [Task 8](#task-8)     | [Task 9](#task-9)     | [Task 10](#task-10)   |
| [Task 11](#task-11)   | [Task 12](#task-12)   | [Task 13](#task-13)   | [Task 14](#task-14)   | [Task 15](#task-15)   |
| [Task 16](#task-16)   | [Task 17](#task-17)   | [Task 18](#task-18)   | [Task 19](#task-19)   |[Task 20](#task-20)   |
| [Task 21](#task-21)   |                   




## Task 1

### Description:
Join the `user_actions` and `users` tables on the `user_id` key. Include two columns with `user_id` from both tables, naming them `user_id_left` and `user_id_right`. Also, include the columns `order_id`, `time`, `action`, `sex`, `birth_date`. Sort the resulting table by ascending user ID (from either of the two columns).

### SQL Query:
```sql
SELECT a.user_id AS user_id_left,
       b.user_id AS user_id_right,
       order_id,
       time,
       action,
       sex,
       birth_date
FROM   user_actions a 
JOIN   users b USING (user_id)
ORDER BY user_id_left;
```
### Explanation:
This query combines the user_actions and users tables using a JOIN on the user_id column. It retrieves user IDs from both tables, renaming them for clarity, and sorts the results by the left table's user_id.

## Task 2
### Description:
Join the user_actions and users tables on the user_id key. Instead of retrieving all user IDs, count the unique user_id values from one of the columns. Output this count as users_count.

### SQL Query:
```sql
SELECT COUNT(DISTINCT a.user_id) AS users_count
FROM   user_actions a 
JOIN   users b USING (user_id);
```
### Explanation:
This query combines the user_actions and users tables using a JOIN on the user_id column. It counts the distinct user IDs from the left table (user_actions) and outputs the result as users_count, providing the total number of unique users in the combined dataset.

## Task 3
### Description:
Use a LEFT JOIN to combine the user_actions and users tables on the user_id key. Note the order of the tables: the left table is user_actions and the right table is users. Include two columns with user_id from both tables, naming them user_id_left and user_id_right. Also, include the columns order_id, time, action, sex, birth_date. Sort the resulting table by ascending user ID from the left table.

### SQL Query:
```sql
SELECT a.user_id AS user_id_left,
       b.user_id AS user_id_right,
       order_id,
       time,
       action,
       sex,
       birth_date
FROM   user_actions a
LEFT JOIN users b USING (user_id)
ORDER BY user_id_left;
```
### Explanation:
This query combines the user_actions and users tables using a LEFT JOIN on the user_id column. It retrieves user IDs from both tables, renaming them for clarity, and sorts the results by the left table's user_id, ensuring that all entries from user_actions are included, even if there are no matching entries in the users table.

## Task 4
Description:
Modify the previous query to count the unique user_id values from the left table (user_actions) after performing a LEFT JOIN with the users table. Output this count as users_count.

### SQL Query:
```sql
SELECT COUNT(DISTINCT a.user_id) AS users_count
FROM   user_actions a
LEFT JOIN users b USING (user_id);
```
### Explanation:
This query uses a LEFT JOIN to combine the user_actions and users tables on the user_id column. It counts the distinct user_id values from the left table (user_actions) and outputs the result as users_count, providing the total number of unique users from the user_actions table, regardless of whether they have corresponding entries in the users table.

## Task 5
### Description:
Take the previous query that combines the user_actions and users tables using a LEFT JOIN. Add a WHERE clause to exclude NULL values in the user_id column from the right table (users). Include all the same columns and sort the resulting table by ascending user ID from the left table.

### SQL Query:
```sql

SELECT a.user_id AS user_id_left,
       b.user_id AS user_id_right,
       order_id,
       time,
       action,
       sex,
       birth_date
FROM   user_actions a
LEFT JOIN users b USING (user_id)
WHERE  b.user_id IS NOT NULL
ORDER BY user_id_left;
```
### Explanation:
This query combines the user_actions and users tables using a LEFT JOIN on the user_id column. The WHERE clause filters out any rows where the user_id from the right table (users) is NULL, effectively converting the LEFT JOIN into an INNER JOIN for this case. It retrieves user IDs from both tables and sorts the results by the left table's user_id, ensuring only matching records are included in the final output.

## Task 6
### Description:
Use a FULL JOIN to combine the results of the two subqueries based on the birth_date key. Include two columns with birth_date from both tables, naming them users_birth_date and couriers_birth_date. Also, include the columns for the counts of users and couriers as users_count and couriers_count. Sort the resulting table first by users_birth_date in ascending order and then by couriers_birth_date in ascending order.

### SQL Query:
```sql

SELECT a.birth_date AS users_birth_date,
       users_count,
       b.birth_date AS couriers_birth_date,
       couriers_count
FROM   (SELECT birth_date,
               COUNT(user_id) AS users_count
        FROM   users
        WHERE  birth_date IS NOT NULL
        GROUP BY birth_date) a 
FULL JOIN (SELECT birth_date,
                   COUNT(courier_id) AS couriers_count
            FROM   couriers
            WHERE  birth_date IS NOT NULL
            GROUP BY birth_date) b 
USING (birth_date)
ORDER BY users_birth_date, couriers_birth_date;
```
### Explanation:
This query performs a FULL JOIN on the results of two subqueries, which count the number of users and couriers grouped by birth_date. It retrieves the birth_date from both tables, naming them accordingly, and includes the counts of users and couriers. The results are sorted first by users_birth_date and then by couriers_birth_date, ensuring all records are displayed, even if one of the counts is NULL.

## Task 7
### Description:
Combine the results of the two queries that retrieve non-null birth_date values from the users and couriers tables to create a set of unique dates. Then, count the number of unique dates in the resulting set. Name the output column dates_count.

### SQL Query:
```sql

SELECT COUNT(DISTINCT birth_date) AS dates_count
FROM   (SELECT birth_date
        FROM   users
        WHERE  birth_date IS NOT NULL
        UNION
        SELECT birth_date
        FROM   couriers
        WHERE  birth_date IS NOT NULL) t;
```
### Explanation:
This query combines the results of two SELECT statements using the UNION operator, which ensures that only unique birth_date values are included from both the users and couriers tables. The outer query then counts the number of unique dates and outputs this count as dates_count. This approach effectively consolidates the non-null birth dates from both tables into a single result.

## Task 8
### Description:
Select the IDs of the first 100 users from the users table using LIMIT. Then, perform a CROSS JOIN with all product names from the products table. Output two columns: the user ID and the product name. Sort the results first by ascending user ID and then by ascending product name.

### SQL Query:
```sql
SELECT user_id,
       name
FROM   (SELECT user_id
        FROM   users
        LIMIT 100) t1 
CROSS JOIN (SELECT name
            FROM   products) t2
ORDER BY user_id, name;
```
### Explanation:
This query retrieves the IDs of the first 100 users from the users table. It then performs a CROSS JOIN with all product names from the products table, resulting in a combination of each of the selected user IDs with every product name. Finally, the results are sorted by user ID and product name in ascending order, ensuring a clear view of the user-product relationships.

## Task 9
### Description:
Join the user_actions and orders tables using the order_id key. Output the user ID, order ID, and the list of product IDs in the order. Sort the resulting table by user ID in ascending order and then by order ID in ascending order. Use a LIMIT clause to display only the first 1000 rows of the resulting table.

### SQL Query:
```sql

SELECT user_id,
       order_id,
       product_ids
FROM   user_actions
LEFT JOIN orders USING (order_id)
ORDER BY user_id, order_id
LIMIT 1000;
```
### Explanation:
This query performs a LEFT JOIN between the user_actions and orders tables based on the order_id column. It retrieves the user ID, order ID, and product IDs for each order. The results are sorted first by user ID and then by order ID in ascending order, ensuring a clear view of the user-order relationships. The LIMIT 1000 clause restricts the output to the first 1000 rows, making the dataset more manageable for analysis.

## Task 10
### Description:
Using the previous query, calculate the average number of products ordered by each user. Output the user ID and the average order size, rounding the average to two decimal places. Name the output column avg_order_size. Sort the result by user ID in ascending order. Use a LIMIT clause to display only the first 1000 rows of the resulting table.

### SQL Query:
```sql

SELECT user_id,
       ROUND(AVG(ARRAY_LENGTH(product_ids, 1)), 2) AS avg_order_size
FROM   (SELECT user_id,
               order_id
        FROM   user_actions
        WHERE  order_id NOT IN (SELECT order_id
                                FROM   user_actions
                                WHERE  action = 'cancel_order')) t1
LEFT JOIN orders USING (order_id)
GROUP BY user_id
ORDER BY user_id
LIMIT 1000;
```
### Explanation:
This query takes the results of the previous query and calculates the average number of products per order for each user. It uses the ARRAY_LENGTH function to determine the number of products in each order, averages these values, and rounds the result to two decimal places. The final output includes the user ID and the rounded average order size, sorted by user ID in ascending order. The LIMIT 1000 clause restricts the output to the first 1000 rows, allowing for a concise overview of average order sizes by user.

## Task 11
### Description:
Using the previous query, calculate the total price for each order. Output the columns for order ID and the total order price, naming the price column order_price. Sort the results by order ID in ascending order. Use a LIMIT clause to display only the first 1000 rows of the resulting table.

### SQL Query:
```sql

SELECT order_id,
       SUM(price) AS order_price
FROM   (SELECT order_id,
               product_ids,
               UNNEST(product_ids) AS product_id
        FROM   orders) t1
LEFT JOIN products USING (product_id)
GROUP BY order_id
ORDER BY order_id
LIMIT 1000;
```
### Explanation:
This query builds on the previous one by calculating the total price for each order. It uses the SUM function to aggregate the prices of all products associated with each order ID. The results are grouped by order_id to ensure that the total price reflects the sum of all products in each order. The final output includes the order ID and the calculated total price (order_price), sorted by order ID in ascending order. The LIMIT 1000 clause ensures that only the first 1000 rows are returned, providing a clear view of order prices.

## Task 12
### Description:
First, apply the unnest function to the orders table to expand the product IDs into individual rows, naming the column product_id. Then, join this expanded table with the products table using the product_id key to add price information. Output the columns for order ID, product ID, and product price. Sort the resulting table first by ascending order ID and then by ascending product ID. Use a LIMIT clause to display only the first 1000 rows of the resulting table.

### SQL Query:
```sql

SELECT order_id,
       product_id,
       price
FROM   (SELECT order_id,
               product_ids,
               UNNEST(product_ids) AS product_id
        FROM   orders) AS t
LEFT JOIN products USING (product_id)
ORDER BY order_id, product_id
LIMIT 1000;
```
### Explanation:
This query starts by using the UNNEST function to expand the product_ids array from the orders table into individual rows, allowing for each product to be associated with its respective order. The resulting table includes the order ID and the individual product IDs, which are then joined with the products table to retrieve the corresponding prices for each product. The final output displays the order ID, product ID, and price, sorted by order ID and product ID in ascending order. The LIMIT 1000 clause ensures that only the first 1000 rows are returned, providing a concise overview of the order and product data.

## Task 13
### Description:
Using the previous query, calculate the total price for each order. Output the order ID and the total order price, naming the price column order_price. Sort the results by order ID in ascending order. Use a LIMIT clause to display only the first 1000 rows of the resulting table.

### SQL Query:
```sql

SELECT order_id,
       SUM(price) AS order_price
FROM   (SELECT order_id,
               product_ids,
               UNNEST(product_ids) AS product_id
        FROM   orders) t1
LEFT JOIN products USING (product_id)
GROUP BY order_id
ORDER BY order_id
LIMIT 1000;
```
### Explanation:
This query builds on the previous one by calculating the total price for each order. It uses the SUM function to aggregate the prices of all products associated with each order ID. The results are grouped by order_id to ensure that the total price reflects the sum of all products in each order. The final output includes the order ID and the calculated total price (order_price), sorted by order ID in ascending order. The LIMIT 1000 clause ensures that only the first 1000 rows are returned, providing a clear view of order prices.

## Task 14
### Description:
Combine the previous queries that calculate order prices and the average order size for users. From the combined dataset, calculate the following metrics for each user:

-  Total number of orders (orders_count)
-  Average number of products in an order (avg_order_size)
-  Total value of all purchases (sum_order_value)
-  Average order value (avg_order_value)
-  Minimum order value (min_order_value)
-  Maximum order value (max_order_value)
Sort the final results by user ID in ascending order. Use a LIMIT clause to display only the first 1000 rows of the resulting table. Remember to only consider non-cancelled orders and round the average values to two decimal places.

### SQL Query:
```sql

SELECT user_id,
       COUNT(order_price) AS orders_count,
       ROUND(AVG(order_size), 2) AS avg_order_size,
       SUM(order_price) AS sum_order_value,
       ROUND(AVG(order_price), 2) AS avg_order_value,
       MIN(order_price) AS min_order_value,
       MAX(order_price) AS max_order_value
FROM   (SELECT user_id,
               order_id,
               ARRAY_LENGTH(product_ids, 1) AS order_size
        FROM   (SELECT user_id,
                       order_id
                FROM   user_actions
                WHERE  order_id NOT IN (SELECT order_id
                                        FROM   user_actions
                                        WHERE  action = 'cancel_order')) t1
            LEFT JOIN orders USING (order_id)) t2
    LEFT JOIN (SELECT order_id,
                      SUM(price) AS order_price
               FROM   (SELECT order_id,
                              product_ids,
                              UNNEST(product_ids) AS product_id
                       FROM   orders
                       WHERE  order_id NOT IN (SELECT order_id
                                               FROM   user_actions
                                               WHERE  action = 'cancel_order')) t3
                   LEFT JOIN products USING (product_id)
               GROUP BY order_id) t4 USING (order_id)
GROUP BY user_id
ORDER BY user_id
LIMIT 1000;
```
### Explanation:
This query combines various calculations for users' orders, filtering out cancelled orders. It begins by selecting user IDs and order IDs while determining the size of each order (number of products) from the user_actions and orders tables. The query then computes the total price of each order by joining with the products table. Finally, it aggregates the data by user ID to calculate the total number of orders, average order size, total order value, average order value, minimum, and maximum order values. The results are sorted by user ID, and the LIMIT 1000 clause ensures that only the first 1000 rows are displayed, providing a concise overview of the users' order statistics.

## Task 15
### Description:
Calculate the daily revenue of the service based on data from the orders, products, and user_actions tables. The revenue is defined as the total value of all products sold in orders. Name the column with the date date and the column with the revenue value revenue. Only include non-cancelled orders in the calculations. Sort the results by date in ascending order.

### SQL Query:
```sql

SELECT DATE(creation_time) AS date,
       SUM(price) AS revenue
FROM   (SELECT order_id,
               creation_time,
               product_ids,
               UNNEST(product_ids) AS product_id
        FROM   orders
        WHERE  order_id NOT IN (SELECT order_id
                                FROM   user_actions
                                WHERE  action = 'cancel_order')) t1
LEFT JOIN products USING (product_id)
GROUP BY DATE(creation_time)
ORDER BY date;
```
### Explanation:
This query calculates the daily revenue by first selecting the relevant data from the orders table, expanding the product_ids using the UNNEST function. It filters out any orders that have been cancelled, as identified by the user_actions table. The query then joins with the products table to retrieve the prices for each product. The results are grouped by date (derived from the creation_time column), and the total revenue is calculated using the SUM function. Finally, the results are sorted by date in ascending order, providing a clear view of daily revenue figures.

## Task 16
### Description:
Identify the 10 most popular products delivered in September 2022 from the courier_actions, orders, and products tables. The popularity of a product is determined by how frequently it appears in orders, counting each product only once per order, regardless of how many units were purchased. Output the product names and their purchase counts, naming the column with the count times_purchased.

### SQL Query:
```sql

SELECT name,
       COUNT(DISTINCT order_id) AS times_purchased
FROM   (SELECT order_id,
               product_id,
               name
        FROM   (SELECT DISTINCT order_id,
                                UNNEST(product_ids) AS product_id
                FROM   orders
                    LEFT JOIN courier_actions USING (order_id)
                WHERE  action = 'deliver_order'
                   AND DATE_PART('month', time) = 9
                   AND DATE_PART('year', time) = 2022) t1
            LEFT JOIN products USING (product_id)) t2
GROUP BY name
ORDER BY times_purchased DESC
LIMIT 10;
```
### Explanation:
This query retrieves the 10 most popular products delivered in September 2022. It first creates a subquery that selects distinct order IDs and expands the product_ids using the UNNEST function, filtering for only those orders marked as delivered (action = 'deliver_order') within the specified month and year. The results are then joined with the products table to get the product names. Finally, the outer query counts the distinct order IDs for each product name to determine how many times each product was purchased (counting only one instance per order). The results are sorted in descending order by times_purchased, and the LIMIT 10 clause restricts the output to the top 10 products.

## Task 17
### Description:
Modify a previous query to include gender data from the users table for users found in the user_actions table, ensuring that all users from user_actions are retained in the result. Calculate the average cancel rate for each gender, rounding the result to three decimal places. Name the column with the average value avg_cancel_rate. For users with missing gender information, display 'unknown' as their gender using the COALESCE function. Finally, sort the results by the gender column in ascending order.

### SQL Query:
```sql

SELECT COALESCE(sex, 'unknown') AS sex,
       ROUND(AVG(cancel_rate), 3) AS avg_cancel_rate
FROM   (SELECT user_id,
               sex,
               COUNT(DISTINCT order_id) FILTER (WHERE action = 'cancel_order')::DECIMAL / COUNT(DISTINCT order_id) AS cancel_rate
        FROM   user_actions
        LEFT JOIN users USING (user_id)
        GROUP BY user_id, sex) t
GROUP BY sex
ORDER BY sex;
```
### Explanation:
This query starts by calculating the cancel rate for each user in the user_actions table. It performs a LEFT JOIN with the users table to include gender information. The inner query computes the cancel rate by counting the distinct order IDs where the action is 'cancel_order' and dividing that by the total distinct order IDs for each user. The outer query then averages the cancel rates by gender, using COALESCE to substitute 'unknown' for any users with missing gender data. The final result is sorted by gender in ascending order, providing a comprehensive overview of average cancel rates segmented by user gender.

## Task 18
### Description:
Identify the IDs of the ten orders that took the longest to deliver using the orders and courier_actions tables. The resulting table should include the order_id.

### SQL Query:
```sql

SELECT order_id
FROM   (SELECT order_id,
               time AS delivery_time
        FROM   courier_actions
        WHERE  action = 'deliver_order') AS t
LEFT JOIN orders USING (order_id)
ORDER BY delivery_time - creation_time DESC
LIMIT 10;
```
### Explanation:
This query retrieves the order IDs of the ten orders with the longest delivery times. It begins by selecting the order_id and the delivery time from the courier_actions table for orders marked as delivered (action = 'deliver_order'). This result is then joined with the orders table to access the creation_time of each order. The difference between the delivery_time and the creation_time is calculated to determine the total delivery duration. The results are sorted in descending order by this duration, and the LIMIT 10 clause ensures that only the ten orders with the longest delivery times are returned.

## Task 19
### Description:
Replace the lists of product IDs in the orders table with lists of product names from the products table. Name the new column containing the product names product_names. Use a LIMIT clause to display only the first 1000 rows of the resulting table.

### SQL Query:
```sql

SELECT order_id,
       ARRAY_AGG(name) AS product_names
FROM   (SELECT order_id,
               UNNEST(product_ids) AS product_id
        FROM   orders) t
JOIN products USING (product_id)
GROUP BY order_id
LIMIT 1000;
```
### Explanation:
This query transforms the product IDs in the orders table into their corresponding product names from the products table. It first selects the order_id and uses the UNNEST function to expand the product_ids array into individual rows. The result is then joined with the products table to retrieve the product names based on the product IDs. Finally, the ARRAY_AGG function is used to aggregate the product names back into an array for each order, and the results are limited to the first 1000 rows for a concise output.

## Task 20
### Description:
Identify who ordered and delivered the largest orders based on the number of products. Output the order ID, user ID, and courier ID. Also, provide the ages of both the user and the courier in separate columns, measuring age in full years based on the latest date in the user_actions table. Name the columns for age user_age and courier_age. Sort the results by order ID in ascending order.

### SQL Query:
```sql

WITH order_id_large_size AS (
    SELECT order_id
    FROM   orders
    WHERE  ARRAY_LENGTH(product_ids, 1) = (SELECT MAX(ARRAY_LENGTH(product_ids, 1))
                                             FROM   orders)
)
SELECT DISTINCT t1.order_id,
                t1.user_id,
                DATE_PART('year', AGE((SELECT MAX(time)
                                        FROM   user_actions), users.birth_date))::INTEGER AS user_age,
                t2.courier_id,
                DATE_PART('year', AGE((SELECT MAX(time)
                                        FROM   user_actions), couriers.birth_date))::INTEGER AS courier_age
FROM   (SELECT order_id,
               user_id
        FROM   user_actions
        WHERE  order_id IN (SELECT * FROM order_id_large_size)) t1
LEFT JOIN (SELECT order_id,
                   courier_id
            FROM   courier_actions
            WHERE  order_id IN (SELECT * FROM order_id_large_size)) t2 USING (order_id)
LEFT JOIN users USING (user_id)
LEFT JOIN couriers USING (courier_id)
ORDER BY t1.order_id;
```
### Explanation:
This query determines the users and couriers associated with the largest orders, defined by the highest number of products in an order. It creates a temporary result set (order_id_large_size) that contains the IDs of orders with the maximum number of products. The main query retrieves the relevant data, including the user_id and order_id, while calculating the ages of both users and couriers using the AGE function. The results are sorted by order_id, giving a structured view of who ordered and delivered the largest orders along with their ages.

## Task 21
### Description:
Identify the most frequently purchased pairs of products based on the orders table, excluding cancelled orders. Output two columns: one for the product pairs (sorted in ascending order by name) and another for the count of how often each pair appears in the orders. Name the columns pair and count_pair. Sort the results first by the frequency of the pairs in descending order, and then by the pair column in ascending order.

### SQL Query:
```sql

WITH main_table AS (
    SELECT DISTINCT order_id,
                    product_id,
                    name
    FROM   (SELECT order_id,
                   UNNEST(product_ids) AS product_id
            FROM   orders
            WHERE  order_id NOT IN (SELECT order_id
                                    FROM   user_actions
                                    WHERE  action = 'cancel_order')) t
    LEFT JOIN products USING (product_id)
)
SELECT pair,
       COUNT(order_id) AS count_pair
FROM   (SELECT DISTINCT a.order_id,
                        CASE WHEN a.name > b.name THEN STRING_TO_ARRAY(CONCAT(b.name, '+', a.name), '+')
                             ELSE STRING_TO_ARRAY(CONCAT(a.name, '+', b.name), '+') END AS pair
        FROM   main_table a 
        JOIN   main_table b
        ON     a.order_id = b.order_id AND
               a.name != b.name) t
GROUP BY pair
ORDER BY count_pair DESC, pair;
```
### Explanation:
This query finds the most frequently purchased pairs of products by creating a common table expression (main_table) that includes distinct order_id, product_id, and name. It then performs a self-join to form pairs of product names within the same order. The results are grouped by the formed pairs, and the query counts how many times each pair appears in the orders, sorting the results first by the frequency of the pairs in descending order and then by the pair in ascending order.
