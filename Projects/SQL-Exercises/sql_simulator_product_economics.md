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
                    else 'ДРУГОЕ' end as product_name,
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
                       case when date_part('month',
                                                                                                                                                                      date) = 8 then 120000.0 + 140 * coalesce(orders_packed, 0) + 150 * coalesce(orders_delivered, 0) + 400 * coalesce(couriers_count, 0)
                            when date_part('month',
                                                                                                                                                                      date) = 9 then 150000.0 + 115 * coalesce(orders_packed, 0) + 150 * coalesce(orders_delivered, 0) + 500 * coalesce(couriers_count, 0) end as costs,
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





