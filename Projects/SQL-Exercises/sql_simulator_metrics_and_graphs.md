## Metrics and Graphs

## SQL Task Links

|               |                 |               |             |                |
|---------------|-----------------|---------------|--------------|---------------|
| [Task 1](#task-1)  | [Task 2](#task-2)     | [Task 3](#task-3)     | [Task 4](#task-4)     | [Task 5](#task-5)     |
| [Task 6](#task-6)     | [Task 7](#task-7)     | [Task 8](#task-8)     | 


## Task 1
### Description:  
For each day represented in the `user_actions` and `courier_actions` tables, calculate the following metrics:
- Number of new users.
- Number of new couriers.
- Total number of users as of the current day.
- Total number of couriers as of the current day.

### SQL Query
```sql
SELECT start_date AS date,
       new_users,
       new_couriers,
       (SUM(new_users) OVER (ORDER BY start_date))::int AS total_users,
       (SUM(new_couriers) OVER (ORDER BY start_date))::int AS total_couriers
FROM   (SELECT start_date,
               COUNT(courier_id) AS new_couriers
        FROM   (SELECT courier_id,
                       MIN(time::date) AS start_date
                FROM   courier_actions
                GROUP BY courier_id) t1
        GROUP BY start_date) t2
    LEFT JOIN (SELECT start_date,
                      COUNT(user_id) AS new_users
               FROM   (SELECT user_id,
                              MIN(time::date) AS start_date
                       FROM   user_actions
                       GROUP BY user_id) t3
               GROUP BY start_date) t4 USING (start_date)
ORDER BY date;
```
### Explanation
The outer query retrieves the start_date and calculates the metrics for new users and new couriers.  
For new couriers:  
  The inner subquery selects courier_id and determines the minimum date (first appearance) of that courier in the courier_actions table.  
  The result is grouped by start_date to count the number of new couriers for each day.  
For new users:  
  A similar inner subquery is used to gather user_id and find their first action date in the user_actions table.
  This result is also grouped by start_date to count new users.  
The two results are combined using a LEFT JOIN on the start_date, ensuring all days are represented even if there are no new users or couriers.  
The SUM(...) OVER (ORDER BY start_date) function calculates cumulative totals of new users and new couriers, providing running totals.  
The ::int casts the cumulative sums to integer type, ensuring the output metrics are expressed as whole numbers.  
Finally, the results are sorted by date in ascending order.  




## Task 2
### Description  
Extend the previous query to calculate additional metrics for each day represented in the `user_actions` and `courier_actions` tables:
- Growth in the number of new users.
- Growth in the number of new couriers.
- Growth in the total number of users.
- Growth in the total number of couriers.

All growth metrics should be calculated as a percentage relative to the values from the previous day, rounded to two decimal places. The resulting table should be sorted by date in ascending order. The metrics will show how the counts change compared to the previous day, expressed as percentages.  

### SQL Query
```sql
SELECT date,
       new_users,
       new_couriers,
       total_users,
       total_couriers,
       ROUND(100 * (new_users - LAG(new_users, 1) OVER (ORDER BY date)) / LAG(new_users, 1) OVER (ORDER BY date)::decimal, 2) AS new_users_change,
       ROUND(100 * (new_couriers - LAG(new_couriers, 1) OVER (ORDER BY date)) / LAG(new_couriers, 1) OVER (ORDER BY date)::decimal, 2) AS new_couriers_change,
       ROUND(100 * total_users::decimal / LAG(total_users, 1) OVER (ORDER BY date), 2) AS total_users_growth,
       ROUND(100 * total_couriers::decimal / LAG(total_couriers, 1) OVER (ORDER BY date), 2) AS total_couriers_growth
FROM   (SELECT start_date AS date,
               new_users,
               new_couriers,
               (SUM(new_users) OVER (ORDER BY start_date))::int AS total_users,
               (SUM(new_couriers) OVER (ORDER BY start_date))::int AS total_couriers
        FROM   (SELECT start_date,
                       COUNT(courier_id) AS new_couriers
                FROM   (SELECT courier_id,
                               MIN(time::date) AS start_date
                        FROM   courier_actions
                        GROUP BY courier_id) t1
                GROUP BY start_date) t2
            LEFT JOIN (SELECT start_date,
                              COUNT(user_id) AS new_users
                       FROM   (SELECT user_id,
                                      MIN(time::date) AS start_date
                               FROM   user_actions
                               GROUP BY user_id) t3
                       GROUP BY start_date) t4 USING (start_date)) t5
ORDER BY date;
```



### Explanation:  
The outer query retrieves the date and previously calculated metrics for new users and couriers, along with total counts.  
The LAG function is used to access the values of new_users and new_couriers from the previous day to calculate changes.  
Growth percentages for new users and couriers are computed by subtracting the previous day's values from the current day's values, dividing by the previous day's value, and multiplying by 100.  
Growth percentages for total users and total couriers are calculated by comparing the current total to the previous day's total, also expressed as a percentage.  
The ROUND function is used to round the results to two decimal places.  
The final output is sorted by date in ascending order to provide a clear chronological view of the metrics.  





## Task 3
### Description:  
For each day represented in the `user_actions` and `courier_actions` tables, calculate the following metrics:
- Number of paying users.
- Number of active couriers.
- Share of paying users in the total number of users as of the current day.
- Share of active couriers in the total number of couriers as of the current day.

### SQL Query
```sql
SELECT date,
       paying_users,
       active_couriers,
       ROUND(100 * paying_users::decimal / total_users, 2) AS paying_users_share,
       ROUND(100 * active_couriers::decimal / total_couriers, 2) AS active_couriers_share
FROM   (SELECT start_date AS date,
               new_users,
               new_couriers,
               (SUM(new_users) OVER (ORDER BY start_date))::int AS total_users,
               (SUM(new_couriers) OVER (ORDER BY start_date))::int AS total_couriers
        FROM   (SELECT start_date,
                       COUNT(courier_id) AS new_couriers
                FROM   (SELECT courier_id,
                               MIN(time::date) AS start_date
                        FROM   courier_actions
                        GROUP BY courier_id) t1
                GROUP BY start_date) t2
            LEFT JOIN (SELECT start_date,
                              COUNT(user_id) AS new_users
                       FROM   (SELECT user_id,
                                      MIN(time::date) AS start_date
                               FROM   user_actions
                               GROUP BY user_id) t3
                       GROUP BY start_date) t4 USING (start_date)) t5
    LEFT JOIN (SELECT time::date AS date,
                      COUNT(DISTINCT courier_id) AS active_couriers
               FROM   courier_actions
               WHERE  order_id NOT IN (SELECT order_id
                                       FROM   user_actions
                                       WHERE  action = 'cancel_order')
               GROUP BY date) t6 USING (date)
    LEFT JOIN (SELECT time::date AS date,
                      COUNT(DISTINCT user_id) AS paying_users
               FROM   user_actions
               WHERE  order_id NOT IN (SELECT order_id
                                       FROM   user_actions
                                       WHERE  action = 'cancel_order')
               GROUP BY date) t7 USING (date);
```

### Explanation:  
The outer query retrieves the date, number of paying users, and number of active couriers, along with their respective shares.  
The first subquery calculates the total number of new users and new couriers for each day, while maintaining cumulative totals using the SUM(...) OVER (ORDER BY start_date) function.  
The second subquery (aliased as t6) counts distinct courier_id values as active couriers for each day, filtering out orders that were canceled.  
The third subquery (aliased as t7) counts distinct user_id values as paying users for each day, also excluding canceled orders.  
The shares of paying users and active couriers are calculated as percentages of the total users and total couriers, respectively, and are rounded to two decimal places using the ROUND function.  
Finally, the results are presented for each date, allowing for an analysis of user engagement over time.  






## Task 4
### Description:  
For each day represented in the `user_actions` table, calculate the following metrics:
- The share of users who made only one order that day out of the total number of paying users.
- The share of users who made multiple orders that day out of the total number of paying users.

### SQL Query
```sql
SELECT date,
       ROUND(100 * single_order_users::decimal / paying_users, 2) AS single_order_users_share,
       100 - ROUND(100 * single_order_users::decimal / paying_users, 2) AS several_orders_users_share
FROM   (SELECT time::date AS date,
               COUNT(DISTINCT user_id) AS paying_users
        FROM   user_actions
        WHERE  order_id NOT IN (SELECT order_id
                                FROM   user_actions
                                WHERE  action = 'cancel_order')
        GROUP BY date) t1
    LEFT JOIN (SELECT date,
                      COUNT(user_id) AS single_order_users
               FROM   (SELECT time::date AS date,
                              user_id,
                              COUNT(DISTINCT order_id) AS user_orders
                       FROM   user_actions
                       WHERE  order_id NOT IN (SELECT order_id
                                               FROM   user_actions
                                               WHERE  action = 'cancel_order')
                       GROUP BY date, user_id 
                       HAVING COUNT(DISTINCT order_id) = 1) t2
               GROUP BY date) t3 USING (date)
ORDER BY date;
```
### Explanation:
The outer query retrieves the date, calculates the share of users who made a single order, and the share of users who made multiple orders.  
The first subquery (aliased as t1) counts the total number of distinct paying users for each date, filtering out canceled orders.  
The second subquery (aliased as t3) counts distinct user_id values who made only one order on each date, also filtering out canceled orders. This is achieved by grouping by date and user_id, and using the HAVING clause to restrict the count of distinct orders to one.  
The shares of single-order and several-order users are calculated as percentages of the total paying users, with the ROUND function ensuring the results are rounded to two decimal places.  
The final results are sorted by date in ascending order, allowing for a clear view of user behavior over time.  



## Task 5
### Description:  
For each day represented in the `user_actions` table, calculate the following metrics:
- Total number of orders.
- Number of first orders (orders made by users for the first time).
- Number of orders made by new users (orders made by users on the same day they first used the service).
- Share of first orders in the total number of orders (share of metric 2 in metric 1).
- Share of new user orders in the total number of orders (share of metric 3 in metric 1).

### SQL Query
```sql
SELECT date,
       orders,
       first_orders,
       new_users_orders::int,
       ROUND(100 * first_orders::decimal / orders, 2) AS first_orders_share,
       ROUND(100 * new_users_orders::decimal / orders, 2) AS new_users_orders_share
FROM   (SELECT creation_time::date AS date,
               COUNT(DISTINCT order_id) AS orders
        FROM   orders
        WHERE  order_id NOT IN (SELECT order_id
                                FROM   user_actions
                                WHERE  action = 'cancel_order')
           AND order_id IN (SELECT order_id
                            FROM   courier_actions
                            WHERE  action = 'deliver_order')
        GROUP BY date) t5
    LEFT JOIN (SELECT first_order_date AS date,
                      COUNT(user_id) AS first_orders
               FROM   (SELECT user_id,
                              MIN(time::date) AS first_order_date
                       FROM   user_actions
                       WHERE  order_id NOT IN (SELECT order_id
                                               FROM   user_actions
                                               WHERE  action = 'cancel_order')
                       GROUP BY user_id) t4
               GROUP BY first_order_date) t7 USING (date)
    LEFT JOIN (SELECT start_date AS date,
                      SUM(orders) AS new_users_orders
               FROM   (SELECT t1.user_id,
                              t1.start_date,
                              COALESCE(t2.orders, 0) AS orders
                       FROM   (SELECT user_id,
                                      MIN(time::date) AS start_date
                               FROM   user_actions
                               GROUP BY user_id) t1
                           LEFT JOIN (SELECT user_id,
                                             time::date AS date,
                                             COUNT(DISTINCT order_id) AS orders
                                      FROM   user_actions
                                      WHERE  order_id NOT IN (SELECT order_id
                                                              FROM   user_actions
                                                              WHERE  action = 'cancel_order')
                                      GROUP BY user_id, date) t2
                               ON t1.user_id = t2.user_id AND
                                  t1.start_date = t2.date) t3
               GROUP BY start_date) t6 USING (date)
ORDER BY date;
```

### Explanation:
The outer query retrieves the date, total orders, first orders, and orders made by new users, along with their respective shares.  
The first subquery (aliased as t5) counts the total distinct orders for each date, filtering out canceled orders and only including delivered orders.  
The second subquery (aliased as t7) identifies the first orders made by users, grouping by user ID to find the earliest order date for each user.  
The third subquery (aliased as t6) counts the number of orders made by new users on their first day, using a left join to associate the start date with the corresponding orders.  
The shares of first orders and new user orders are calculated as percentages of the total number of orders, rounded to two decimal places using the ROUND function.  
Finally, the results are sorted by date in ascending order, providing a clear chronological view of order metrics.



## Task 6
### Description:  
Based on the data in the `user_actions`, `courier_actions`, and `orders` tables, calculate the following metrics for each day:
- Number of paying users per active courier.
- Number of orders per active courier.

### SQL Query
```sql
SELECT date,
       ROUND(paying_users::decimal / couriers, 2) AS users_per_courier,
       ROUND(orders::decimal / couriers, 2) AS orders_per_courier
FROM   (SELECT time::date AS date,
               COUNT(DISTINCT courier_id) AS couriers
        FROM   courier_actions
        WHERE  order_id NOT IN (SELECT order_id
                                FROM   user_actions
                                WHERE  action = 'cancel_order')
        GROUP BY date) t1 
JOIN   (SELECT creation_time::date AS date,
               COUNT(DISTINCT order_id) AS orders
        FROM   orders
        WHERE  order_id NOT IN (SELECT order_id
                                FROM   user_actions
                                WHERE  action = 'cancel_order')
        GROUP BY date) t2 USING (date) 
JOIN   (SELECT time::date AS date,
               COUNT(DISTINCT user_id) AS paying_users
        FROM   user_actions
        WHERE  order_id NOT IN (SELECT order_id
                                FROM   user_actions
                                WHERE  action = 'cancel_order')
        GROUP BY date) t3 USING (date)
ORDER BY date;
```
### Explanation
The outer query retrieves the date, calculates the number of paying users per active courier, and the number of orders per active courier.  
The first subquery (aliased as t1) counts the distinct active couriers for each date, excluding any orders that have been canceled.  
The second subquery (aliased as t2) counts the total distinct orders for each date, similarly filtering out canceled orders.  
The third subquery (aliased as t3) counts the distinct paying users for each date, also filtering out canceled orders.  
The metrics for paying users per courier and orders per courier are calculated by dividing the respective counts by the number of couriers, rounded to two decimal places using the ROUND function.  
Finally, the results are sorted by date in ascending order, providing a clear view of these metrics over time.  




## Task 7
### Description:  
Based on the data in the `courier_actions` table, calculate the average delivery time in minutes for couriers for each day.

### SQL Query
```sql
SELECT date,
       ROUND(AVG(delivery_time))::int AS minutes_to_deliver
FROM   (SELECT order_id,
               MAX(time::date) AS date,
               EXTRACT(epoch FROM MAX(time) - MIN(time)) / 60 AS delivery_time
        FROM   courier_actions
        WHERE  order_id NOT IN (SELECT order_id
                                FROM   user_actions
                                WHERE  action = 'cancel_order')
        GROUP BY order_id) t
GROUP BY date
ORDER BY date;
```
### Explanation
The outer query calculates the average delivery time in minutes for each day by aggregating the delivery times.  
The inner subquery (aliased as t) calculates the delivery time for each order_id by finding the maximum and minimum timestamps in the courier_actions table. The difference between these two timestamps is converted from seconds to minutes using the EXTRACT(epoch FROM ...) function divided by 60.  
The results from the inner query include the order ID, the corresponding date, and the calculated delivery time.  
The outer query computes the average delivery time across all orders for each date and rounds the result to the nearest integer using the ROUND function.  
Finally, the results are sorted by date in ascending order, providing a clear view of delivery times over time.  





## Task 8
### Description:  
Based on the data in the `orders` table, calculate the following metrics for each hour of the day:
- Number of successful (delivered) orders.
- Number of canceled orders.
- Share of canceled orders in the total number of orders (cancel rate).

### SQL Query
```sql
SELECT hour,
       successful_orders,
       canceled_orders,
       ROUND(canceled_orders::decimal / (successful_orders + canceled_orders), 3) AS cancel_rate
FROM   (SELECT DATE_PART('hour', creation_time)::int AS hour,
               COUNT(order_id) AS successful_orders
        FROM   orders
        WHERE  order_id NOT IN (SELECT order_id
                                FROM   user_actions
                                WHERE  action = 'cancel_order')
        GROUP BY hour) t1
    LEFT JOIN (SELECT DATE_PART('hour', creation_time)::int AS hour,
                      COUNT(order_id) AS canceled_orders
               FROM   orders
               WHERE  order_id IN (SELECT order_id
                                   FROM   user_actions
                                   WHERE  action = 'cancel_order')
               GROUP BY hour) t2 USING (hour)
ORDER BY hour;
```
### Explanation:
The outer query retrieves the hour of the day along with the counts of successful and canceled orders, as well as the cancel rate.  
The first subquery (aliased as t1) counts the total successful orders for each hour, filtering out any orders that have been canceled based on the user_actions table.  
The second subquery (aliased as t2) counts the canceled orders for each hour, specifically those that match the canceled actions recorded in the user_actions table.  
The cancel rate is calculated by dividing the number of canceled orders by the total number of orders (successful + canceled) and rounding the result to three decimal places using the ROUND function.  
The results are then combined using a LEFT JOIN on the hour and sorted by hour in ascending order, providing a clear view of order metrics throughout the day.  




























