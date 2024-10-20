## Metrics and Graphs

## SQL Task Links

|               |                 |               |             |                |
|-----------------------|-----------------------|-----------------------|-----------------------|-----------------------|
| [Task 1](#task-1)     | [Task 2](#task-2)     | [Task 3](#task-3)     | [Task 4](#task-4)     | [Task 5](#task-5)     |
| [Task 6](#task-6)     | [Task 7](#task-7)     | [Task 8](#task-8)     | [Task 9](#task-9)     | [Task 10](#task-10)   |
| [Task 11](#task-11)   | [Task 12](#task-12)   | [Task 13](#task-13)   | [Task 14](#task-14)   | [Task 15](#task-15)   |
| [Task 16](#task-16)   | [Task 17](#task-17)   | [Task 18](#task-18)   |    


### Task
For each day represented in the `user_actions` and `courier_actions` tables, calculate the following metrics:
- Number of new users.
- Number of new couriers.
- Total number of users as of the current day.
- Total number of couriers as of the current day.

### Description
The query should return the following columns:
- `date` — the date.
- `new_users` — the number of new users.
- `new_couriers` — the number of new couriers.
- `total_users` — the total number of users as of the current day.
- `total_couriers` — the total number of couriers as of the current day.

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






































