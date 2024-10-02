# Basic SQL Queries

## Task 1
### Description:
Retrieve all records from the `products` table.
### Resulting Table Fields:
- **product_id**
- **name**
- **price**
### SQL Query:
```sql
SELECT product_id,
       name,
       price
FROM   products;
```
### Explanation:
This query selects the `product_id`, `name`, and `price` fields from the `products` table without any filters or conditions, retrieving all entries.

## Task 2
### Description:
Retrieve all records from the `products` table, sorting them by product names in alphabetical order (ascending).

### Resulting Table Fields:
-   **product_id**
-   **name**
-   **price**

### SQL Query:
```sql
SELECT product_id,
       name,
       price
FROM   products
ORDER BY name;
```
### Explanation:
This query selects the same fields as Task 1 but adds the `ORDER BY name` clause to sort the results alphabetically based on the `name` of the products in ascending order.

## Task 3
### Description:
Sort the `courier_actions` table first by the `courier_id` in ascending order, then by the `action` column (also in ascending order), and finally by the `time` column in descending order---from the most recent action to the oldest. Use the `LIMIT` clause to return only the first 1000 rows of the resulting table.
### Resulting Table Fields:
-   **courier_id**
-   **order_id**
-   **action**
-   **time**
### SQL Query:
```sql
SELECT courier_id,
       order_id,
       action,
       time
FROM   courier_actions
ORDER BY courier_id, action, time DESC
LIMIT 1000;
```
### Explanation:
This query retrieves the `courier_id`, `order_id`, `action`, and `time` fields from the `courier_actions` table. It sorts the results first by `courier_id` in ascending order, then by `action` also in ascending order, and finally by `time` in descending order to show the most recent actions first. The `LIMIT 1000` clause restricts the output to the first 1000 records.


## Task 4
### Description:
Using the `SELECT`, `FROM`, `ORDER BY`, and `LIMIT` operators, identify the 5 most expensive products in the `products` table that our service delivers. Display their names and prices.
### Resulting Table Fields:
- **name**
- **price**
### SQL Query:
```sql
SELECT name,
       price
FROM   products
ORDER BY price DESC
LIMIT 5;
```
### Explanation:
This query retrieves the `name` and `price` of the products from the `products` table. It sorts the results in descending order based on the `price`, ensuring that the most expensive products appear first. The `LIMIT 5` clause restricts the output to the top 5 entries.



## Task 5
### Description:
Repeat the previous query, but rename the columns `name` and `price` to `product_name` and `product_price`, respectively.
### Resulting Table Fields:
-   **product_name**
-   **product_price**

### SQL Query:
```sql
SELECT name AS product_name,
       price AS product_price
FROM   products
ORDER BY price DESC
LIMIT 5;
```
### Explanation:
This query selects the `name` and `price` fields from the `products` table, renaming them to `product_name` and `product_price` using the `AS` keyword. The results are sorted in descending order by `price`, and the `LIMIT 5` clause again restricts the output to the top 5 most expensive products.


## Task 6
### Description:
Using the `SELECT`, `FROM`, `ORDER BY`, and `LIMIT` operators, along with the `LENGTH` function, determine the product with the longest name in the `products` table. Display its name, the length of the name in characters, and the price of the product. Name the column with the length of the name as `name_length`.
### Resulting Table Fields:
- **name**
- **name_length**
- **price**

### SQL Query:
```sql
SELECT name,
       LENGTH(name) AS name_length,
       price
FROM   products
ORDER BY name_length DESC
LIMIT 1;
```
### Explanation:
This query selects the `name` and `price` fields from the `products` table, and calculates the length of each product's name using the `LENGTH` function, aliasing it as `name_length`. The results are sorted in descending order based on `name_length`, ensuring that the product with the longest name appears first. The `LIMIT 1` clause restricts the output to only the top entry.


## Task 7
### Description:
Apply the `UPPER` and `SPLIT_PART` functions sequentially to the `name` column to transform the product names in the `products` table, retaining only the first word in uppercase. Name the column containing the new first word as `first_word`. Include the original product names, the new first word, and the product prices in the result. Sort the output by the original product names in ascending order.
### Resulting Table Fields:
- **name**
- **first_word**
- **price**

### SQL Query:
```sql
SELECT name,
       UPPER(SPLIT_PART(name, ' ', 1)) AS first_word,
       price
FROM   products
ORDER BY name;
```

### Explanation:
This query selects the name and price fields from the products table. It uses the SPLIT_PART function to extract the first word from the name column, and then applies the UPPER function to convert it to uppercase, aliasing it as first_word. The results are sorted in ascending order based on the original product names in the name column.
















