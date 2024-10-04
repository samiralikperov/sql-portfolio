# Data Filtering

## SQL Task Links

| Task Number | Task Description                                     | Link                                     |
|-------------|-----------------------------------------------------|-------------------------------------------|


# SQL Tasks

## Task 1

### Description:
Write an SQL query to select all information about products from the `products` table where the price does not exceed 100 rubles. Sort the result by the product ID in ascending order.

### Resulting Table Fields:
- **product_id**
- **name**
- **price**

### SQL Query:
```sql
SELECT product_id,
       name,
       price
FROM   products
WHERE  price <= 100
ORDER BY product_id;
```
### Explanation:
This query retrieves the product_id, name, and price fields from the products table. It uses the WHERE clause to filter the results, returning only those products with a price of 100 rubles or less. The ORDER BY product_id clause sorts the results in ascending order based on the product_id.

## Task 2
### Description:
Select female users from the users table and output only their IDs. Sort the result by the user ID in ascending order. Use the LIMIT operator to return only the first 1000 IDs from the sorted list.
### Resulting Table Fields:
user_id
### SQL Query:
```sql
SELECT user_id
FROM   users
WHERE  sex = 'female'
ORDER BY user_id
LIMIT 1000;
```
###  Explanation:
This query selects the user_id from the users table, filtering the results to include only users of female gender using the WHERE clause. The results are sorted in ascending order by user_id, and the LIMIT 1000 clause restricts the output to the first 1000 entries.

## Task 3
### Description:
Select all user actions related to creating orders from the user_actions table that occurred after midnight on September 6, 2022. Output the columns for user ID, order ID, and the time of the actions. Sort the results by order ID in ascending order.

### Resulting Table Fields:
user_id
order_id
time
### SQL Query:
```sql
SELECT user_id,
       order_id,
       time
FROM   user_actions
WHERE  action = 'create_order'
   AND time > '2022-09-06'
ORDER BY order_id;
```
### Explanation:
This query retrieves the user_id, order_id, and time fields from the user_actions table, filtering for actions where the action is 'create_order' and the time is after midnight on September 6, 2022. The results are sorted in ascending order based on order_id.

## Task 4
### Description:
Apply a 20% discount to all products in the products table and select those for which the discounted price exceeds 100 rubles. Output the product IDs, names, original price, and new price after applying the discount. Name the column with the old price as old_price and the column with the new price as new_price.

### Resulting Table Fields:
product_id
name
old_price
new_price
### SQL Query:
```sql
SELECT product_id,
       name,
       price AS old_price,
       price * 0.8 AS new_price
FROM   products
WHERE  price * 0.8 > 100
ORDER BY product_id;
```
### Explanation:
This query selects the product_id, name, and price from the products table, where price is aliased as old_price, and the new price is calculated by applying a 20% discount (multiplying the price by 0.8) and aliased as new_price. The WHERE clause filters for products where the discounted price exceeds 100 rubles, and the results are sorted in ascending order by product_id.

## Task 5
### Description:
Select all products from the products table where the product names either start with the word "чай" (tea) or consist of five characters. Output the product IDs and their names. Sort the results by product ID in ascending order.

### Resulting Table Fields:
product_id
name
### SQL Query:
```sql
SELECT product_id,
       name
FROM   products
WHERE  SPLIT_PART(name, ' ', 1) = 'чай'
    OR LENGTH(name) = 5
ORDER BY product_id;
```
### Explanation:
This query retrieves the product_id and name from the products table, filtering results to include products whose names start with "чай" or have a length of five characters. The results are sorted by product_id in ascending order.


