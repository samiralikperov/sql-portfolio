# Data Grouping

## SQL Task Links

| Task Number | Task Description                                     | Link                                     |
|-------------|-----------------------------------------------------|------------------------------------------|
| Task 1      | Select products where the price does not exceed 100 rubles | [View Task 1](#task-1)                  |



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

