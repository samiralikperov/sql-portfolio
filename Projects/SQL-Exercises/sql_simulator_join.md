## JOIN

## SQL Task Links

| Task Number | Task Description                                            | Link                                     |
|-------------|------------------------------------------------------------|------------------------------------------|
| Task 1      | Join user_actions and users, select user IDs              | [View Task 1](#task-1)                  |
| Task 2      | Count unique user IDs from the joined tables               | [View Task 2](#task-2)                  |
| Task 3      | LEFT JOIN user_actions and users, select user IDs         | [View Task 3](#task-3)                  |
| Task 4      | Count unique user IDs from the LEFT JOIN                   | [View Task 4](#task-4)                  |
| Task 5      | Filter NULL values in RIGHT JOIN and sort results          | [View Task 5](#task-5)                  |
| Task 6      | FULL JOIN user_actions and users, count unique birth dates | [View Task 6](#task-6)                  |
| Task 7      | Union unique birth dates from users and couriers           | [View Task 7](#task-7)                  |
| Task 8      | CROSS JOIN first 100 users with all product names         | [View Task 8](#task-8)                  |
| Task 9      | JOIN user_actions and orders, select user and order IDs    | [View Task 9](#task-9)                  |
| Task 10     | Filter unique non-canceled orders from the JOIN            | [View Task 10](#task-10)                |
| Task 11     | Calculate average items per user from non-canceled orders  | [View Task 11](#task-11)                |
| Task 12     | Unnest product IDs in orders and join with products        | [View Task 12](#task-12)                |
| Task 13     | Calculate total order price from previous query            | [View Task 13](#task-13)                |
| Task 14     | Combine previous queries to get average order metrics      | [View Task 14](#task-14)                |
| Task 15     | Calculate daily revenue from orders                        | [View Task 15](#task-15)                |
| Task 16     | Find top 10 most popular items delivered in September 2022 | [View Task 16](#task-16)                |
| Task 17     | Calculate average cancel rate by gender                   | [View Task 17](#task-17)                |
| Task 18     | Determine orders that took longest to deliver              | [View Task 18](#task-18)                |
| Task 19     | Replace product IDs with names in orders                   | [View Task 19](#task-19)                |
| Task 20     | Find largest orders and their user and courier IDs        | [View Task 20](#task-20)                |
| Task 21     | Identify frequently purchased item pairs                   | [View Task 21](#task-21)                |
| Task 22     | Calculate average age of users and couriers from orders    | [View Task 22](#task-22)                |


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
Now, try rewriting the query from the previous task to count the unique IDs in the combined table. Again, join the tables, but this time count the unique user_id from one of the ID columns. Output this count as a result, naming the column users_count.
### SQL Query:
```sql
SELECT COUNT(DISTINCT a.user_id) AS users_count
FROM   user_actions a 
JOIN   users b USING (user_id);
```
### Explanation:
In this query, we join the two tables again, but instead of selecting columns, we count unique user_id values from the user_actions table. This helps determine how many unique users have taken actions.

## Task 3
### Description:
Using a LEFT JOIN, combine the user_actions and users tables on the user_id key. Note the order of the tables — user_actions on the left and users on the right. Include two columns with user_id from both tables, naming them user_id_left and user_id_right. Also include order_id, time, action, sex, birth_date. Sort the resulting table by ascending user ID from the left table.

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
This query uses a LEFT JOIN to combine user_actions and users, ensuring all records from user_actions are included, even if there’s no match in users. This lets us see actions taken by users, including those without corresponding entries in the users table.

## Task 4
### Description:
Now, try to rewrite the query from the previous task to count the unique user_id values from the left table user_actions. Output this count as a result, naming the column users_count.

### SQL Query:
```sql
SELECT COUNT(DISTINCT a.user_id) AS users_count
FROM   user_actions a 
LEFT JOIN users b USING (user_id);
```
### Explanation:
This query counts unique user_id values from the user_actions table after performing a LEFT JOIN. It helps verify how many distinct users have made actions recorded in user_actions.

## Task 5
### Description:
Take the query from Task 3, where you joined the user_actions and users tables using a LEFT JOIN, and add a WHERE clause to exclude NULL values in the user_id column from the right table. Include all the same columns and sort the resulting table by ascending user ID from the left table.

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
This query filters out entries with NULL user_id values from the users table after performing a LEFT JOIN. This gives us only those user actions that have matching user details in both tables.




## Task 6

### Description:
Using a `FULL JOIN`, combine the two tables obtained from the previous queries (i.e., join the two subqueries). Do not modify them; simply add the necessary JOIN. Include two columns with `birth_date` from both tables, naming them `users_birth_date` and `couriers_birth_date`. Also, include the columns with user and courier counts — `users_count` and `couriers_count`. Sort the resulting table first by the `users_birth_date` column in ascending order, then by the `couriers_birth_date` column in ascending order.

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
            GROUP BY birth_date) b USING(birth_date)
ORDER BY users_birth_date, couriers_birth_date;
```
### Explanation:
This query combines the users and couriers tables using a FULL JOIN, which includes all records from both tables. It counts users and couriers by their birth dates and sorts the results based on the birth dates.

## Task 7
### Description:
Combine the following two queries to create a set of unique dates from the users and couriers tables:

SELECT birth_date
FROM users
WHERE birth_date IS NOT NULL;

SELECT birth_date
FROM couriers
WHERE birth_date IS NOT NULL;
Use a subquery to count the number of unique dates obtained after merging the sets. Name the column with the count dates_count.

### SQL Query:
```sql
SELECT COUNT(birth_date) AS dates_count
FROM   (SELECT birth_date
        FROM   users
        WHERE  birth_date IS NOT NULL
        UNION
        SELECT birth_date
        FROM   couriers
        WHERE  birth_date IS NOT NULL) t;
```
### Explanation:
This query uses a UNION to combine unique birth dates from both the users and couriers tables, then counts how many unique dates exist in total.

## Task 8
### Description:
Select the first 100 users from the users table (simply select the first 100 records using a LIMIT) and perform a CROSS JOIN with all the item names from the products table. Output two columns — user ID and item name. Sort the result by ascending user ID, then by item name — also in ascending order.

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
This query retrieves the first 100 users from the users table and combines them with every item from the products table using a CROSS JOIN, resulting in a Cartesian product of users and menu items.

## Task 9
### Description:
Join the user_actions and orders tables on the order_id key. Output the user IDs and order IDs, as well as the list of items in each order. Sort the table by user ID in ascending order, then by order ID — also in ascending order. Include a LIMIT to output only the first 1000 rows of the resulting table.

### SQL Query:
```sql
SELECT user_id,
       order_id,
       product_ids
FROM   user_actions
LEFT JOIN orders USING(order_id)
ORDER BY user_id, order_id 
LIMIT 1000;
```
### Explanation:
This query joins the user_actions and orders tables on order_id, providing details of which users made which orders and the products in those orders. It limits the results to the first 1000 records for ease of analysis.

## Task 10
### Description:
Again, join the user_actions and orders tables, but now only keep unique, non-canceled orders (we did a similar query in the previous lesson). The other conditions remain the same: output user IDs and order IDs, as well as the list of items in each order. Sort the table by user ID in ascending order, then by order ID — also in ascending order. Include a LIMIT to output only the first 1000 rows of the resulting table.

### SQL Query:
```sql
SELECT user_id,
       order_id,
       product_ids
FROM   (SELECT user_id,
               order_id
        FROM   user_actions
        WHERE  order_id NOT IN (SELECT order_id
                                FROM   user_actions
                                WHERE  action = 'cancel_order')) t
LEFT JOIN orders USING(order_id)
ORDER BY user_id, order_id 
LIMIT 1000;
```
### Explanation:
This query filters out canceled orders from the user_actions table before joining it with the orders table, ensuring that only valid orders are considered. It helps in analyzing active user interactions with valid orders.

## Task 11
### Description:
Using the query from the previous task, calculate the average number of items each user orders. Output the user ID and the average number of items in the order. Round the average value to two decimal places. Name the column with the calculated values avg_order_size. Sort the results by ascending user ID. Include a LIMIT to output only the first 1000 rows of the resulting table.

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
LEFT JOIN orders USING(order_id)
GROUP BY user_id
ORDER BY user_id 
LIMIT 1000;
```
### Explanation:
This query calculates the average order size for each user, considering only non-canceled orders. It aggregates the results to provide insights into how many items users typically order.




## Task 12

### Description:
Using a `FULL JOIN`, combine the orders table with the previous query results. The two `birth_date` columns should be named `users_birth_date` and `couriers_birth_date`. Also include columns for the count of users and couriers — `users_count` and `couriers_count`. Sort the resulting table first by the `users_birth_date` and then by the `couriers_birth_date` in ascending order.

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
            GROUP BY birth_date) b USING(birth_date)
ORDER BY users_birth_date, couriers_birth_date;
```
### Explanation:
This query combines data from both the users and couriers tables by birth_date, counting the number of users and couriers per birth date.

## Task 13
Description:
Combine the following two queries to create a set of unique birth dates from the users and couriers tables. Use a subquery to count the number of unique dates obtained after merging the sets. Name the column with the count dates_count.

### SQL Query:
```sql
SELECT COUNT(birth_date) AS dates_count
FROM   (SELECT birth_date
        FROM   users
        WHERE  birth_date IS NOT NULL
        UNION
        SELECT birth_date
        FROM   couriers
        WHERE  birth_date IS NOT NULL) t;
```
### Explanation:
This query merges unique birth dates from both tables and counts them, allowing you to see how many unique birth dates exist across both datasets.

## Task 14
### Description:
Select the first 100 users from the users table and perform a CROSS JOIN with all item names from the products table. Output two columns — user ID and item name. Sort the result by ascending user ID and item name.

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
This query retrieves the first 100 users and combines them with every item from the products table using a CROSS JOIN, resulting in a Cartesian product.

## Task 15
### Description:
Join the user_actions and orders tables on the order_id key. Output user IDs and order IDs, as well as the list of items in each order. Sort the table by user ID in ascending order, then by order ID. Include a LIMIT to output only the first 1000 rows.

### SQL Query:
```sql
SELECT user_id,
       order_id,
       product_ids
FROM   user_actions
LEFT JOIN orders USING(order_id)
ORDER BY user_id, order_id 
LIMIT 1000;
```
### Explanation:
This query joins the user_actions and orders tables on order_id, providing details of users and their respective orders along with the products ordered.

## Task 16
### Description:
Again, join the user_actions and orders tables, but only keep unique, non-canceled orders. The other conditions remain the same: output user IDs and order IDs, as well as the list of items in each order. Sort the table by user ID and order ID. Include a LIMIT to output only the first 1000 rows.

### SQL Query:
```sql
Copy code
SELECT user_id,
       order_id,
       product_ids
FROM   (SELECT user_id,
               order_id
        FROM   user_actions
        WHERE  order_id NOT IN (SELECT order_id
                                FROM   user_actions
                                WHERE  action = 'cancel_order')) t
LEFT JOIN orders USING(order_id)
ORDER BY user_id, order_id 
LIMIT 1000;
```
### Explanation:
This query filters out canceled orders and then retrieves valid user actions and their respective orders, ensuring only active orders are considered.

## Task 17
### Description:
Using the query from the previous task, calculate the average number of items each user orders. Output user ID and average number of items in the order. Round the average value to two decimal places. Name the column avg_order_size. Sort the results by user ID and include a LIMIT to output only the first 1000 rows.

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
LEFT JOIN orders USING(order_id)
GROUP BY user_id
ORDER BY user_id 
LIMIT 1000;
```
### Explanation:
This query calculates the average order size for each user, considering only non-canceled orders, helping to understand the typical purchasing behavior of users.

## Task 18
### Description:
Using the orders, products, and user_actions tables, calculate the daily revenue for the service. Revenue is defined as the total cost of all sold items contained in orders. Name the date column date and the revenue column revenue. Only consider non-canceled orders in the calculations. Sort the results by date in ascending order.

### SQL Query:
```sql
Copy code
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
LEFT JOIN products USING(product_id)
GROUP BY DATE(creation_time)
ORDER BY date;
```
### Explanation:
This query calculates daily revenue by summing the prices of items sold on each day while excluding canceled orders, providing insights into daily sales performance.

## Task 19
### Description:
Using the courier_actions, orders, and products tables, identify the 10 most popular items delivered in September 2022. Popular items are those that appeared most frequently in orders, counting each item only once per order. Output the item names and how many times they were purchased. Name the count column times_purchased.

### SQL Query:
```sql
SELECT name,
       COUNT(product_id) AS times_purchased
FROM   (SELECT order_id,
               product_id,
               name
        FROM   (SELECT DISTINCT order_id,
                             UNNEST(product_ids) AS product_id
                FROM   orders
                LEFT JOIN courier_actions USING(order_id)
                WHERE  action = 'deliver_order'
                   AND DATE_PART('month', time) = 9
                   AND DATE_PART('year', time) = 2022) t1
            LEFT JOIN products USING(product_id)) t2
GROUP BY name
ORDER BY times_purchased DESC 
LIMIT 10;
```
### Explanation:
This query finds the most popular items delivered during a specified period by counting unique occurrences of each item across orders, helping to identify top-selling products.

## Task 20
### Description:
Take the query from one of the previous lessons and join it with the users table to include user details. Calculate the average cancel rate for each gender, rounding it to three decimal places. Name the column avg_cancel_rate. Also, calculate the cancel rate for users without gender information and label this as 'unknown'. Sort the results by gender in ascending order.

### SQL Query:
```sql
SELECT COALESCE(sex, 'unknown') AS sex,
       ROUND(AVG(cancel_rate), 3) AS avg_cancel_rate
FROM   (SELECT user_id,
               sex,
               COUNT(DISTINCT order_id) FILTER (WHERE action = 'cancel_order')::decimal / COUNT(DISTINCT order_id) AS cancel_rate
        FROM   user_actions
        LEFT JOIN users USING(user_id)
        GROUP BY user_id, sex
        ORDER BY cancel_rate DESC) t
GROUP BY sex
ORDER BY sex;
```
### Explanation:
This query computes the average cancel rate based on the user’s gender, including those with missing gender data. It provides insights into cancellation trends by gender.

## Task 21
### Description:
Identify the 10 orders that took the longest to deliver using the orders and courier_actions tables. Output the order ID, user ID, and courier ID, as well as the user and courier ages. Age is measured in full years and is calculated relative to the last date in the user_actions table. Name the age columns user_age and courier_age. Sort the results by order ID in ascending order.

### SQL Query:
```sql
WITH order_id_large_size AS (
    SELECT order_id
    FROM   orders
    WHERE  ARRAY_LENGTH(product_ids, 1) = (SELECT MAX(ARRAY_LENGTH(product_ids, 1))
                                            FROM   orders)
)
SELECT DISTINCT order_id,
                user_id,
                DATE_PART('year', AGE((SELECT MAX(time)
                       FROM   user_actions), users.birth_date))::integer AS user_age,
                courier_id,
                DATE_PART('year', AGE((SELECT MAX(time)
                                  FROM   user_actions), couriers.birth_date))::integer AS courier_age
FROM   (SELECT order_id,
               user_id
        FROM   user_actions
        WHERE  order_id IN (SELECT *
                            FROM   order_id_large_size)) t1
LEFT JOIN (SELECT order_id,
                      courier_id
               FROM   courier_actions
               WHERE  order_id IN (SELECT *
                                   FROM   order_id_large_size)) t2 USING(order_id)
LEFT JOIN users USING(user_id)
LEFT JOIN couriers USING(courier_id)
ORDER BY order_id;
```
### Explanation:
This query identifies the largest orders by item count and provides details about the users and couriers associated with those orders, including their ages calculated based on the latest activity in the user_actions table.

## Task 22
### Description:
Determine which pairs of items are most frequently purchased together based on the orders table. Exclude canceled orders. Output the pairs of item names and the count of how many times each pair was purchased together. Name the columns pair and count_pair. Sort the results by purchase frequency in descending order and by item pair names in ascending order.

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
        LEFT JOIN products USING(product_id)
)
SELECT pair,
       COUNT(order_id) AS count_pair
FROM   (SELECT DISTINCT a.order_id,
                        CASE WHEN a.name > b.name THEN STRING_TO_ARRAY(CONCAT(b.name, '+', a.name), '+')
                             ELSE STRING_TO_ARRAY(CONCAT(a.name, '+', b.name), '+') END AS pair
        FROM   main_table a JOIN main_table b
                ON a.order_id = b.order_id AND
                   a.name != b.name) t
GROUP BY pair
ORDER BY count_pair DESC, pair;
```
### Explanation:
This query identifies and counts pairs of items that are frequently purchased together, which can provide insights into customer buying patterns and item associations.
