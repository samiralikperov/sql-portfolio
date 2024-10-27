## Product Economics

## SQL Task Links

|               |                 |               |             |                |
|-----------------------|-----------------------|-----------------------|-----------------------|-----------------------|
| [Task 1](#task-1)     | [Task 2](#task-2)     | [Task 3](#task-3)     | [Task 4](#task-4)     | [Task 5](#task-5)     |
| [Task 6](#task-6)     | [Task 7](#task-7)     | 

## Task 1

### Description

Calculate the following metrics for each day in the `orders` table:

- Daily revenue for the day.
- Cumulative revenue up to the current day.
- Revenue growth for the day compared to the previous day’s revenue.

This query calculates key daily metrics to track revenue trends over time. Specifically:

1. **Daily Revenue (`revenue`)** — This column shows the revenue generated for each specific day. We calculate it by summing up the prices of all sold products for the day, excluding canceled orders. This gives a clear view of the revenue from completed transactions.

2. **Cumulative Revenue (`total_revenue`)** — This column shows the cumulative revenue up to the current day. Using a rolling sum function with `SUM` and `OVER (ORDER BY date)`, we accumulate daily revenues, helping to visualize total revenue growth over time.

3. **Revenue Growth Compared to Previous Day (`revenue_change`)** — This column displays the revenue growth rate relative to the previous day’s revenue. Using the `LAG` function, we compare each day’s revenue to the previous day’s. The result is expressed in percentage terms and rounded to two decimal places, allowing for a quick assessment of day-to-day changes.

### SQL Query

```sql
SELECT 
    date,
    revenue,
    SUM(revenue) OVER (ORDER BY date) AS total_revenue,
    ROUND(
        100 * (revenue - LAG(revenue, 1) OVER (ORDER BY date))::decimal / LAG(revenue, 1) OVER (ORDER BY date),
        2
    ) AS revenue_change
FROM (
    SELECT 
        creation_time::date AS date,
        SUM(price) AS revenue
    FROM (
        SELECT 
            creation_time,
            UNNEST(product_ids) AS product_id
        FROM orders
        WHERE order_id NOT IN (
            SELECT order_id
            FROM user_actions
            WHERE action = 'cancel_order'
        )
    ) t1
    LEFT JOIN products USING (product_id)
    GROUP BY date
) t2;
```
| Date       | Revenue    | Total Revenue  | Revenue Change (%) |
|------------|------------|----------------|---------------------|
| 08/24/22   | 49,924.0   | 49,924.0       |                     |
| 08/25/22   | 430,860.0  | 480,784.0      | 763.03             |
| 08/26/22   | 534,766.0  | 1,015,550.0    | 24.12              |
| 08/27/22   | 817,053.0  | 1,832,603.0    | 52.79              |
| 08/28/22   | 1,133,370.0| 2,965,973.0    | 38.71              |
| 08/29/22   | 1,279,891.0| 4,245,864.0    | 12.93              |
| 08/30/22   | 1,279,377.0| 5,525,241.0    | -0.04              |
| 08/31/22   | 1,312,720.0| 6,837,961.0    | 2.61               |
| 09/01/22   | 1,406,101.0| 8,244,062.0    | 7.11               |
| 09/02/22   | 1,907,107.0| 10,151,169.0   | 35.63              |
| 09/03/22   | 2,210,988.0| 12,362,157.0   | 15.93              |
| 09/04/22   | 2,294,009.0| 14,656,166.0   | 3.75               |
| 09/05/22   | 1,784,690.0| 16,440,856.0   | -22.2              |
| 09/06/22   | 1,330,931.0| 17,771,787.0   | -25.43             |
| 09/07/22   | 1,807,800.0| 19,579,587.0   | 35.83              |
| 09/08/22   | 2,099,508.0| 21,679,095.0   | 16.14              |


### Explanation:
This query efficiently calculates daily, cumulative, and percentage growth metrics:  
The inner subquery (t1) extracts individual product_ids from each order by unnesting the product_ids array. It filters out canceled orders using a subquery on user_actions.  
Revenue Calculation (t2):   
Aggregates the daily revenue by grouping on date (converted from creation_time), summing prices from the products table.
Final Calculations:  
total_revenue: Cumulative revenue sum up to the current day.  
revenue_change: Daily revenue growth percentage compared to the previous day. This uses the LAG function for a one-day offset and rounds to two decimal places.





## Task 2

### Description

Calculate the following average revenue metrics for each day using the `orders` and `user_actions` tables:

1. **ARPU (Average Revenue Per User)** — the average revenue per user for the day.
2. **ARPPU (Average Revenue Per Paying User)** — the average revenue per paying user for the day.
3. **AOV (Average Order Value)** — the average revenue per order for the day.

This query calculates key revenue metrics to help analyze consumer spending behavior for each day:

1. **ARPU**: Average revenue per user for the day, showing how much revenue, on average, each unique user generated.
2. **ARPPU**: Average revenue per paying user for the day, which considers only those users who made purchases.
3. **AOV**: Average order value for the day, showing the average revenue per order made, helping assess the typical order size.



### SQL Query

```sql
SELECT 
    date,
    ROUND(revenue::decimal / users, 2) AS arpu,
    ROUND(revenue::decimal / paying_users, 2) AS arppu,
    ROUND(revenue::decimal / orders, 2) AS aov
FROM (
    SELECT 
        creation_time::date AS date,
        COUNT(DISTINCT order_id) AS orders,
        SUM(price) AS revenue
    FROM (
        SELECT 
            order_id,
            creation_time,
            UNNEST(product_ids) AS product_id
        FROM orders
        WHERE order_id NOT IN (
            SELECT order_id
            FROM user_actions
            WHERE action = 'cancel_order'
        )
    ) t1
    LEFT JOIN products USING (product_id)
    GROUP BY date
) t2
LEFT JOIN (
    SELECT 
        time::date AS date,
        COUNT(DISTINCT user_id) AS users
    FROM user_actions
    GROUP BY date
) t3 USING (date)
LEFT JOIN (
    SELECT 
        time::date AS date,
        COUNT(DISTINCT user_id) AS paying_users
    FROM user_actions
    WHERE order_id NOT IN (
        SELECT order_id
        FROM user_actions
        WHERE action = 'cancel_order'
    )
    GROUP BY date
) t4 USING (date)
ORDER BY date;
```

| Date       | ARPU   | ARPPU  | AOV    |
|------------|--------|--------|--------|
| 08/24/22   | 372.57 | 393.10 | 361.77 |
| 08/25/22   | 508.09 | 525.44 | 406.86 |
| 08/26/22   | 452.04 | 470.33 | 369.57 |
| 08/27/22   | 509.38 | 527.81 | 381.62 |
| 08/28/22   | 528.38 | 544.10 | 378.04 |
| 08/29/22   | 559.15 | 581.24 | 391.76 |
| 08/30/22   | 546.74 | 567.85 | 379.52 |
| 08/31/22   | 517.63 | 540.21 | 384.96 |
| 09/01/22   | 499.33 | 518.86 | 381.26 |
| 09/02/22   | 537.67 | 556.17 | 381.35 |
| 09/03/22   | 565.90 | 582.76 | 387.28 |
| 09/04/22   | 541.55 | 558.97 | 381.70 |
| 09/05/22   | 512.84 | 530.84 | 381.75 |
| 09/06/22   | 475.33 | 492.75 | 385.67 |
| 09/07/22   | 496.65 | 514.02 | 378.44 |
| 09/08/22   | 521.10 | 536.68 | 383.54 |


### Explanation:
This query calculates average revenue metrics by joining and aggregating data:

Orders and Revenue Calculation:  
We calculate the daily total number of orders and the total revenue (revenue) from non-canceled orders. The inner query (t1) removes canceled orders and joins with the products table to sum up price values by date.  
User Counts:  
users: Count of unique users who interacted with the service each day, regardless of whether they made a purchase.  
paying_users: Count of unique users who completed at least one non-canceled order each day.  
Final Metrics:  
arpu: Average revenue per user (revenue divided by users).  
arppu: Average revenue per paying user (revenue divided by paying_users).  
aov: Average order value (revenue divided by orders).  







## Task 3

### Description

For each day, calculate the following cumulative metrics from the `orders` and `user_actions` tables:

1. **Running ARPU** — cumulative revenue per user up to the current day.
2. **Running ARPPU** — cumulative revenue per paying user up to the current day.
3. **Running AOV** — cumulative average order value up to the current day.

These metrics will allow us to see how the averages for revenue per user, revenue per paying user, and order value evolve over time as more data accumulates.

In this query, we calculate dynamic, cumulative metrics that provide insight into the trends of average user spending behavior. These metrics are:

1. **Running ARPU**: Cumulative average revenue per user up to each day, showing how the overall average revenue per user changes as more data accumulates.
2. **Running ARPPU**: Cumulative average revenue per paying user up to each day, which helps to see spending trends among paying users over time.
3. **Running AOV**: Cumulative average order value, representing how the average value per order evolves.

### SQL Query

```sql
SELECT date,
       round(sum(revenue) OVER (ORDER BY date)::decimal / sum(new_users) OVER (ORDER BY date),
             2) as running_arpu,
       round(sum(revenue) OVER (ORDER BY date)::decimal / sum(new_paying_users) OVER (ORDER BY date),
             2) as running_arppu,
       round(sum(revenue) OVER (ORDER BY date)::decimal / sum(orders) OVER (ORDER BY date),
             2) as running_aov
FROM   (SELECT creation_time::date as date,
               count(distinct order_id) as orders,
               sum(price) as revenue
        FROM   (SELECT order_id,
                       creation_time,
                       unnest(product_ids) as product_id
                FROM   orders
                WHERE  order_id not in (SELECT order_id
                                        FROM   user_actions
                                        WHERE  action = 'cancel_order')) t1
            LEFT JOIN products using(product_id)
        GROUP BY date) t2
    LEFT JOIN (SELECT time::date as date,
                      count(distinct user_id) as users
               FROM   user_actions
               GROUP BY date) t3 using (date)
    LEFT JOIN (SELECT time::date as date,
                      count(distinct user_id) as paying_users
               FROM   user_actions
               WHERE  order_id not in (SELECT order_id
                                       FROM   user_actions
                                       WHERE  action = 'cancel_order')
               GROUP BY date) t4 using (date)
    LEFT JOIN (SELECT date,
                      count(user_id) as new_users
               FROM   (SELECT user_id,
                              min(time::date) as date
                       FROM   user_actions
                       GROUP BY user_id) t5
               GROUP BY date) t6 using (date)
    LEFT JOIN (SELECT date,
                      count(user_id) as new_paying_users
               FROM   (SELECT user_id,
                              min(time::date) as date
                       FROM   user_actions
                       WHERE  order_id not in (SELECT order_id
                                               FROM   user_actions
                                               WHERE  action = 'cancel_order')
                       GROUP BY user_id) t7
               GROUP BY date) t8 using (date);
```

| Date       | Running ARPU | Running ARPPU | Running AOV |
|------------|--------------|---------------|-------------|
| 08/24/22   | 372.57       | 393.10        | 361.77      |
| 08/25/22   | 499.26       | 517.53        | 401.66      |
| 08/26/22   | 512.90       | 530.87        | 384.10      |
| 08/27/22   | 571.80       | 590.21        | 382.99      |
| 08/28/22   | 632.13       | 649.72        | 381.08      |
| 08/29/22   | 707.53       | 726.29        | 384.24      |
| 08/30/22   | 766.86       | 786.40        | 383.14      |
| 08/31/22   | 792.81       | 813.46        | 383.49      |
| 09/01/22   | 813.18       | 832.90        | 383.11      |
| 09/02/22   | 844.17       | 863.05        | 382.77      |
| 09/03/22   | 886.24       | 904.39        | 383.57      |
| 09/04/22   | 921.71       | 938.78        | 383.28      |
| 09/05/22   | 950.45       | 967.17        | 383.11      |
| 09/06/22   | 970.18       | 986.72        | 383.30      |
| 09/07/22   | 992.38       | 1,007.85      | 382.85      |
| 09/08/22   | 1,012.99     | 1,028.03      | 382.91      |

### Explanation:
Orders and Revenue Calculation (t2):  

This subquery calculates the daily total number of orders (orders) and total revenue (revenue) from non-canceled orders.  
User Counts:  
Total Users (users): Count of unique users interacting with the service each day.  
Paying Users (paying_users): Count of unique users who made non-canceled purchases.  
New User Counts:  
new_users: Number of unique users appearing for the first time each day.  
new_paying_users: Number of unique users making their first non-canceled purchase each day.  
Running Metrics:  
running_arpu: Cumulative average revenue per user up to the current date (revenue / new_users).  
running_arppu: Cumulative average revenue per paying user up to the current date (revenue / new_paying_users).  
running_aov: Cumulative average order value up to the current date (revenue / orders).  





## Task 4

### Description

Calculate average revenue metrics for each day of the week (Monday through Sunday) in the `orders` and `user_actions` tables:

1. **ARPU (Average Revenue Per User)** — average revenue per user.
2. **ARPPU (Average Revenue Per Paying User)** — average revenue per paying user.
3. **AOV (Average Order Value)** — average revenue per order.

Include data only for the period between August 26, 2022, and September 8, 2022, to ensure two complete occurrences of each weekday. The result should include the weekday name (e.g., Monday) and the weekday number (1 for Monday to 7 for Sunday), with columns named `weekday` and `weekday_number`, respectively. Round each metric to two decimal places, and order the results by weekday number.

This query calculates the average revenue per user, paying user, and per order for each day of the week within a defined period. Metrics are calculated as follows:

1. **ARPU**: Average revenue per user, indicating the average spend per user on each day of the week.
2. **ARPPU**: Average revenue per paying user, showing average spend among paying users by day of the week.
3. **AOV**: Average order value, representing the typical revenue per order for each weekday.

### SQL Query

```sql
SELECT weekday,
       t1.weekday_number as weekday_number,
       round(revenue::decimal / users, 2) as arpu,
       round(revenue::decimal / paying_users, 2) as arppu,
       round(revenue::decimal / orders, 2) as aov
FROM   (SELECT to_char(creation_time, 'Day') as weekday,
               max(date_part('isodow', creation_time)) as weekday_number,
               count(distinct order_id) as orders,
               sum(price) as revenue
        FROM   (SELECT order_id,
                       creation_time,
                       unnest(product_ids) as product_id
                FROM   orders
                WHERE  order_id not in (SELECT order_id
                                        FROM   user_actions
                                        WHERE  action = 'cancel_order')
                   and creation_time >= '2022-08-26'
                   and creation_time < '2022-09-09') t4
            LEFT JOIN products using(product_id)
        GROUP BY weekday) t1
    LEFT JOIN (SELECT to_char(time, 'Day') as weekday,
                      max(date_part('isodow', time)) as weekday_number,
                      count(distinct user_id) as users
               FROM   user_actions
               WHERE  time >= '2022-08-26'
                  and time < '2022-09-09'
               GROUP BY weekday) t2 using (weekday)
    LEFT JOIN (SELECT to_char(time, 'Day') as weekday,
                      max(date_part('isodow', time)) as weekday_number,
                      count(distinct user_id) as paying_users
               FROM   user_actions
               WHERE  order_id not in (SELECT order_id
                                       FROM   user_actions
                                       WHERE  action = 'cancel_order')
                  and time >= '2022-08-26'
                  and time < '2022-09-09'
               GROUP BY weekday) t3 using (weekday)
ORDER BY weekday_number;
```

| Weekday   | Weekday Number | ARPU   | ARPPU  | AOV    |
|-----------|----------------|--------|--------|--------|
| Monday    | 1              | 555.98 | 575.18 | 385.87 |
| Tuesday   | 2              | 528.94 | 548.04 | 382.63 |
| Wednesday | 3              | 528.90 | 548.33 | 381.16 |
| Thursday  | 4              | 533.98 | 551.37 | 382.62 |
| Friday    | 5              | 534.79 | 553.21 | 378.70 |
| Saturday  | 6              | 578.53 | 595.48 | 385.74 |
| Sunday    | 7              | 566.23 | 583.38 | 380.48 |

### Explanation:
Orders and Revenue Calculation (t1):  
Calculates the daily total of orders and revenue for each day of the week within the specified period, excluding canceled orders. The weekday_number and weekday are extracted to identify the weekday.  
User Counts:  
Total Users (users): Counts unique users for each weekday.  
Paying Users (paying_users): Counts unique users making non-canceled purchases for each weekday.  
Final Metrics:  
arpu: Average revenue per user for each weekday (revenue / users).  
arppu: Average revenue per paying user for each weekday (revenue / paying_users).  
aov: Average order value for each weekday (revenue / orders).  





## Task 5

For each day, calculate the following revenue metrics from the `orders` and `user_actions` tables:

1. **Total Revenue (`revenue`)** — the revenue generated from all users (both new and returning) for the day.
2. **Revenue from New Users (`new_users_revenue`)** — the revenue generated from orders by new users for the day.
3. **New Users Revenue Share (`new_users_revenue_share`)** — the percentage share of revenue from new users relative to the total daily revenue.
4. **Old Users Revenue Share (`old_users_revenue_share`)** — the percentage share of revenue from returning users relative to the total daily revenue.

The columns should be named `revenue`, `new_users_revenue`, `new_users_revenue_share`, `old_users_revenue_share`, and `date`. Express the shares in percentages, rounded to two decimal places.

### Description

This query calculates the daily revenue and distinguishes the revenue generated by new users from the total. The final output provides insights into the contribution of new users to the daily revenue and the breakdown of revenue sources.

1. **Revenue**: Total revenue for each day, combining both new and returning users.
2. **New Users Revenue**: Revenue generated by users placing their first orders on a given day.
3. **New Users Revenue Share**: The percentage of daily revenue generated by new users.
4. **Old Users Revenue Share**: The percentage of daily revenue generated by returning users.

### SQL Query

```sql
SELECT date,
       revenue,
       new_users_revenue,
       round(new_users_revenue / revenue * 100, 2) as new_users_revenue_share,
       100 - round(new_users_revenue / revenue * 100, 2) as old_users_revenue_share
FROM   (SELECT creation_time::date as date,
               sum(price) as revenue
        FROM   (SELECT order_id,
                       creation_time,
                       unnest(product_ids) as product_id
                FROM   orders
                WHERE  order_id not in (SELECT order_id
                                        FROM   user_actions
                                        WHERE  action = 'cancel_order')) t3
            LEFT JOIN products using (product_id)
        GROUP BY date) t1
    LEFT JOIN (SELECT start_date as date,
                      sum(revenue) as new_users_revenue
               FROM   (SELECT t5.user_id,
                              t5.start_date,
                              coalesce(t6.revenue, 0) as revenue
                       FROM   (SELECT user_id,
                                      min(time::date) as start_date
                               FROM   user_actions
                               GROUP BY user_id) t5
                           LEFT JOIN (SELECT user_id,
                                             date,
                                             sum(order_price) as revenue
                                      FROM   (SELECT user_id,
                                                     time::date as date,
                                                     order_id
                                              FROM   user_actions
                                              WHERE  order_id not in (SELECT order_id
                                                                      FROM   user_actions
                                                                      WHERE  action = 'cancel_order')) t7
                                          LEFT JOIN (SELECT order_id,
                                                            sum(price) as order_price
                                                     FROM   (SELECT order_id,
                                                                    unnest(product_ids) as product_id
                                                             FROM   orders
                                                             WHERE  order_id not in (SELECT order_id
                                                                                     FROM   user_actions
                                                                                     WHERE  action = 'cancel_order')) t9
                                                         LEFT JOIN products using (product_id)
                                                     GROUP BY order_id) t8 using (order_id)
                                      GROUP BY user_id, date) t6
                               ON t5.user_id = t6.user_id and
                                  t5.start_date = t6.date) t4
               GROUP BY start_date) t2 using (date);
```

| Date       | Revenue   | New Users Revenue | New Users Revenue Share (%) | Old Users Revenue Share (%) |
|------------|-----------|-------------------|-----------------------------|-----------------------------|
| 08/24/22   | 49,924.0  | 49,924.0         | 100.0                       | 0.0                         |
| 08/25/22   | 430,860.0 | 417,333.0        | 96.86                       | 3.14                        |
| 08/26/22   | 534,766.0 | 463,326.0        | 86.64                       | 13.36                       |
| 08/27/22   | 817,053.0 | 619,318.0        | 75.8                        | 24.2                        |
| 08/28/22   | 1,133,370.0 | 801,162.0      | 70.69                       | 29.31                       |
| 08/29/22   | 1,279,891.0 | 717,374.0      | 56.05                       | 43.95                       |
| 08/30/22   | 1,279,377.0 | 656,429.0      | 51.31                       | 48.69                       |
| 08/31/22   | 1,312,720.0 | 720,381.0      | 54.88                       | 45.12                       |
| 09/01/22   | 1,406,101.0 | 757,287.0      | 53.86                       | 46.14                       |
| 09/02/22   | 1,907,107.0 | 1,017,824.0    | 53.37                       | 46.63                       |
| 09/03/22   | 2,210,988.0 | 1,079,256.0    | 48.81                       | 51.19                       |
| 09/04/22   | 2,294,009.0 | 1,063,997.0    | 46.38                       | 53.62                       |
| 09/05/22   | 1,784,690.0 | 714,459.0      | 40.03                       | 59.97                       |
| 09/06/22   | 1,330,931.0 | 495,058.0      | 37.2                        | 62.8                        |
| 09/07/22   | 1,807,800.0 | 710,154.0      | 39.28                       | 60.72                       |
| 09/08/22   | 2,099,508.0 | 887,959.0      | 42.29                       | 57.71                       |

### Explanation:
Total Revenue Calculation (t1):  
Aggregates the total revenue from non-canceled orders each day. This includes revenue from both new and returning users.
Revenue from New Users:  
New Users (t5): Identifies each user’s first interaction date.  
Revenue Calculation for New Users (t6): Joins on the first interaction date to capture revenue only from orders placed by users on their first active day, filtering out canceled orders.  
Final Calculations:  
new_users_revenue_share: Calculates the percentage of revenue from new users relative to the total daily revenue.  
old_users_revenue_share: The remainder of the revenue percentage (from returning users) is calculated as 100 - new_users_revenue_share.  






## Task 6

For each product listed in the `products` table, calculate the following metrics over the entire period in the `orders` table:

1. **Total Revenue (`revenue`)** — total revenue generated from sales of each product.
2. **Revenue Share (`share_in_revenue`)** — percentage share of each product's revenue relative to the overall revenue.

If the rounded revenue share for any product is less than 0.5%, group it under the label "ДРУГОЕ" (OTHER) by summing up the revenue and revenue shares of such products.

### Description

This query analyzes product performance by calculating total revenue and the contribution of each product to overall revenue. Products generating less than 0.5% of total revenue are grouped under "ДРУГОЕ."

1. **Revenue**: Total revenue generated by each product over the entire period.
2. **Share in Revenue**: Percentage of each product’s revenue contribution to the overall revenue, rounded to two decimal places.

### SQL Query

```sql
SELECT product_name,
       sum(revenue) as revenue,
       sum(share_in_revenue) as share_in_revenue
FROM   (SELECT case when round(100 * revenue / sum(revenue) OVER (), 2) >= 0.5 then name
                    else 'OTHER' end as product_name,
               revenue,
               round(100 * revenue / sum(revenue) OVER (), 2) as share_in_revenue
        FROM   (SELECT name,
                       sum(price) as revenue
                FROM   (SELECT order_id,
                               unnest(product_ids) as product_id
                        FROM   orders
                        WHERE  order_id not in (SELECT order_id
                                                FROM   user_actions
                                                WHERE  action = 'cancel_order')) t1
                    LEFT JOIN products using(product_id)
                GROUP BY name) t2) t3
GROUP BY product_name
ORDER BY revenue desc;
```

| Product Name     | Revenue    | Share in Revenue (%) |
|------------------|------------|-----------------------|
| Product Name 1   | 1,353,600.0 | 6.24                |
| OTHER            | 1,225,387.0 | 5.64                |
| Product Name 3   | 1,171,140.0 | 5.4                 |
| Product Name 4   | 1,163,250.0 | 5.37                |
| Product Name 5   | 977,170.0   | 4.51                |
| ...              | ...         | ...                 |
| Product Name 20  | 206,280.0   | 0.95                |
| Product Name 21  | 202,160.0   | 0.93                |
| Product Name 22  | 199,800.0   | 0.92                |
| Product Name 23  | 194,350.0   | 0.9                 |
| Product Name 24  | 179,676.0   | 0.83                |
| ...              | ...         | ...                 |
| Product Name 40  | 118,930.0   | 0.55                |
| Product Name 41  | 112,580.0   | 0.52                |
| Product Name 42  | 112,448.0   | 0.52                |
| Product Name 43  | 109,750.0   | 0.51                |
| Product Name 44  | 109,650.0   | 0.51                |

### Explanation:
Product Revenue Calculation (t2):  
Aggregates the total revenue generated by each product over all non-canceled orders.  
Revenue Share and Product Grouping:  
share_in_revenue: Calculates the revenue share percentage for each product relative to the total revenue.  
Products with a revenue share less than 0.5% are grouped under "ДРУГОЕ" by summing up their revenue and revenue share values.  
Final Aggregation:  
Groups by product_name, so that products in the "ДРУГОЕ" category are combined, and sorts by revenue in descending order for a clear view of top-performing products.  







## Task 7

For each day, calculate the following financial metrics from the `orders` and `courier_actions` tables:

1. **Daily Revenue (`revenue`)** — total revenue generated on the day.
2. **Daily Costs (`costs`)** — costs incurred on the day, including variable and fixed costs.
3. **Daily Tax (`tax`)** — tax collected from sales on the day.
4. **Daily Gross Profit (`gross_profit`)** — net profit for the day, calculated as `revenue - costs - tax`.
5. **Cumulative Revenue (`total_revenue`)** — cumulative revenue up to the current day.
6. **Cumulative Costs (`total_costs`)** — cumulative costs up to the current day.
7. **Cumulative Tax (`total_tax`)** — cumulative tax collected up to the current day.
8. **Cumulative Gross Profit (`total_gross_profit`)** — cumulative gross profit up to the current day.
9. **Gross Profit Ratio (`gross_profit_ratio`)** — percentage of gross profit relative to daily revenue.
10. **Cumulative Gross Profit Ratio (`total_gross_profit_ratio`)** — percentage of cumulative gross profit relative to cumulative revenue.

### Description

This query calculates daily and cumulative financial metrics, allowing for an analysis of profitability trends. Metrics are as follows:

1. **Revenue**: Total revenue generated per day from completed sales.
2. **Costs**: Total costs per day, accounting for variable costs (number of orders) and fixed costs.
3. **Tax**: Tax collected on sales, calculated per product based on tax category.
4. **Gross Profit**: Daily net profit after deducting costs and taxes from revenue.
5. **Cumulative Metrics**: Cumulative totals for revenue, costs, tax, and gross profit to track financial trends over time.
6. **Profit Ratios**: Gross profit as a percentage of daily and cumulative revenue.

### SQL Query

```sql
SELECT date,
       revenue,
       costs,
       tax,
       gross_profit,
       total_revenue,
       total_costs,
       total_tax,
       total_gross_profit,
       round(gross_profit / revenue * 100, 2) as gross_profit_ratio,
       round(total_gross_profit / total_revenue * 100, 2) as total_gross_profit_ratio
FROM   (SELECT date,
               revenue,
               costs,
               tax,
               revenue - costs - tax as gross_profit,
               sum(revenue) OVER (ORDER BY date) as total_revenue,
               sum(costs) OVER (ORDER BY date) as total_costs,
               sum(tax) OVER (ORDER BY date) as total_tax,
               sum(revenue - costs - tax) OVER (ORDER BY date) as total_gross_profit
        FROM   (SELECT date,
                       orders_packed,
                       orders_delivered,
                       couriers_count,
                       revenue,
                       case when date_part('month',date) = 8 then 120000.0 + 140 * coalesce(orders_packed, 0) + 150 * coalesce(orders_delivered, 0) + 400 * coalesce(couriers_count, 0)
                            when date_part('month', date) = 9 then 150000.0 + 115 * coalesce(orders_packed, 0) + 150 * coalesce(orders_delivered, 0) + 500 * coalesce(couriers_count, 0) end as costs,
                       tax
                FROM   (SELECT creation_time::date as date,
                               count(distinct order_id) as orders_packed,
                               sum(price) as revenue,
                               sum(tax) as tax
                        FROM   (SELECT order_id,
                                       creation_time,
                                       product_id,
                                       name,
                                       price,
                                       case when name in ('сахар', 'сухарики', 'сушки', 'семечки', 'масло льняное', 'виноград', 'масло оливковое', 'арбуз', 'батон', 'йогурт', 'сливки', 'гречка', 'овсянка', 'макароны', 'баранина', 'апельсины', 'бублики', 'хлеб', 'горох', 'сметана', 'рыба копченая', 'мука', 'шпроты', 'сосиски', 'свинина', 'рис', 'масло кунжутное', 'сгущенка', 'ананас', 'говядина', 'соль', 'рыба вяленая', 'масло подсолнечное', 'яблоки', 'груши', 'лепешка', 'молоко', 'курица', 'лаваш', 'вафли', 'мандарины') then round(price/110*10,
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         2)
                                            else round(price/120*20, 2) end as tax
                                FROM   (SELECT order_id,
                                               creation_time,
                                               unnest(product_ids) as product_id
                                        FROM   orders
                                        WHERE  order_id not in (SELECT order_id
                                                                FROM   user_actions
                                                                WHERE  action = 'cancel_order')) t1
                                    LEFT JOIN products using (product_id)) t2
                        GROUP BY date) t3
                    LEFT JOIN (SELECT time::date as date,
                                      count(distinct order_id) as orders_delivered
                               FROM   courier_actions
                               WHERE  order_id not in (SELECT order_id
                                                       FROM   user_actions
                                                       WHERE  action = 'cancel_order')
                                  and action = 'deliver_order'
                               GROUP BY date) t4 using (date)
                    LEFT JOIN (SELECT date,
                                      count(courier_id) as couriers_count
                               FROM   (SELECT time::date as date,
                                              courier_id,
                                              count(distinct order_id) as orders_delivered
                                       FROM   courier_actions
                                       WHERE  order_id not in (SELECT order_id
                                                               FROM   user_actions
                                                               WHERE  action = 'cancel_order')
                                          and action = 'deliver_order'
                                       GROUP BY date, courier_id having count(distinct order_id) >= 5) t5
                               GROUP BY date) t6 using (date)) t7) t8;
```

| Date       | Revenue   | Costs     | Tax       | Gross Profit | Total Revenue | Total Costs | Total Tax   | Total Gross Profit | Gross Profit Ratio (%) | Total Gross Profit Ratio (%) |
|------------|-----------|-----------|-----------|--------------|---------------|-------------|-------------|---------------------|------------------------|------------------------------|
| 08/24/22   | 49,924.0  | 159,120.0 | 6,334.09  | -115,530.09  | 49,924.0      | 159,120.0   | 6,334.09    | -115,530.09        | -231.41               | -231.41                      |
| 08/25/22   | 430,860.0 | 447,560.0 | 53,545.01 | -70,245.01   | 480,784.0     | 606,680.0   | 59,879.1    | -185,775.1         | -16.3                 | -38.64                       |
| 08/26/22   | 534,766.0 | 565,680.0 | 66,229.97 | -97,143.97   | 1,015,550.0   | 1,172,360.0 | 126,109.07  | -282,919.07        | -18.17                | -27.86                       |
| 08/27/22   | 817,053.0 | 781,040.0 | 102,245.77| -66,232.77   | 1,832,603.0   | 1,953,400.0 | 228,354.84  | -349,151.84        | -8.11                 | -19.05                       |
| 08/28/22   | 1,133,370.0 | 1,055,870.0 | 140,881.28 | -63,381.28 | 2,965,973.0   | 3,009,270.0 | 369,236.12  | -412,533.12        | -5.59                 | -13.91                       |
| 08/29/22   | 1,279,891.0 | 1,144,280.0 | 160,378.17 | -24,767.17 | 4,245,864.0   | 4,153,550.0 | 529,614.29  | -437,300.29        | -1.94                 | -10.3                        |
| 08/30/22   | 1,279,377.0 | 1,169,140.0 | 159,020.61 | -48,783.61 | 5,525,241.0   | 5,322,690.0 | 688,634.9   | -486,083.9         | -3.81                 | -8.8                         |
| 08/31/22   | 1,312,720.0 | 1,159,250.0 | 162,871.96 | -9,401.96  | 6,837,961.0   | 6,481,940.0 | 851,506.86  | -495,485.86        | -0.72                 | -7.25                        |
| 09/01/22   | 1,406,101.0 | 1,180,320.0 | 175,026.05 | 50,754.95  | 8,244,062.0   | 7,662,260.0 | 1,026,532.91| -444,730.91        | 3.61                  | -5.39                        |
| 09/02/22   | 1,907,107.0 | 1,590,965.0 | 237,729.71 | 78,412.29  | 10,151,169.0  | 9,253,225.0 | 1,264,262.62| -366,318.62        | 4.11                  | -3.61                        |
| 09/03/22   | 2,210,988.0 | 1,813,235.0 | 276,335.45 | 121,417.55 | 12,362,157.0  | 11,066,460.0| 1,540,598.07| -244,901.07        | 5.49                  | -1.98                        |
| 09/04/22   | 2,294,009.0 | 1,885,300.0 | 287,736.54 | 120,972.46 | 14,656,166.0  | 12,951,760.0| 1,828,334.61| -123,928.61        | 5.27                  | -0.85                        |
| 09/05/22   | 1,784,690.0 | 1,456,825.0 | 222,821.25 | 105,043.75 | 16,440,856.0  | 14,408,585.0| 2,051,155.86| -18,884.86         | 5.89                  | -0.11                        |
| 09/06/22   | 1,330,931.0 | 1,077,765.0 | 165,911.75 | 87,254.25  | 17,771,787.0  | 15,486,350.0| 2,217,067.61| 68,369.39          | 6.56                  | 0.38                         |
| 09/07/22   | 1,807,800.0 | 1,452,755.0 | 226,142.45 | 128,902.55 | 19,579,587.0  | 16,939,105.0| 2,443,210.06| 197,271.94         | 7.13                  | 1.01                         |
| 09/08/22   | 2,099,508.0 | 1,669,410.0 | 263,641.82 | 166,456.18 | 21,679,095.0  | 18,608,515.0| 2,706,851.88| 363,728.12         | 7.93                  | 1.68                         |

### Explanation
Daily Metrics Calculation:  
Revenue: Sum of prices for non-canceled orders.  
Costs: Fixed and variable costs calculated based on order counts and courier activity, varying by month.  
Tax: Calculated per product based on product type, either at 10% or 20% of price.  
Gross Profit: Calculated as revenue - costs - tax.  
Cumulative Metrics:  
Uses window functions (SUM with OVER (ORDER BY date)) to compute cumulative revenue, costs, tax, and gross profit.  
Profit Ratios:  
gross_profit_ratio: Ratio of daily gross profit to daily revenue.  
total_gross_profit_ratio: Ratio of cumulative gross profit to cumulative revenue.  





