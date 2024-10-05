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






