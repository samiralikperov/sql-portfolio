# Data Grouping

## SQL Task Links

| Task Number | Task Description                                                  | Link                                     |
|-------------|------------------------------------------------------------------|------------------------------------------|
| Task 1      | Count the number of couriers by gender                          | [View Task 1](#task-1)                  |
| Task 2      | Count the number of created and canceled orders                  | [View Task 2](#task-2)                  |
| Task 3      | Count how many orders were placed in each month                  | [View Task 3](#task-3)                  |
| Task 4      | Count placed and canceled orders by month                        | [View Task 4](#task-4)                  |
| Task 5      | Calculate the maximum month number of birth by gender            | [View Task 5](#task-5)                  |
| Task 6      | Calculate the month number of birth of the youngest user         | [View Task 6](#task-6)                  |
| Task 7      | Count the maximum age of users by gender                         | [View Task 7](#task-7)                  |
| Task 8      | Count the number of users by age                                 | [View Task 8](#task-8)                  |
| Task 9      | Count users by age and gender                                    | [View Task 9](#task-9)                  |
| Task 10     | Count the number of products in each order                       | [View Task 10](#task-10)                 |
| Task 11     | Count the number of created and canceled orders                  | [View Task 11](#task-11)                |
| Task 12     | Identify the five users who made the most orders in August 2022 | [View Task 12](#task-12)                |
| Task 13     | Find couriers who delivered only one order in September 2022    | [View Task 13](#task-13)                |
| Task 14     | Select users whose last order was created before September 8, 2022 | [View Task 14](#task-14)              |
| Task 15     | Group orders based on the number of items                        | [View Task 15](#task-15)                |
| Task 16     | Group users by age into four categories                          | [View Task 16](#task-16)                |
| Task 17     | Calculate the average order size for weekends and weekdays       | [View Task 17](#task-17)                |
| Task 18     | Calculate total orders and cancellation rate for each user       | [View Task 18](#task-18)                |
| Task 19     | Calculate order statistics for each day of the week             | [View Task 19](#task-19)                |




# SQL Tasks

## Task 1

### Description:
Use the `GROUP BY` operator to count the number of male and female couriers in the `couriers` table. Name the new column with the count of couriers as `couriers_count`. Sort the result by this column in ascending order.

### Resulting Table Fields:
- **sex**
- **couriers_count**

### SQL Query:
```sql
SELECT sex,
       COUNT(courier_id) AS couriers_count
FROM   couriers
GROUP BY sex
ORDER BY couriers_count;
```
### Explanation:
This query counts the number of couriers in the couriers table grouped by their gender (indicated by the sex column). The COUNT(courier_id) function counts the number of courier IDs for each gender group. The results are sorted in ascending order by the couriers_count, which represents the number of couriers for each gender.



## Task 2

### Description:
Count the number of created and canceled orders in the `user_actions` table. Name the new column with the order count as `orders_count`. Sort the result by the number of orders in ascending order.

### Resulting Table Fields:
- **action**
- **orders_count**

### SQL Query:
```sql
SELECT action,
       COUNT(order_id) AS orders_count
FROM   user_actions
GROUP BY action
ORDER BY orders_count;
```
### Explanation:
This query counts the occurrences of each action (created or canceled orders) in the user_actions table using the GROUP BY clause. The count of each action type is aliased as orders_count, and the results are sorted in ascending order by orders_count.

## Task 3
### Description:
Using grouping and the DATE_TRUNC function, truncate all dates to the beginning of the month and count how many orders were placed in each month from the orders table. Name the column with the truncated date as month and the column with the number of orders as orders_count. Sort the results by month in ascending order.
### Resulting Table Fields:
month
orders_count
### SQL Query:
```sql
SELECT DATE_TRUNC('month', creation_time) AS month,
       COUNT(order_id) AS orders_count
FROM   orders
GROUP BY month
ORDER BY month;
```
### Explanation:
This query truncates the creation_time to the beginning of each month using DATE_TRUNC and counts the number of order_id records for each month. The results are sorted by the truncated date (month) in ascending order.

## Task 4
### Description:
Using grouping and the DATE_TRUNC function, truncate all dates to the beginning of the month and count how many orders were placed and how many were canceled in each month from the user_actions table. Name the column with the truncated date as month and the column with the number of orders as orders_count. Sort the results first by month in ascending order, then by action in ascending order.
### Resulting Table Fields:
month
action
orders_count
### SQL Query:
```sql
SELECT DATE_TRUNC('month', time) AS month,
       action,
       COUNT(order_id) AS orders_count
FROM   user_actions
GROUP BY month, action
ORDER BY month, action;
```
### Explanation:
This query truncates the time to the beginning of each month and counts the occurrences of each action (placed or canceled orders) in the user_actions table. The results are grouped by month and action, then sorted first by month and then by action in ascending order.

## Task 5
### Description:
From the users table, calculate the maximum month number among all birth month numbers of service users. Group the results separately for male and female users. Name the new column with the maximum month number as max_month and convert the values in this column to INTEGER format.
### Resulting Table Fields:
sex
max_month
### SQL Query:
```sql
SELECT sex,
       MAX(DATE_PART('month', birth_date))::INTEGER AS max_month
FROM   users
GROUP BY sex
ORDER BY sex;
```
### Explanation:
This query calculates the maximum month number from the birth_date of users, grouping the results by sex. The maximum month is converted to INTEGER format and aliased as max_month. The results are sorted by gender.

## Task 6
### Description:
From the users table, calculate the month number of birth of the youngest user of the service. Group the results separately for male and female users. Name the column with the month number of the youngest user as max_month and convert the values in this column to INTEGER format.
### Resulting Table Fields:
sex
max_month
### SQL Query:
```sql
SELECT sex,
       DATE_PART('month', MAX(birth_date))::INTEGER AS max_month
FROM   users
GROUP BY sex
ORDER BY sex;
```
### Explanation:
This query calculates the month number of birth for the youngest user by finding the maximum birth_date and extracting the month using DATE_PART. The results are grouped by sex and converted to INTEGER format, aliased as max_month, and sorted by gender.

## Task 7
Description:
Count the maximum age of users by gender in the users table. Measure age in full years. Name the new column with age as max_age and convert the values in this column to INTEGER format.
### Resulting Table Fields:
sex
max_age
### SQL Query:
```sql
Copy code
SELECT sex,
       DATE_PART('year', MAX(AGE(birth_date))) AS max_age
FROM   users
GROUP BY sex
ORDER BY max_age;
```
### Explanation:
This query calculates the maximum age of users by using the AGE function on the birth_date. The result is converted to years and aliased as max_age. The results are grouped by gender and sorted by age in ascending order.

## Task 8
### Description:
Group users from the users table by age (measured in full years) and count the number of users of each age. Name the age column as age and the column with the number of users as users_count. Convert the values in the age column to INTEGER format. Sort the results by the age column in ascending order.
### Resulting Table Fields:
age
users_count
### SQL Query:
```sql
SELECT DATE_PART('year', AGE(birth_date)) AS age,
       COUNT(user_id) AS users_count
FROM   users
GROUP BY age
ORDER BY age;
```
### Explanation:
This query groups users by age calculated from their birth_date and counts the number of users at each age. The age is converted to INTEGER format and aliased as age, while the user count is aliased as users_count. The results are sorted by age in ascending order.

## Task 9
### Description:
Again, group users from the users table by age (measured in full years), but now also include gender in the grouping. Count the number of users in each age-gender group. Filter out all NULL values in the birth_date column using WHERE.

### Resulting Table Fields:
age
sex
users_count
### SQL Query:
```sql
SELECT DATE_PART('year', AGE(birth_date)) AS age,
       sex,
       COUNT(user_id) AS users_count
FROM   users
WHERE  birth_date IS NOT NULL
GROUP BY age, sex
ORDER BY age, sex;
```
### Explanation:
This query groups users by age and gender, counting the number of users in each age-gender group. It filters out users with NULL birth_date values. The age is converted to INTEGER format, and the results are sorted first by age and then by gender.

## Task 10
### Description:
Count the number of products in each order, apply grouping, and calculate the number of orders for each group during the week from August 29 to September 4, 2022. Use data from the orders table. Output two columns: order size and the number of orders of that size during the specified period. Name the columns order_size and orders_count.

### Resulting Table Fields:
order_size
orders_count
### SQL Query:
```sql
SELECT ARRAY_LENGTH(product_ids, 1) AS order_size,
       COUNT(order_id) AS orders_count
FROM   orders
WHERE  creation_time BETWEEN '2022-08-29' AND '2022-09-04'
GROUP BY order_size
ORDER BY order_size;
```
### Explanation:
This query counts the number of products in each order by using the ARRAY_LENGTH function on product_ids. It filters orders placed within the specified date range and groups by order_size. The results are sorted by order_size in ascending order.





## Task 11

### Description:
Count the number of items in each order, apply grouping, and calculate the number of orders for each group. Consider only orders placed on weekdays. Include only those order sizes where the total number exceeds 2000. Use data from the `orders` table. Output two columns: order size and the number of orders of that size. Name the columns `order_size` and `orders_count`. Sort the results by order size in ascending order.

### Resulting Table Fields:
- **order_size**
- **orders_count**

### SQL Query:
```sql
SELECT ARRAY_LENGTH(product_ids, 1) AS order_size,
       COUNT(order_id) AS orders_count
FROM   orders
WHERE  DATE_PART('dow', creation_time) NOT IN (0, 6) -- Exclude weekends
GROUP BY order_size
HAVING COUNT(order_id) > 2000
ORDER BY order_size;
```
### Explanation:
This query calculates the number of items in each order using the ARRAY_LENGTH function on product_ids. It filters the results to include only weekdays (excluding Saturdays and Sundays) and groups by order_size. The results are filtered to include only those sizes with more than 2000 orders, and sorted by order_size in ascending order.

## Task 12
### Description:
From the user_actions table, determine the five users who made the most orders in August 2022. Output two columns: user IDs and the number of orders they placed. Name the column with the number of orders as created_orders. Sort the result first by the number of orders in descending order, then by user ID in ascending order.

### Resulting Table Fields:
user_id
created_orders
### SQL Query:
```sql
SELECT user_id,
       COUNT(order_id) AS created_orders
FROM   user_actions
WHERE  DATE_PART('month', time) = 8 AND DATE_PART('year', time) = 2022
GROUP BY user_id
ORDER BY created_orders DESC, user_id
LIMIT 5;
```
### Explanation:
This query counts the number of orders made by each user in August 2022, filtering by the time column. The results are grouped by user_id and sorted first by the number of orders in descending order and then by user_id in ascending order, limited to the top five users.

## Task 13
### Description:
From the courier_actions table, identify couriers who delivered only one order in September 2022. Output only one column with courier IDs. Sort the result by courier ID in ascending order.

### Resulting Table Fields:
courier_id
### SQL Query:
```sql
SELECT courier_id
FROM   courier_actions
WHERE  action = 'deliver_order'
   AND DATE_PART('month', time) = 9
   AND DATE_PART('year', time) = 2022
GROUP BY courier_id
HAVING COUNT(DISTINCT order_id) = 1
ORDER BY courier_id;
```
### Explanation:
This query counts the number of distinct orders delivered by each courier in September 2022. It groups the results by courier_id and filters those who delivered only one order. The results are sorted by courier_id in ascending order.

## Task 14
Description:
From the user_actions table, select users whose last order was created before September 8, 2022. Output only their IDs; do not include the creation date of the order. Sort the result by user ID in ascending order.

### Resulting Table Fields:
user_id
### SQL Query:
```sql
SELECT user_id
FROM   user_actions
WHERE  action = 'create_order'
GROUP BY user_id
HAVING MAX(time) < '2022-09-08'
ORDER BY user_id;
```
### Explanation:
This query retrieves user IDs from the user_actions table whose latest order was placed before September 8, 2022. It groups results by user_id and filters using the HAVING clause based on the maximum order creation time. The results are sorted by user_id in ascending order.

## Task 15
### Description:
Group the orders from the orders table into three categories based on the number of items in each order:
Small (1 to 3 items)
Medium (4 to 6 items)
Large (7 or more items)
Count the number of orders that fall into each group. Name the groups "Small", "Medium", and "Large". Output the names of the groups and the number of orders in each group. Name the columns order_size and orders_count. Sort the result by the number of orders in ascending order.

### Resulting Table Fields:
order_size
orders_count
### SQL Query:
```sql
SELECT CASE 
           WHEN ARRAY_LENGTH(product_ids, 1) BETWEEN 1 AND 3 THEN 'Small'
           WHEN ARRAY_LENGTH(product_ids, 1) BETWEEN 4 AND 6 THEN 'Medium'
           ELSE 'Large' 
       END AS order_size,
       COUNT(order_id) AS orders_count
FROM   orders
GROUP BY order_size
ORDER BY orders_count;
```
###  Explanation:
This query categorizes orders based on the number of items using the ARRAY_LENGTH function on product_ids. It counts the number of orders in each category (Small, Medium, Large) and groups the results accordingly. The results are sorted by the number of orders in ascending order.

## Task 16
### Description:
Group users from the users table into four age categories:

18 to 24 years
25 to 29 years
30 to 35 years
36 years and older
Count the number of users in each age group. Name the age groups "18-24", "25-29", "30-35", and "36+". Exclude users without a birth date from the calculations. Measure age in full years.

### Resulting Table Fields:
group_age
users_count
### SQL Query:
```sql
SELECT CASE 
           WHEN DATE_PART('year', AGE(birth_date)) BETWEEN 18 AND 24 THEN '18-24'
           WHEN DATE_PART('year', AGE(birth_date)) BETWEEN 25 AND 29 THEN '25-29'
           WHEN DATE_PART('year', AGE(birth_date)) BETWEEN 30 AND 35 THEN '30-35'
           ELSE '36+' 
       END AS group_age,
       COUNT(user_id) AS users_count
FROM   users
WHERE  birth_date IS NOT NULL
GROUP BY group_age
ORDER BY group_age;
```
### Explanation:
This query categorizes users by age and counts the number of users in each age group. It filters out users with NULL birth_date values, and the results are sorted by the group names in ascending order.

## Task 17
### Description:
From the orders table, calculate the average order size for weekends and weekdays. Name the weekend group "weekend" and the weekday group "weekdays". Output two columns: name the column with the groups as week_part and the column with the average order size as avg_order_size. Round the average order size to two decimal places. Sort the results by the average order size in ascending order.

Resulting Table Fields:
week_part
avg_order_size
### SQL Query:
```sql
SELECT CASE 
           WHEN TO_CHAR(creation_time, 'Dy') IN ('Sat', 'Sun') THEN 'weekend'
           ELSE 'weekdays' 
       END AS week_part,
       ROUND(AVG(ARRAY_LENGTH(product_ids, 1)), 2) AS avg_order_size
FROM   orders
GROUP BY week_part
ORDER BY avg_order_size;
```
### Explanation:
This query categorizes orders by whether they were placed on weekends or weekdays and calculates the average size of orders in each category. The average order size is rounded to two decimal places, and the results are sorted by the average order size in ascending order.

## Task 18
### Description:
For each user in the user_actions table, calculate the total number of orders placed and the cancellation rate of those orders. Name the new columns orders_count and cancel_rate, rounding the cancellation rate to two decimal places. Include only users who have placed more than three orders and have a cancellation rate of at least 0.5. Sort the results by user ID in ascending order.

### Resulting Table Fields:
user_id
orders_count
cancel_rate
### SQL Query:
```sql
SELECT user_id,
       ROUND(COUNT(DISTINCT order_id) FILTER (WHERE action = 'cancel_order')::DECIMAL / COUNT(DISTINCT order_id), 2) AS cancel_rate,
       COUNT(DISTINCT order_id) AS orders_count
FROM   user_actions
GROUP BY user_id
HAVING COUNT(DISTINCT order_id) > 3 AND 
       ROUND(COUNT(DISTINCT order_id) FILTER (WHERE action = 'cancel_order')::DECIMAL / COUNT(DISTINCT order_id), 2) >= 0.5
ORDER BY user_id;
```
### Explanation:
This query calculates the total number of orders placed by each user and the rate of canceled orders. It filters for users with more than three total orders and a cancellation rate of at least 0.5. The results are sorted by user_id in ascending order.

## Task 19
### Description:
For each day of the week in the user_actions table, calculate:

The total number of orders placed.
The total number of canceled orders.
The total number of non-canceled orders (i.e., delivered).
The success rate of non-canceled orders.
Name the new columns created_orders, canceled_orders, actual_orders, and success_rate, rounding the success rate to three decimal places. Perform calculations for the period from August 24 to September 6, 2022, to include a complete range of weekdays. Group the results by the day of the week.

### Resulting Table Fields:
weekday_number
weekday
created_orders
canceled_orders
actual_orders
success_rate
### SQL Query:
```sql
SELECT DATE_PART('isodow', time)::INT AS weekday_number,
       TO_CHAR(time, 'Dy') AS weekday,
       COUNT(order_id) FILTER (WHERE action = 'create_order') AS created_orders,
       COUNT(order_id) FILTER (WHERE action = 'cancel_order') AS canceled_orders,
       COUNT(order_id) FILTER (WHERE action = 'create_order') - COUNT(order_id) FILTER (WHERE action = 'cancel_order') AS actual_orders,
       ROUND((COUNT(order_id) FILTER (WHERE action = 'create_order') - COUNT(order_id) FILTER (WHERE action = 'cancel_order'))::DECIMAL / COUNT(order_id) FILTER (WHERE action = 'create_order'), 3) AS success_rate
FROM   user_actions
WHERE  time >= '2022-08-24' AND time < '2022-09-07'
GROUP BY weekday_number, weekday
ORDER BY weekday_number;
```
### Explanation:
This query calculates the total number of orders created, canceled, and delivered for each day of the week. It also calculates the success rate of non-canceled orders. The results are grouped by the day of the week and sorted by the weekday number in ascending order.


