# SQL Simulator Assignments

Welcome to my collection of completed assignments from the SQL simulator!  
This repository features a variety of tasks across different topics, ranging from beginner to advanced levels. Each assignment is designed to enhance my SQL skills and understanding of key concepts.  

Feel free to explore the solutions and insights I gained through this learning process!

The diagram below shows the relationships between the tables:
![2023_01_24_214337_negate](https://github.com/user-attachments/assets/67e1bf10-e3d4-4aa9-bb3e-37a65d3d729b)

# Structure and Content of Tables

## user_actions — User actions with orders.

| Column      | Data Type     | Description                                                        |
|-------------|----------------|--------------------------------------------------------------------|
| user_id     | INT            | User ID                                                           |
| order_id    | INT            | Order ID                                                          |
| action      | VARCHAR(50)    | User action with the order; 'create_order' — creating an order, 'cancel_order' — canceling an order |
| time        | TIMESTAMP      | Time of the action                                                |

## courier_actions — Courier actions with orders.

| Column      | Data Type     | Description                                                        |
|-------------|----------------|--------------------------------------------------------------------|
| courier_id  | INT            | Courier ID                                                        |
| order_id    | INT            | Order ID                                                          |
| action      | VARCHAR(50)    | Courier action with the order; 'accept_order' — accepting an order, 'deliver_order' — delivering an order |
| time        | TIMESTAMP      | Time of the action                                                |

## orders — Information about orders.

| Column          | Data Type     | Description                                                  |
|-----------------|----------------|--------------------------------------------------------------|
| order_id        | INT            | Order ID                                                   |
| creation_time   | TIMESTAMP      | Time of order creation                                     |
| product_ids     | integer[]      | List of product IDs in the order                          |

## users — Information about users.

| Column      | Data Type     | Description                                                  |
|-------------|----------------|--------------------------------------------------------------|
| user_id     | INT            | User ID                                                   |
| birth_date  | DATE           | Date of birth                                             |
| sex         | VARCHAR(50)    | Gender; 'male' — male, 'female' — female                  |

## couriers — Information about couriers.

| Column      | Data Type     | Description                                                  |
|-------------|----------------|--------------------------------------------------------------|
| courier_id  | INT            | Courier ID                                                |
| birth_date  | DATE           | Date of birth                                            |
| sex         | VARCHAR(50)    | Gender; 'male' — male, 'female' — female                  |

## products — Information about products delivered by the service.

| Column      | Data Type     | Description                                                  |
|-------------|----------------|--------------------------------------------------------------|
| product_id  | INT            | Product ID                                               |
| name        | VARCHAR(50)    | Product name                                            |
| price       | FLOAT(4)       | Product price                                           |

# SQL Tasks Links


| Task Number | Task Description                                     | Link                                     |
|-------------|-----------------------------------------------------|------------------------------------------|
| Task 1      | Retrieve all records from the `products` table     | [View Task 1](#task-1)                  |
| Task 2      | Sort records from the `products` table by name     | [View Task 2](#task-2)                  |
| Task 3      | Sort `courier_actions` by multiple criteria         | [View Task 3](#task-3)                  |
| Task 4      | Identify the 5 most expensive products              | [View Task 4](#task-4)                  |
| Task 5      | Rename columns in the previous query                 | [View Task 5](#task-5)                  |



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

























