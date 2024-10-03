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


## Task 8

### Description:
Change the data type of the `price` column in the `products` table to `VARCHAR`. Display the columns with product names, the original price format, and the price in the new `VARCHAR` format. Name the new column with the price in `VARCHAR` format as `price_char`. Sort the results by the original product names in ascending order. There should be no limit on the number of records displayed.

### Resulting Table Fields:
- **name**
- **price**
- **price_char**

### SQL Query:
```sql
SELECT name,
       price,
       CAST(price AS VARCHAR) AS price_char
FROM   products
ORDER BY name;
```
### Explanation:
This query selects the name and price fields from the products table. It uses the CAST function to convert the price column from its original data type to VARCHAR, creating a new column named price_char. The results are sorted in ascending order based on the original product names in the name column, and there is no limit on the number of records displayed.



## Task 9

### Description:
For the first 200 records from the `orders` table, output the information in the following format (pay attention to the spaces):
**Order № [order_id] created [creation_time]**
Name the resulting column as `order_info`.
### Resulting Table Fields:
- **order_info**
### SQL Query:
```sql
SELECT CONCAT('Заказ № ', order_id, ' создан ', DATE(creation_time)) AS order_info
FROM   orders
LIMIT 200;
```
### Explanation:
This query selects the order_id and creation_time fields from the orders table. It uses the CONCAT function to combine the static text "Заказ №", the order_id, the static text "создан", and the formatted creation_time (converted to date) into a single string, which is aliased as order_info. The LIMIT 200 clause restricts the output to the first 200 records.





## Task 10
### Description:
Retrieve the IDs of all couriers and their birth years from the `couriers` table. The birth year should be extracted from the `birth_date` column. Name the new column with the year as `birth_year`. Sort the results first by the birth year in descending order (from youngest to oldest), and then by the courier ID in ascending order.
### Resulting Table Fields:
- **courier_id**
- **birth_year**
### SQL Query:
```sql
SELECT courier_id,
       DATE_PART('year', birth_date) AS birth_year
FROM   couriers
ORDER BY birth_year DESC, courier_id;
```
### Explanation:
This query selects the courier_id and extracts the year from the birth_date field using the DATE_PART function, aliasing it as birth_year. The results are sorted in descending order by birth_year to list the youngest couriers first, and then by courier_id in ascending order to maintain a consistent ordering among couriers born in the same year.


## Task 11
### Description:
Similar to the previous task, retrieve the IDs of all couriers and their birth years from the `couriers` table. Apply the `COALESCE` function to the extracted year so that instead of `NULL` values, the result will contain the text value "unknown". Keep the field names the same as before. Sort the final table first by the birth year in descending order (from youngest to oldest) and then by the courier ID in ascending order.
### Resulting Table Fields:
- **courier_id**
- **birth_year**
### SQL Query:
```sql
SELECT courier_id,
       COALESCE(DATE_PART('year', birth_date)::VARCHAR, 'unknown') AS birth_year
FROM   couriers
ORDER BY birth_year DESC, courier_id;
```
### Explanation:
This query selects the courier_id and extracts the year from the birth_date field using the DATE_PART function. The COALESCE function is applied to replace any NULL values in the year with the text "unknown". The results are sorted in descending order by birth_year to show the youngest couriers first, and then by courier_id in ascending order to maintain a consistent ordering among couriers with the same birth year.



## Task 12
### Description:
Imagine that for some inexplicable reason, we decided to increase the price of all products in the `products` table by 5%. Output the IDs and names of all products, along with their old and new prices. Name the column with the old price as `old_price` and the column with the new price as `new_price`. 
Sort the results first by the new price in descending order and then by the product ID in ascending order.
### Resulting Table Fields:
- **product_id**
- **name**
- **old_price**
- **new_price**
### SQL Query:
```sql
SELECT product_id,
       name,
       price AS old_price,
       price * 1.05 AS new_price
FROM   products
ORDER BY new_price DESC, product_id;
```
### Explanation:
This query selects the product_id, name, and price from the products table. The price is aliased as old_price, and the new price is calculated by multiplying the old price by 1.05, aliased as new_price. The results are sorted in descending order by new_price to show the most expensive products first, and then by product_id in ascending order to maintain a consistent ordering among products.


## Task 13

### Description:
Once again, increase the price of all products by 5%. However, this time apply the `ROUND` function to the column with the new price. Output the IDs and names of the products, their old prices, and the new prices rounded to one decimal place. Do not change the data type of the new price.
Sort the results first by the new price in descending order and then by the product ID in ascending order.
### Resulting Table Fields:
- **product_id**
- **name**
- **old_price**
- **new_price**
### SQL Query:
```sql
SELECT product_id,
       name,
       price AS old_price,
       ROUND(price * 1.05, 1) AS new_price
FROM   products
ORDER BY new_price DESC, product_id;
```
### Explanation:
This query selects the product_id, name, and price from the products table. The price is aliased as old_price, and the new price is calculated by multiplying the old price by 1.05 and then rounding it to one decimal place using the ROUND function, aliasing it as new_price. The results are sorted in descending order by new_price to show the most expensive products first, and then by product_id in ascending order to maintain a consistent ordering among products.




## Task 14
### Description:
Increase the price by 5% only for those products whose price exceeds 100 rubles. Leave the prices of other products unchanged. Additionally, do not raise the price of caviar, which already costs 800 rubles. Output the IDs and names of all products, their old prices, and their new prices without rounding.
Sort the results first by the new price in descending order and then by the product ID in ascending order.
### Resulting Table Fields:
- **product_id**
- **name**
- **old_price**
- **new_price**
### SQL Query:
```sql
SELECT product_id,
       name,
       price AS old_price,
       CASE 
           WHEN price <= 100 OR name = 'икра' THEN price
           WHEN price > 100 THEN price * 1.05
           ELSE 0 
       END AS new_price
FROM   products
ORDER BY new_price DESC, product_id;
```
### Explanation:
This query selects the product_id, name, and price from the products table, with price aliased as old_price. It uses a CASE statement to determine the new_price: if the price is less than or equal to 100 rubles or if the product name is 'икра', it retains the original price; if the price is greater than 100 rubles, it increases the price by 5%. The results are sorted in descending order by new_price to display the most expensive products first, followed by the product ID in ascending order.



## Task 15
### Description:
Calculate the VAT for each product in the `products` table and compute the price excluding VAT. Output all information about the products, including the tax amount and the price before tax. Name the columns for the tax amount and the price before VAT as `tax` and `price_before_tax`, respectively. Round the values in these columns to two decimal places.
Sort the results first by the price before VAT in descending order and then by the product ID in ascending order.
### Resulting Table Fields:
- **product_id**
- **name**
- **price**
- **tax**
- **price_before_tax**
### SQL Query:
```sql
SELECT product_id,
       name,
       price,
       ROUND(price / 120 * 20, 2) AS tax,
       ROUND(price - price / 120 * 20, 2) AS price_before_tax
FROM   products
ORDER BY price_before_tax DESC, product_id;
```
### Explanation:
This query selects the product_id, name, and price from the products table. It calculates the VAT (tax) as 20% of the price divided by 120, rounding it to two decimal places. The price before tax (price_before_tax) is calculated by subtracting the VAT from the original price, also rounded to two decimal places. The results are sorted in descending order by price_before_tax to show the most expensive products first, and then by product_id in ascending order.



