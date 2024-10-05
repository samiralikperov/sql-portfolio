# Data Aggregation

## SQL Task Links

| Task Number | Task Description                                     | Link                                     |
|-------------|-----------------------------------------------------|------------------------------------------|
| Task 1      | Select products where the price does not exceed 100 rubles | [View Task 1](#task-1)                  |



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
