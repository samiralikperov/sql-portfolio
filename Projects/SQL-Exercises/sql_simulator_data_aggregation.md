# Data Aggregation

## SQL Task Links

| Task Number | Task Description                                     | Link                                     |
|-------------|-----------------------------------------------------|------------------------------------------|
| Task 1      | Select IDs of all unique users from the user_actions table | [View Task 1](#task-1)                  |
| Task 2      | Select unique pairs of courier_id and order_id from courier_actions | [View Task 2](#task-2)                  |
| Task 3      | Calculate the maximum and minimum prices of products | [View Task 3](#task-3)                  |
| Task 4      | Count total records and non-null birth dates in users table | [View Task 4](#task-4)                  |
| Task 5      | Count total and unique user IDs in user_actions table | [View Task 5](#task-5)                  |
| Task 6      | Count the number of female couriers in the couriers table | [View Task 6](#task-6)                  |
| Task 7      | Calculate the first and last delivery times in courier_actions | [View Task 7](#task-7)                  |
| Task 8      | Calculate the total cost of an order of specific products | [View Task 8](#task-8)                  |
| Task 9      | Count the number of orders with nine or more products | [View Task 9](#task-9)                  |
| Task 10     | Calculate the age difference between oldest and youngest male users | [View Task 10](#task-10)                |
| Task 11     | Calculate the total cost of an order with specified quantities | [View Task 11](#task-11)                |
| Task 12     | Calculate the average price of products containing "чай" or "кофе" | [View Task 12](#task-12)                |
| Task 13     | Calculate the age difference between oldest and youngest male users | [View Task 13](#task-13)                |
| Task 14     | Calculate average order size on weekends | [View Task 14](#task-14)                |
| Task 15     | Calculate unique users and orders per user | [View Task 15](#task-15)                |
| Task 16     | Count users who never canceled their orders | [View Task 16](#task-16)                |
| Task 17     | Count total orders and share of large orders | [View Task 17](#task-17)                |




# SQL Tasks

# SQL Tasks

## Task 1

### Description:
Select the IDs of all unique users from the `user_actions` table. Sort the result by user ID in ascending order.
### Resulting Table Fields:
- **user_id**
### SQL Query:
```sql
SELECT DISTINCT user_id
FROM   user_actions
ORDER BY user_id;
```
### Explanation:
This query retrieves distinct user_id values from the user_actions table. The DISTINCT keyword ensures that only unique user IDs are returned. The results are sorted in ascending order by user_id.

## Task 2
### Description:
Apply DISTINCT to two columns of the courier_actions table and select unique pairs of courier_id and order_id. Sort the result first by courier_id in ascending order, then by order_id in ascending order.

### Resulting Table Fields:
courier_id
order_id
### SQL Query:
``` sql
SELECT DISTINCT courier_id,
                order_id
FROM   courier_actions
ORDER BY courier_id, order_id;
```
### Explanation:
This query selects unique combinations of courier_id and order_id from the courier_actions table using the DISTINCT keyword. The results are sorted first by courier_id and then by order_id, both in ascending order.



## Task 3
### Description:
Calculate the maximum and minimum prices of products in the products table. Name the fields max_price and min_price, respectively.
### Resulting Table Fields:
max_price
min_price
SQL Query:
```sql
SELECT MAX(price) AS max_price,
       MIN(price) AS min_price
FROM   products;
```
### Explanation:
This query retrieves the maximum and minimum values of the price column from the products table using the MAX() and MIN() aggregate functions. The results are aliased as max_price and min_price.


## Task 4
### Description:
Count the total number of records in the users table and the number of records where the birth_date is specified (not NULL). Name the column with the total number of records as dates and the column with records that have no NULL values as dates_not_null.
### Resulting Table Fields:
dates
dates_not_null
### SQL Query:
```sql
SELECT COUNT(*) AS dates,
       COUNT(birth_date) AS dates_not_null
FROM   users;
```
### Explanation:
This query counts all records in the users table and counts only those records with a non-null birth_date. The total number of records is aliased as dates, while the count of records with specified birth dates is aliased as dates_not_null.

## Task 5
### Description:
Count all values in the user_id column of the user_actions table, as well as the number of unique values in this column (i.e., the number of unique users of the service). Name the first column as users and the second column as unique_users.
### Resulting Table Fields:
users
unique_users
### SQL Query:
```sql
Copy code
SELECT COUNT(user_id) AS users,
       COUNT(DISTINCT user_id) AS unique_users
FROM   user_actions;
```
### Explanation:
This query counts all occurrences of user_id in the user_actions table, representing the total number of actions taken by users. It also counts distinct user_id values to determine the number of unique users, with the results aliased as users and unique_users, respectively.





## Task 6
### Description:
Count the number of female couriers in the `couriers` table. Name the resulting column with the count as `couriers`.
### Resulting Table Fields:
- **couriers**

### SQL Query:
```sql
SELECT COUNT(courier_id) AS couriers
FROM   couriers
WHERE  sex = 'female';
```
### Explanation:
This query counts the number of records in the couriers table where the sex is 'female'. The result is aliased as couriers, providing the total number of female couriers.



## Task 7
### Description:
Calculate the times when the first and last deliveries of orders occurred in the courier_actions table. Name the column with the first delivery time as first_delivery and the column with the last delivery time as last_delivery.
### Resulting Table Fields:
first_delivery
last_delivery
### SQL Query:
```sql
SELECT MIN(time) AS first_delivery,
       MAX(time) AS last_delivery
FROM   courier_actions
WHERE  action = 'deliver_order';
```
### Explanation:
This query retrieves the minimum and maximum times from the courier_actions table where the action is 'deliver_order'. The earliest delivery time is aliased as first_delivery and the latest as last_delivery.




## Task 8
### Description:
Assume that a user ordered one pack of croutons, one pack of chips, and one energy drink. Calculate the total cost of this order. Name the column with the calculated order cost as order_price. Use the products table for the calculations.
### Resulting Table Fields:
order_price
### SQL Query:
```sql
Copy code
SELECT SUM(price) AS order_price
FROM   products
WHERE  name IN ('сухарики', 'чипсы', 'энергетический напиток');
```
### Explanation:
This query sums the prices of the products from the products table that match the specified names (croutons, chips, and energy drink). The result is aliased as order_price, representing the total cost of the order.



## Task 9
### Description:
Count the number of orders in the orders table that have nine or more products. Use the array_length function to filter the data by the number of products in the order and perform aggregation. Name the resulting column as orders.
### Resulting Table Fields:
orders
### SQL Query:
```sql
SELECT COUNT(order_id) AS orders
FROM   orders
WHERE  ARRAY_LENGTH(product_ids, 1) >= 9;
```
### Explanation:
This query counts the number of order_id records in the orders table where the length of the product_ids array is nine or more. The result is aliased as orders, indicating the total count of qualifying orders.




## Task 10
### Description:
Using the AGE function and an aggregate function, calculate the age of the youngest male courier in the couriers table. Express the age in years, months, and days as a VARCHAR. Use the current date for the calculation.
### Resulting Table Fields:
min_age
SQL Query:
```sql
SELECT MIN(AGE(birth_date))::VARCHAR AS min_age
FROM   couriers
WHERE  sex = 'male';
```
### Explanation:
This query calculates the age of the youngest male courier in the couriers table using the AGE function on the birth_date. The result is converted to a string format and aliased as min_age.

## Task 11
### Description:
Calculate the total cost of an order consisting of three packs of croutons, two packs of chips, and one energy drink. Name the column with the calculated order cost as order_price. Use the products table for the calculations.
### Resulting Table Fields:
order_price
### SQL Query:
```sql
SELECT SUM(CASE 
               WHEN name = 'сухарики' THEN price * 3
               WHEN name = 'чипсы' THEN price * 2
               WHEN name = 'энергетический напиток' THEN price
               ELSE 0 
           END) AS order_price
FROM   products;
```
### Explanation:
This query calculates the total cost of the specified order by using a CASE statement to multiply the prices of croutons, chips, and the energy drink by their respective quantities. The total sum is aliased as order_price, representing the total cost of the order.










## Task 12

### Description:
Calculate the average price of products in the `products` table whose names contain the words "чай" (tea) or "кофе" (coffee). Exclude products that contain "иван-чай" (Ivan tea) or "чайный гриб" (tea mushroom) from the calculation. Round the average price to two decimal places. Name the resulting column as `avg_price`.

### Resulting Table Fields:
- **avg_price**

### SQL Query:
```sql
SELECT ROUND(AVG(price), 2) AS avg_price
FROM   products
WHERE  (name LIKE '%чай%'
    OR name LIKE '%кофе%')
   AND name NOT LIKE '%иван-чай%'
   AND name NOT LIKE '%чайный гриб%';
```
### Explanation:
This query calculates the average price of products in the products table that have "чай" or "кофе" in their names. It excludes those that contain "иван-чай" or "чайный гриб". The average price is rounded to two decimal places and is aliased as avg_price.





## Task 13
### Description:
Using the AGE function, calculate the age difference between the oldest and youngest male users in the users table. Express the age difference in years, months, and days, converting it to VARCHAR. Name the column with the calculated value as age_diff.
### Resulting Table Fields:
age_diff
### SQL Query:
```sql
SELECT AGE(MAX(birth_date), MIN(birth_date))::VARCHAR AS age_diff
FROM   users
WHERE  sex = 'male';
```
### Explanation:
This query calculates the age difference between the oldest and youngest male users by using the AGE function on the maximum and minimum birth_date. The result is converted to a string format and aliased as age_diff.







## Task 14
### Description:
Calculate the average number of products in orders from the orders table that users placed on weekends (Saturday and Sunday) throughout the service's operation. Round the resulting value to two decimal places. Name the column as avg_order_size.
### Resulting Table Fields:
avg_order_size
### SQL Query:
```sql
SELECT ROUND(AVG(ARRAY_LENGTH(product_ids, 1)), 2) AS avg_order_size
FROM   orders
WHERE  DATE_PART('dow', creation_time) IN (6, 0);
```
### Explanation:
This query calculates the average number of products in orders placed on weekends by using the ARRAY_LENGTH function. It filters results to include only orders placed on Saturday (6) and Sunday (0). The average is rounded to two decimal places and aliased as avg_order_size.





## Task 15
### Description:
Based on the data in the user_actions table, count the number of unique service users, the number of unique orders, and divide one by the other to find out how many orders correspond to one user. Include all three values in the resulting table, naming the columns as unique_users, unique_orders, and orders_per_user. Round the number of orders per user to two decimal places.
### Resulting Table Fields:
unique_users
unique_orders
orders_per_user
### SQL Query:
```sql
SELECT COUNT(DISTINCT user_id) AS unique_users,
       COUNT(DISTINCT order_id) AS unique_orders,
       ROUND(COUNT(DISTINCT order_id)::DECIMAL / COUNT(DISTINCT user_id), 2) AS orders_per_user
FROM   user_actions;
```
### Explanation:
This query counts unique user_id values to get the total number of users and unique order_id values for the total number of orders. It then calculates the average number of orders per user, rounding the result to two decimal places. The columns are aliased as unique_users, unique_orders, and orders_per_user.





## Task 16
### Description:
Count how many users have never canceled their orders. Subtract the number of unique users who have canceled at least once from the total number of unique users. Consider the necessary condition in the FILTER to get the correct result. Name the resulting column as users_count.
### Resulting Table Fields:
users_count
### SQL Query:
```sql
Copy code
SELECT COUNT(DISTINCT user_id) - COUNT(DISTINCT user_id) FILTER (WHERE action = 'cancel_order') AS users_count
FROM   user_actions;
```
### Explanation:
This query counts the total number of unique users in the user_actions table and subtracts the count of unique users who have canceled at least one order, using the FILTER clause. The result is aliased as users_count, indicating the number of users who have never canceled an order.





## Task 17
### Description:
Count the total number of orders in the orders table, the number of orders with five or more products, and find the share of orders with five or more products in the total number of orders. Name the resulting columns as orders, large_orders, and large_orders_share. Round the share of orders to two decimal places.
### Resulting Table Fields:
orders
large_orders
large_orders_share
### SQL Query:
```sql
SELECT COUNT(order_id) AS orders,
       COUNT(order_id) FILTER (WHERE ARRAY_LENGTH(product_ids, 1) >= 5) AS large_orders,
       ROUND(COUNT(order_id) FILTER (WHERE ARRAY_LENGTH(product_ids, 1) >= 5)::DECIMAL / COUNT(order_id), 2) AS large_orders_share
FROM   orders;
```
### Explanation:
This query counts the total number of order_id records in the orders table and the number of orders with five or more products using the FILTER clause. It calculates the share of large orders and rounds the result to two decimal places. The columns are aliased as orders, large_orders, and large_orders_share.


