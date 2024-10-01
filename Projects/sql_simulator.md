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

## Task 2

### Description:
Retrieve all records from the `products` table, sorting them by product names in alphabetical order (ascending). Use the `ORDER BY` clause for sorting.

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
ORDER BY name;

