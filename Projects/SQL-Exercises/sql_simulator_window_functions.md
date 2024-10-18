## Window functions

## SQL Task Links

|               |                 |               |             |                |
|-----------------------|-----------------------|-----------------------|-----------------------|-----------------------|
| [Task 1](#task-1)     | [Task 2](#task-2)     | [Task 3](#task-3)     | [Task 4](#task-4)     | [Task 5](#task-5)     |
| [Task 6](#task-6)     | [Task 7](#task-7)     | [Task 8](#task-8)     | [Task 9](#task-9)     | [Task 10](#task-10)   |
| [Task 11](#task-11)   | [Task 12](#task-12)   | [Task 13](#task-13)   | [Task 14](#task-14)   | [Task 15](#task-15)   |
| [Task 16](#task-16)   | [Task 17](#task-17)   | [Task 18](#task-18)   |              




## Task 1
### Description:
Apply window functions to the products table to rank all products by price, from the most expensive to the least expensive. Add the following columns to the output:
A column product_number with the sequential number of the product using the ROW_NUMBER function.
A column product_rank with the rank of the product allowing for gaps using the RANK function.
A column product_dense_rank with the rank of the product without gaps using the DENSE_RANK function.
Ensure to specify the ordering of records in the window functions, as failing to do so may lead to incorrect results. Partitioning within the window is not required. No sorting of records in the resulting table is necessary.

### SQL Query:
```sql

SELECT product_id,
       name,
       price,
       ROW_NUMBER() OVER (ORDER BY price DESC) AS product_number,
       RANK() OVER (ORDER BY price DESC) AS product_rank,
       DENSE_RANK() OVER (ORDER BY price DESC) AS product_dense_rank
FROM   products;
```
### Explanation:
This query selects the product_id, name, and price from the products table. It applies three different ranking functions to assign ranks based on the price of each product:

ROW_NUMBER() assigns a unique sequential number to each product ordered by price in descending order.
RANK() assigns ranks to products with gaps in the ranking for ties.
DENSE_RANK() assigns ranks without gaps, ensuring that tied products receive the same rank, but the next product in order receives the next consecutive rank.




## Task 2
### Description:
Apply a window function to the products table to assign the price of the most expensive product in a new column named max_price for each record. Then, calculate the share of each product's price relative to the most expensive product's price by dividing the product's price by max_price. Round the resulting shares to two decimal places and name the column share_of_max. Output all information about the products, including the values in the new columns. Sort the results first by the price of the product in descending order and then by the product ID in ascending order.

### SQL Query:
```sql

SELECT product_id,
       name,
       price,
       MAX(price) OVER () AS max_price,
       ROUND(price / MAX(price) OVER (), 2) AS share_of_max
FROM   products
ORDER BY price DESC, product_id;
```
### Explanation:
This query retrieves the product_id, name, and price from the products table. It uses the MAX(price) OVER () function to calculate the highest price across all products and assigns this value to the max_price column for each row. It then calculates the share of each product's price relative to max_price and rounds the result to two decimal places, storing it in the share_of_max column. The final output is sorted by the product price in descending order and by product_id in ascending order, providing a clear overview of the products and their price ratios.




## Task 3
### Description:
Apply two window functions to the products table: one using the MAX function to compute the maximum price and another using the MIN function to compute the minimum price. For both window functions, set the ORDER BY clause to sort the results by price in descending order. Place the results of these computations in two columns named max_price and min_price. Output all information about the products, including the values in the new columns. Sort the final results first by the price of the product in descending order and then by the product ID in ascending order.

### SQL Query:
```sql

SELECT product_id,
       name,
       price,
       MAX(price) OVER (ORDER BY price DESC) AS max_price,
       MIN(price) OVER (ORDER BY price DESC) AS min_price
FROM   products
ORDER BY price DESC, product_id;
```
### Explanation:
This query retrieves the product_id, name, and price from the products table. It applies the MAX(price) OVER (ORDER BY price DESC) function to calculate the maximum price across all products, regardless of the current row, and stores this value in the max_price column. Similarly, it applies the MIN(price) OVER (ORDER BY price DESC) function to find the minimum price and places this value in the min_price column. The final output includes all product information along with the calculated maximum and minimum prices, sorted by product price in descending order and then by product_id in ascending order, providing a comprehensive view of the products and their price metrics.





## Task 4
### Description:
First, create a new table from the orders table that shows the total number of orders by day, excluding cancelled orders (which can be identified using the user_actions table). Name the column with the dates date and the column with the number of orders orders_count.

Next, use this resulting table in a subquery and apply a window function with the aggregate function SUM to calculate the cumulative sum of the number of orders. Remember to set the window's ORDER BY clause by date. Name the column with the cumulative sum orders_cum_count. The cumulative sum for the last day should equal the total number of orders for the entire period. No sorting of the resulting table is required.

### SQL Query:
```sql

SELECT date,
       orders_count,
       SUM(orders_count) OVER (ORDER BY date) AS orders_cum_count
FROM   (SELECT DATE(creation_time) AS date,
               COUNT(order_id) AS orders_count
        FROM   orders
        WHERE  order_id NOT IN (SELECT order_id
                                FROM   user_actions
                                WHERE  action = 'cancel_order')
        GROUP BY date) t;
```
### Explanation:
This query begins by creating a subquery that counts the number of orders for each day from the orders table. It filters out cancelled orders using a subquery that retrieves order_ids from the user_actions table where the action is 'cancel_order'. The outer query then takes this result and calculates the cumulative number of orders by applying the SUM function as a window function ordered by date. The final output includes the date, the count of orders for that day, and the cumulative count of orders, which allows for an easy assessment of order trends over time without needing to sort the final results.


## Task 5
### Description:
For each user in the user_actions table, calculate the sequential number of each order. Apply the ROW_NUMBER window function to the order time column, ensuring to partition by users and sort within each partition. Exclude cancelled orders from the calculations. Name the new column with the order number order_number. Sort the results first by user ID in ascending order and then by order number in ascending order. Use a LIMIT clause to display only the first 1000 rows of the resulting table.

### SQL Query:
```sql

SELECT user_id,
       order_id,
       time,
       ROW_NUMBER() OVER (PARTITION BY user_id
                          ORDER BY time) AS order_number
FROM   user_actions
WHERE  order_id NOT IN (SELECT order_id
                        FROM   user_actions
                        WHERE  action = 'cancel_order')
ORDER BY user_id, order_number
LIMIT 1000;
```
### Explanation:
This query retrieves the user_id, order_id, and time from the user_actions table. It applies the ROW_NUMBER() function, partitioning the results by user_id and ordering them by time to assign a sequential number to each order for each user. The query filters out cancelled orders using a subquery that selects order_ids associated with the 'cancel_order' action. The results are then sorted by user_id and order_number, providing a clear view of each user's orders in the desired order. The LIMIT 1000 clause restricts the output to the first 1000 rows, making it more manageable for analysis.





## Task 6
### Description:
Enhance the previous query to calculate the time elapsed since the previous order for each order made by each user. To achieve this, first use the LAG function to create a new column that shifts the time column back by one value, naming this column time_lag. Then, subtract the time_lag from the current time to compute the interval, naming this column time_diff. The time difference should maintain a format similar to "3 days, 12:18:22". Continue to exclude cancelled orders and retain the sequential order number calculated in the previous step. Sort the results first by user ID in ascending order and then by order number in ascending order. Use a LIMIT clause to display only the first 1000 rows of the resulting table.

### SQL Query:
```sql

SELECT user_id,
       order_id,
       time,
       ROW_NUMBER() OVER (PARTITION BY user_id
                          ORDER BY time) AS order_number,
       LAG(time, 1) OVER (PARTITION BY user_id
                          ORDER BY time) AS time_lag,
       time - LAG(time, 1) OVER (PARTITION BY user_id
                                 ORDER BY time) AS time_diff
FROM   user_actions
WHERE  order_id NOT IN (SELECT order_id
                        FROM   user_actions
                        WHERE  action = 'cancel_order')
ORDER BY user_id, order_number
LIMIT 1000;
```
### Explanation:
This query builds on the previous task by adding a calculation for the time difference between each order and the previous order made by the same user. It uses the LAG function to obtain the timestamp of the previous order, allowing the calculation of the time difference. The result will include the user ID, order ID, order time, order number, the lagged time, and the calculated time difference. The ORDER BY clause ensures the results are sorted by user ID and order number, while the LIMIT 1000 clause restricts the output to the first 1000 rows for easier analysis.




## Task 7
### Description:
Based on the previous query, calculate the average time between orders for each user, considering only those users who have placed more than one non-cancelled order. Express the average time between orders in hours, rounding the values to the nearest whole number. Name the column with the average time hours_between_orders. Sort the results by user ID in ascending order. Use a LIMIT clause to display only the first 1000 records.

### SQL Query:
```sql

SELECT user_id,
       AVG(hours_diff)::INTEGER AS hours_between_orders
FROM   (SELECT user_id,
               order_id,
               time,
               EXTRACT(EPOCH FROM (time - LAG(time, 1) OVER (PARTITION BY user_id ORDER BY time))) / 3600 AS hours_diff
        FROM   user_actions
        WHERE  order_id NOT IN (SELECT order_id
                                FROM   user_actions
                                WHERE  action = 'cancel_order')) t
WHERE  hours_diff IS NOT NULL
GROUP BY user_id
HAVING COUNT(order_id) > 1
ORDER BY user_id
LIMIT 1000;
```
### Explanation:
This query calculates the average time in hours between orders for users who have made more than one non-cancelled order. It uses a subquery to determine the time difference (hours_diff) between consecutive orders for each user using the LAG function. The EXTRACT(EPOCH FROM ...) function computes the difference in seconds, which is then converted to hours by dividing by 3600.

The outer query filters out null time differences and groups the results by user_id. The HAVING COUNT(order_id) > 1 clause ensures that only users with more than one order are included. Finally, the average time between orders is calculated, rounded to the nearest integer, and sorted by user ID. The LIMIT 1000 clause restricts the output to the first 1000 records for clarity.



## Task 8
### Description:
First, create a new table from the orders table that shows the total number of orders by day, excluding cancelled orders (which can be identified using the user_actions table). Name the column with the number of orders orders_count.

Next, use this resulting table in a subquery and apply a window function with the aggregate function AVG to calculate the moving average of the number of orders. The moving average should consider the three previous days for each record. Ensure that the window frame is set correctly to obtain accurate calculations. Round the moving average values to two decimal places and name the column moving_avg. No sorting of the resulting table is required.

### SQL Query:
```sql

SELECT date,
       orders_count,
       ROUND(AVG(orders_count) OVER (ORDER BY date ROWS BETWEEN 3 PRECEDING AND CURRENT ROW), 2) AS moving_avg
FROM   (SELECT DATE(creation_time) AS date,
               COUNT(order_id) AS orders_count
        FROM   orders
        WHERE  order_id NOT IN (SELECT order_id
                                FROM   user_actions
                                WHERE  action = 'cancel_order')
        GROUP BY date) t;
```
### Explanation:
This query starts by creating a subquery that counts the number of orders for each day from the orders table, excluding cancelled orders identified in the user_actions table. The outer query then calculates the moving average of orders_count using the AVG function as a window function, specifically considering the three previous days including the current day for each record. The ROUND function is used to round the moving average to two decimal places. The final output includes the date, the number of orders for that day, and the calculated moving average, with no additional sorting applied to the results.





## Task 9
### Description:
Identify couriers who delivered more orders in September 2022 than the average number of orders delivered by all couriers during that month. First, calculate the total number of orders delivered by each courier in the courier_actions table for September 2022. Then, use a window function to determine the average number of orders delivered by all couriers during that month. Compare the number of orders delivered by each courier to this average, using a CASE statement to indicate whether a courier delivered more orders (1) or not (0).

Name the column for the comparison result is_above_avg, the column with the count of delivered orders delivered_orders, and the column with the average value avg_delivered_orders. Round the average value to two decimal places. Sort the results by courier ID in ascending order.

### SQL Query:
```sql

SELECT courier_id,
       delivered_orders,
       ROUND(AVG(delivered_orders) OVER (), 2) AS avg_delivered_orders,
       CASE WHEN delivered_orders > ROUND(AVG(delivered_orders) OVER (), 2) THEN 1
            ELSE 0 END AS is_above_avg
FROM   (SELECT courier_id,
               COUNT(order_id) AS delivered_orders
        FROM   courier_actions
        WHERE  action = 'deliver_order'
           AND DATE_PART('month', time) = 9
           AND DATE_PART('year', time) = 2022
        GROUP BY courier_id) t
ORDER BY courier_id;
```
### Explanation:
This query begins by creating a subquery that counts the total number of orders delivered by each courier in September 2022 from the courier_actions table. The outer query calculates the average number of delivered orders across all couriers using a window function, which allows for comparison with each courier's delivered orders. The CASE statement is then used to mark each courier as above or below the average by checking if their delivered order count exceeds the calculated average. The results are displayed with the courier ID, number of delivered orders, average delivered orders, and the comparison result. Finally, the output is sorted by courier ID in ascending order.


## Task 10
### Description:
Count the number of first and repeat orders for each date based on the data from the user_actions table. First, use window functions and the CASE statement to create a table where each order is marked as "Первый" (First) or "Повторный" (Repeat). An order is classified as the first for a user if it was made first in time; all subsequent orders will be labeled as repeat orders. Then, count the number of orders for each category on each date.

Name the column indicating the order type order_type, the column with the date date, and the column with the number of orders orders_count. Only consider non-cancelled orders in the calculations. Sort the results first by date in ascending order and then by the values in the order type column.

### SQL Query:
```sql

SELECT time::date AS date,
       order_type,
       COUNT(order_id) AS orders_count
FROM   (SELECT user_id,
               order_id,
               time,
               CASE WHEN time = MIN(time) OVER (PARTITION BY user_id) THEN 'Первый'
                    ELSE 'Повторный' END AS order_type
        FROM   user_actions
        WHERE  order_id NOT IN (SELECT order_id
                                FROM   user_actions
                                WHERE  action = 'cancel_order')) t
GROUP BY date, order_type
ORDER BY date, order_type;
```
### Explanation:
This query first creates a subquery that uses the MIN function as a window function to determine the earliest order time for each user. The CASE statement classifies each order as "Первый" if its time matches the earliest order time, otherwise it is classified as "Повторный". The outer query then aggregates the results by date and order type, counting the number of orders in each category. The final output is sorted by date and order type, providing a clear overview of order counts categorized by their type for each date, while ensuring that only non-cancelled orders are considered.






## Task 11
### Description:
Enhance the previous query to calculate the share of first and repeat orders for each day. Maintain the structure of the previously generated table while adding one new column for the calculated share of orders. Name this column orders_share. Round the values in this column to two decimal places. Include the count of orders in each category as computed in the previous step. Continue to consider only non-cancelled orders in the calculations. Sort the results first by date in ascending order and then by the values in the order type column.

### SQL Query:
```sql

SELECT date,
       order_type,
       orders_count,
       ROUND(orders_count::decimal / SUM(orders_count) OVER (PARTITION BY date), 2) AS orders_share
FROM   (SELECT time::date AS date,
               order_type,
               COUNT(order_id) AS orders_count
        FROM   (SELECT user_id,
                       order_id,
                       time,
                       CASE WHEN time = MIN(time) OVER (PARTITION BY user_id) THEN 'Первый'
                            ELSE 'Повторный' END AS order_type
                FROM   user_actions
                WHERE  order_id NOT IN (SELECT order_id
                                        FROM   user_actions
                                        WHERE  action = 'cancel_order')) t
        GROUP BY date, order_type) t
ORDER BY date, order_type;
```
### Explanation:
This query builds upon the previous task by adding a calculation for the share of orders for each category (first and repeat) per day. It retains the existing structure, including the date, order type, and count of orders. The ROUND function is used to round the calculated share of orders to two decimal places. The share is calculated by dividing the count of orders for each type by the total number of orders on that date, achieved using a window function (SUM(orders_count) OVER (PARTITION BY date)). The final results are sorted by date and order type, providing a comprehensive view of order distribution by category for each day, while still filtering out cancelled orders.





## Task 12
### Description:
Apply window functions to the products table to calculate the average price of all products in a new column named avg_price for each record. Additionally, calculate the average price of products excluding the most expensive one in another column named avg_price_filtered. Use the FILTER clause to exclude the maximum price when calculating avg_price_filtered. Round both average values to two decimal places. Output all information about the products, including the new columns. Sort the results first by the price of the products in descending order and then by product ID in ascending order.

### SQL Query:
```sql

SELECT product_id,
       name,
       price,
       ROUND(AVG(price) OVER (), 2) AS avg_price,
       ROUND(AVG(price) FILTER (WHERE price != (SELECT MAX(price) FROM products)) OVER (), 2) AS avg_price_filtered
FROM   products
ORDER BY price DESC, product_id;
```
### Explanation:
This query retrieves the product_id, name, and price from the products table. It uses the AVG function as a window function to calculate the average price of all products, which is included in the avg_price column. For the avg_price_filtered column, it calculates the average price while excluding the maximum price by using the FILTER clause, ensuring that the most expensive product is not included in this calculation. Both average values are rounded to two decimal places using the ROUND function. Finally, the results are sorted by product price in descending order and by product ID in ascending order, providing a clear view of the products along with their average prices.




## Task 13
### Description:
For each record in the user_actions table, use window functions and the FILTER clause to calculate how many orders each user has created and how many they have cancelled up to that point in time. This means computing two cumulative sums for each user at each action: the number of created orders and the number of cancelled orders. If a user creates an order, increment the count of created orders by 1; if they cancel an order, increment the count of cancelled orders by 1.

Name the columns for the cumulative sums created_orders and canceled_orders. Based on these two columns, calculate the cancel_rate, which represents the share of cancelled orders relative to the total number of created orders. Round this value to two decimal places and name the column cancel_rate.

The result should include all columns from the original table along with the new dynamic metrics. Sort the results by user_id, order_id, and time in ascending order. Use a LIMIT clause to display only the first 1000 rows of the resulting table.

### SQL Query:
```sql

SELECT user_id,
       order_id,
       action,
       time,
       created_orders,
       canceled_orders,
       ROUND(canceled_orders::DECIMAL / NULLIF(created_orders, 0), 2) AS cancel_rate
FROM   (SELECT user_id,
               order_id,
               action,
               time,
               COUNT(order_id) FILTER (WHERE action != 'cancel_order') OVER (PARTITION BY user_id ORDER BY time) AS created_orders,
               COUNT(order_id) FILTER (WHERE action = 'cancel_order') OVER (PARTITION BY user_id ORDER BY time) AS canceled_orders
        FROM   user_actions) t
ORDER BY user_id, order_id, time
LIMIT 1000;
```
### Explanation:
This query computes the cumulative counts of created and cancelled orders for each user in the user_actions table. It employs the COUNT function along with the FILTER clause to differentiate between created and cancelled orders. The OVER clause defines the window for the calculation, partitioning by user_id and ordering by time.

The ROUND function is used to calculate the cancellation rate, which is the ratio of cancelled orders to created orders. The NULLIF(created_orders, 0) function prevents division by zero by returning NULL if created_orders is zero, ensuring that no errors occur in the calculation.
The final output includes the original columns from user_actions along with the newly calculated columns and is sorted by user_id, order_id, and time. The LIMIT 1000 clause restricts the results to the first 1000 rows for clarity.




## Task 14
### Description:
Select the top 10% of couriers from the courier_actions table based on the total number of orders delivered. Output the courier IDs, the count of delivered orders, and the ranking of each courier based on the number of delivered orders. The courier with the highest number of deliveries should have a rank of 1, while the courier with the lowest number in this top 10% should have a rank equal to 10% of the total number of couriers in the courier_actions table, rounded to the nearest whole number.

Name the columns for the count of delivered orders orders_count and the courier's rank courier_rank. Sort the results by ascending courier rank.

### SQL Query:
```sql

WITH courier_count AS (
    SELECT COUNT(DISTINCT courier_id) AS total_couriers
    FROM   courier_actions
),
ranked_couriers AS (
    SELECT courier_id,
           COUNT(DISTINCT order_id) AS orders_count,
           ROW_NUMBER() OVER (ORDER BY COUNT(DISTINCT order_id) DESC, courier_id) AS courier_rank
    FROM   courier_actions
    WHERE  action = 'deliver_order'
    GROUP BY courier_id
)
SELECT courier_id,
       orders_count,
       courier_rank
FROM   ranked_couriers
WHERE  courier_rank <= ROUND((SELECT total_couriers * 0.1 FROM courier_count));
```
### Explanation:
CTE for Total Couriers: The WITH courier_count AS clause calculates the total number of distinct couriers in the courier_actions table, storing it as total_couriers.

CTE for Ranked Couriers: The second CTE (ranked_couriers) counts the distinct orders delivered by each courier and assigns a rank using the ROW_NUMBER() function, ordering by the count of orders in descending order and by courier_id to break ties.

Final Selection: The outer query selects the courier ID, the count of orders, and the courier's rank from the ranked_couriers CTE. It filters the results to include only those couriers whose rank is within the top 10% of the total number of couriers, calculated as ROUND((SELECT total_couriers * 0.1 FROM courier_count)).

Sorting: The results are sorted by ascending courier rank, providing a clear view of the top couriers based on the number of orders delivered.






## Task 15
### Description:
Use window functions to select couriers from the courier_actions table who have worked for 10 or more days. Additionally, calculate how many orders they have delivered during their entire time working. For this analysis, assume that no couriers have left the company, and consider the duration of employment to be the difference between the first action of the courier and the current time, defined as the time of their last recorded action.

Only count whole days that have passed since the courier's first shift. The current time is represented by the last recorded action in the courier_actions table. Output three columns: the courier ID, the number of days they have been employed (days_employed), and the total number of delivered orders (delivered_orders). Sort the results first by the number of days employed in descending order and then by courier ID in ascending order.

### SQL Query:
```sql

SELECT courier_id,
       days_employed,
       delivered_orders
FROM   (SELECT courier_id,
               delivered_orders,
               DATE_PART('days', MAX(max_time) OVER() - MIN(min_time))::INTEGER AS days_employed
        FROM   (SELECT courier_id,
                       COUNT(DISTINCT order_id) FILTER (WHERE action = 'deliver_order') AS delivered_orders,
                       MIN(time) AS min_time,
                       MAX(time) AS max_time
                FROM   courier_actions
                GROUP BY courier_id) t1) t2
WHERE  days_employed >= 10
ORDER BY days_employed DESC, courier_id;
```
### Explanation:
Inner Query: The inner query aggregates data for each courier by calculating:

delivered_orders: The total number of distinct orders delivered by filtering for the action 'deliver_order'.
min_time: The timestamp of the first action (when the courier started working).
max_time: The timestamp of the last action (the current time for this analysis).
Middle Query: The middle query calculates the total days employed using the MAX(max_time) OVER() function to reference the current time against the MIN(min_time) for each courier. The DATE_PART('days', ...) function computes the difference in days, converting it to an integer.

Final Selection: The outer query filters the results to include only those couriers who have been employed for 10 or more days. It outputs the courier_id, days_employed, and delivered_orders, sorted by days_employed in descending order and then by courier_id in ascending order.



## Task 16
### Description:
Based on the information in the orders and products tables, calculate the total cost of each order, the daily revenue of the service, and the percentage share of each order's cost in the daily revenue. Include the following columns in the result: order ID, creation time of the order, cost of the order, daily revenue for the day the order was placed, and the share of the order's cost in the daily revenue expressed as a percentage.

Round the percentage shares to three decimal places. Sort the results first by the date of the order in descending order (not the time), then by the percentage share of the order in the daily revenue in descending order, and finally by the order ID in ascending order. Exclude cancelled orders from the calculations.

### SQL Query:
```sql

SELECT order_id,
       creation_time,
       order_price,
       SUM(order_price) OVER (PARTITION BY DATE(creation_time)) AS daily_revenue,
       ROUND(100 * order_price::DECIMAL / NULLIF(SUM(order_price) OVER (PARTITION BY DATE(creation_time)), 0), 3) AS percentage_of_daily_revenue
FROM   (SELECT order_id,
               creation_time,
               SUM(price) AS order_price
        FROM   (SELECT order_id,
                       creation_time,
                       product_ids,
                       UNNEST(product_ids) AS product_id
                FROM   orders
                WHERE  order_id NOT IN (SELECT order_id
                                        FROM   user_actions
                                        WHERE  action = 'cancel_order')) t3
            LEFT JOIN products USING (product_id)
        GROUP BY order_id, creation_time) t
ORDER BY DATE(creation_time) DESC, percentage_of_daily_revenue DESC, order_id;
```
### Explanation:
Inner Query: The inner query retrieves the order_id, creation_time, and expands the product_ids using UNNEST. It calculates the total price for each order by summing the prices of the products associated with each order while excluding cancelled orders.

Middle Query: The middle query groups the results by order_id and creation_time to get the total order price for each order.

Outer Query:

It calculates the daily revenue for the day of each order using SUM(order_price) OVER (PARTITION BY DATE(creation_time)).
It computes the percentage share of each order's price in the daily revenue, using NULLIF to prevent division by zero in case there are no orders on a given day.
Sorting: Finally, the results are sorted by the date of creation in descending order, followed by the percentage share of the order in daily revenue in descending order, and then by order ID in ascending order.




## Task 17
### Description:
Based on the information from the orders and products tables, calculate the daily revenue of the service and reflect it in a column named daily_revenue. Then, use window functions and offset functions to compute the daily revenue growth, both in absolute values and as a percentage relative to the previous day. Name the columns for the absolute growth revenue_growth_abs and for the percentage growth revenue_growth_percentage.

For the very first day in the dataset, set the growth to 0 in both columns. Exclude cancelled orders from the calculations. Sort the results by date in ascending order. Round the metrics daily_revenue, revenue_growth_abs, and revenue_growth_percentage to one decimal place using the ROUND() function.

### SQL Query:
```sql

SELECT date,
       ROUND(daily_revenue, 1) AS daily_revenue,
       ROUND(COALESCE(daily_revenue - LAG(daily_revenue, 1) OVER (ORDER BY date), 0), 1) AS revenue_growth_abs,
       ROUND(COALESCE(ROUND((daily_revenue - LAG(daily_revenue, 1) OVER (ORDER BY date))::DECIMAL / NULLIF(LAG(daily_revenue, 1) OVER (ORDER BY date), 0) * 100, 2), 0), 1) AS revenue_growth_percentage
FROM   (SELECT DATE(creation_time) AS date,
               SUM(price) AS daily_revenue
        FROM   (SELECT order_id,
                       creation_time,
                       product_ids,
                       UNNEST(product_ids) AS product_id
                FROM   orders
                WHERE  order_id NOT IN (SELECT order_id
                                        FROM   user_actions
                                        WHERE  action = 'cancel_order')) t1
            LEFT JOIN products USING (product_id)
        GROUP BY date) t2
ORDER BY date;
```
### Explanation:
Inner Query: The inner query retrieves the order_id, creation_time, and expands the product_ids using UNNEST. It sums the prices of products for each order while excluding cancelled orders identified in the user_actions table. The results are grouped by date to obtain the total daily revenue.

Outer Query:

The ROUND(daily_revenue, 1) function is used to round the daily revenue to one decimal place.
The LAG function calculates the revenue of the previous day, and the absolute revenue growth is computed as the difference between today's revenue and the previous day's revenue. The COALESCE function ensures that if there is no previous day's revenue (for the first day), it defaults to 0.
The percentage growth is calculated by dividing the absolute growth by the previous day's revenue and multiplying by 100. The NULLIF function is used to prevent division by zero when there is no previous day's revenue. The result is also rounded to one decimal place.
Sorting: The results are sorted by date in ascending order, providing a clear overview of daily revenue and growth metrics over time.


## Task 18
### Description:
Calculate the median price of all orders from the orders table made through the service, excluding cancelled orders. The result should output a single number representing the median price, and the column should be named median_price.

### SQL Query:
```sql

WITH main_table AS (
    SELECT order_price,
           ROW_NUMBER() OVER (ORDER BY order_price) AS row_number,
           COUNT(*) OVER () AS total_rows
    FROM   (SELECT SUM(price) AS order_price
            FROM   (SELECT order_id,
                           product_ids,
                           UNNEST(product_ids) AS product_id
                    FROM   orders
                    WHERE  order_id NOT IN (SELECT order_id
                                            FROM   user_actions
                                            WHERE  action = 'cancel_order')
                   ) t3
            LEFT JOIN products USING (product_id)
            GROUP BY order_id) t1
)
SELECT AVG(order_price) AS median_price
FROM   main_table
WHERE  row_number BETWEEN total_rows / 2.0 AND total_rows / 2.0 + 1;
```
### Explanation:
Inner Query: The innermost query retrieves each order's order_id, product_ids, and expands the product_ids using UNNEST to associate each order with its respective product prices. It filters out cancelled orders by excluding any order_id present in the user_actions table with the action 'cancel_order'.

Middle Query: The middle query calculates the total price for each order by summing the product prices associated with each order_id, grouping by order_id to ensure each order's total price is correctly aggregated.

Main CTE (main_table):

It assigns a row number to each order price using ROW_NUMBER(), ordered by order_price.
It also counts the total number of rows (orders) using COUNT(*) OVER ().
Final Selection: The outer query calculates the average of the order_price for the rows that fall within the median range. The median is computed by considering the middle value(s) depending on whether the total number of rows is odd or even:

If the total number of rows is odd, the median is the middle number.
If the total is even, the median is the average of the two middle numbers. This is achieved by selecting rows where row_number falls between total_rows / 2.0 and total_rows / 2.0 + 1.












