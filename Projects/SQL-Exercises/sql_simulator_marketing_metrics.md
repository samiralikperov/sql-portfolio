## Marketing Metrics

## SQL Task Links

|               |                 |               |             |                |
|-----------------------|-----------------------|-----------------------|-----------------------|-----------------------|
| [Task 1](#task-1)     | [Task 2](#task-2)     | [Task 3](#task-3)     | [Task 4](#task-4)     | [Task 5](#task-5)     |
| [Task 6](#task-6)     |



## Task 1 

Calculate the **Customer Acquisition Cost (CAC)** for two specific advertising campaigns based on data from the `user_actions` table. CAC represents the cost per acquired customer for each campaign.

1. **Campaign 1**: A well-known YouTube blogger promoted the app, with a total cost of $250,000, resulting in 171 new registrations on September 1.
2. **Campaign 2**: Targeted ads on social media, with the same cost of $250,000, led to 236 new registrations on September 1.

**Note**: Only users who completed at least one order that wasn’t canceled are considered buyers.

### SQL Query

```sql
SELECT 
    CONCAT('Campaign № ', ads_campaign) AS ads_campaign,
    ROUND(250000.0 / COUNT(DISTINCT user_id), 2) AS cac
FROM (
    SELECT 
        user_id,
        order_id,
        action,
        CASE 
            WHEN user_id IN (
                8631, 8632, 8638, 8643, 8657, 8673, 8706, 8707, 8715, 8723, 8732, 
                8739, 8741, 8750, 8751, 8752, 8770, 8774, 8788, 8791, 8804, 8810, 
                8815, 8828, 8830, 8845, 8853, 8859, 8867, 8869, 8876, 8879, 8883, 
                8896, 8909, 8911, 8933, 8940, 8972, 8976, 8988, 8990, 9002, 9004, 
                9009, 9019, 9020, 9035, 9036, 9061, 9069, 9071, 9075, 9081, 9085, 
                9089, 9108, 9113, 9144, 9145, 9146, 9162, 9165, 9167, 9175, 9180, 
                9182, 9197, 9198, 9210, 9223, 9251, 9257, 9278, 9287, 9291, 9313, 
                9317, 9321, 9334, 9351, 9391, 9398, 9414, 9420, 9422, 9431, 9450, 
                9451, 9454, 9472, 9476, 9478, 9491, 9494, 9505, 9512, 9518, 9524, 
                9526, 9528, 9531, 9535, 9550, 9559, 9561, 9562, 9599, 9603, 9605, 
                9611, 9612, 9615, 9625, 9633, 9652, 9654, 9655, 9660, 9662, 9667, 
                9677, 9679, 9689, 9695, 9720, 9726, 9739, 9740, 9762, 9778, 9786, 
                9794, 9804, 9810, 9813, 9818, 9828, 9831, 9836, 9838, 9845, 9871, 
                9887, 9891, 9896, 9897, 9916, 9945, 9960, 9963, 9965, 9968, 9971, 
                9993, 9998, 9999, 10001, 10013, 10016, 10023, 10030, 10051, 10057, 
                10064, 10082, 10103, 10105, 10122, 10134, 10135
            ) THEN 1
            WHEN user_id IN (
                8629, 8630, 8644, 8646, 8650, 8655, 8659, 8660, 8663, 8665, 8670, 
                8675, 8680, 8681, 8682, 8683, 8694, 8697, 8700, 8704, 8712, 8713, 
                8719, 8729, 8733, 8742, 8748, 8754, 8771, 8794, 8795, 8798, 8803, 
                8805, 8806, 8812, 8814, 8825, 8827, 8838, 8849, 8851, 8854, 8855, 
                8870, 8878, 8882, 8886, 8890, 8893, 8900, 8902, 8913, 8916, 8923, 
                8929, 8935, 8942, 8943, 8949, 8953, 8955, 8966, 8968, 8971, 8973, 
                8980, 8995, 8999, 9000, 9007, 9013, 9041, 9042, 9047, 9064, 9068, 
                9077, 9082, 9083, 9095, 9103, 9109, 9117, 9123, 9127, 9131, 9137, 
                9140, 9149, 9161, 9179, 9181, 9183, 9185, 9190, 9196, 9203, 9207, 
                9226, 9227, 9229, 9230, 9231, 9250, 9255, 9259, 9267, 9273, 9281, 
                9282, 9289, 9292, 9303, 9310, 9312, 9315, 9327, 9333, 9335, 9337, 
                9343, 9356, 9368, 9370, 9383, 9392, 9404, 9410, 9421, 9428, 9432, 
                9437, 9468, 9479, 9483, 9485, 9492, 9495, 9497, 9498, 9500, 9510, 
                9527, 9529, 9530, 9538, 9539, 9545, 9557, 9558, 9560, 9564, 9567, 
                9570, 9591, 9596, 9598, 9616, 9631, 9634, 9635, 9636, 9658, 9666, 
                9672, 9684, 9692, 9700, 9704, 9706, 9711, 9719, 9727, 9735, 9741, 
                9744, 9749, 9752, 9753, 9755, 9757, 9764, 9783, 9784, 9788, 9790, 
                9808, 9820, 9839, 9841, 9843, 9853, 9855, 9859, 9863, 9877, 9879, 
                9880, 9882, 9883, 9885, 9901, 9904, 9908, 9910, 9912, 9920, 9929, 
                9930, 9935, 9939, 9958, 9959, 9961, 9983, 10027, 10033, 10038, 
                10045, 10047, 10048, 10058, 10059, 10067, 10069, 10073, 10075, 
                10078, 10079, 10081, 10092, 10106, 10110, 10113, 10131
            ) THEN 2
            ELSE 0 
        END AS ads_campaign,
        COUNT(action) FILTER (WHERE action = 'cancel_order') OVER (PARTITION BY order_id) AS is_canceled
    FROM user_actions
) t1
WHERE ads_campaign IN (1, 2)
  AND is_canceled = 0
GROUP BY ads_campaign
ORDER BY cac DESC;
```
### Explanation:
Campaign Identification:  
User IDs are mapped to either Campaign 1 or Campaign 2 based on predefined lists.  
CAC Calculation:  
cac is calculated as the campaign cost (250,000) divided by the number of unique users who made non-canceled orders for each campaign. This provides the average cost of acquiring one paying customer for each advertising campaign.  






## Task 2

Calculate the **Return on Investment (ROI)** for each of the two advertising campaigns. This metric evaluates the profitability of each campaign by comparing the revenue generated to the campaign cost.


- **Campaign Names**: Label each campaign as "Campaign № 1" and "Campaign № 2."
- **Metric Column**: Name the ROI metric column as `roi`.


### SQL Query

```sql
SELECT 
    CONCAT('Campaign № ', ads_campaign) AS ads_campaign,
    ROUND((SUM(price) - 250000.0) / 250000.0 * 100, 2) AS roi
FROM (
    SELECT 
        ads_campaign,
        user_id,
        order_id,
        product_id,
        price
    FROM (
        SELECT 
            ads_campaign,
            user_id,
            order_id
        FROM (
            SELECT 
                user_id,
                order_id,
                CASE 
                    WHEN user_id IN (
                        8631, 8632, 8638, 8643, 8657, 8673, 8706, 8707, 8715, 8723, 8732, 
                        8739, 8741, 8750, 8751, 8752, 8770, 8774, 8788, 8791, 8804, 8810, 
                        8815, 8828, 8830, 8845, 8853, 8859, 8867, 8869, 8876, 8879, 8883, 
                        8896, 8909, 8911, 8933, 8940, 8972, 8976, 8988, 8990, 9002, 9004, 
                        9009, 9019, 9020, 9035, 9036, 9061, 9069, 9071, 9075, 9081, 9085, 
                        9089, 9108, 9113, 9144, 9145, 9146, 9162, 9165, 9167, 9175, 9180, 
                        9182, 9197, 9198, 9210, 9223, 9251, 9257, 9278, 9287, 9291, 9313, 
                        9317, 9321, 9334, 9351, 9391, 9398, 9414, 9420, 9422, 9431, 9450, 
                        9451, 9454, 9472, 9476, 9478, 9491, 9494, 9505, 9512, 9518, 9524, 
                        9526, 9528, 9531, 9535, 9550, 9559, 9561, 9562, 9599, 9603, 9605, 
                        9611, 9612, 9615, 9625, 9633, 9652, 9654, 9655, 9660, 9662, 9667, 
                        9677, 9679, 9689, 9695, 9720, 9726, 9739, 9740, 9762, 9778, 9786, 
                        9794, 9804, 9810, 9813, 9818, 9828, 9831, 9836, 9838, 9845, 9871, 
                        9887, 9891, 9896, 9897, 9916, 9945, 9960, 9963, 9965, 9968, 9971, 
                        9993, 9998, 9999, 10001, 10013, 10016, 10023, 10030, 10051, 10057, 
                        10064, 10082, 10103, 10105, 10122, 10134, 10135
                    ) THEN 1
                    WHEN user_id IN (
                        8629, 8630, 8644, 8646, 8650, 8655, 8659, 8660, 8663, 8665, 8670, 
                        8675, 8680, 8681, 8682, 8683, 8694, 8697, 8700, 8704, 8712, 8713, 
                        8719, 8729, 8733, 8742, 8748, 8754, 8771, 8794, 8795, 8798, 8803, 
                        8805, 8806, 8812, 8814, 8825, 8827, 8838, 8849, 8851, 8854, 8855, 
                        8870, 8878, 8882, 8886, 8890, 8893, 8900, 8902, 8913, 8916, 8923, 
                        8929, 8935, 8942, 8943, 8949, 8953, 8955, 8966, 8968, 8971, 8973, 
                        8980, 8995, 8999, 9000, 9007, 9013, 9041, 9042, 9047, 9064, 9068, 
                        9077, 9082, 9083, 9095, 9103, 9109, 9117, 9123, 9127, 9131, 9137, 
                        9140, 9149, 9161, 9179, 9181, 9183, 9185, 9190, 9196, 9203, 9207, 
                        9226, 9227, 9229, 9230, 9231, 9250, 9255, 9259, 9267, 9273, 9281, 
                        9282, 9289, 9292, 9303, 9310, 9312, 9315, 9327, 9333, 9335, 9337, 
                        9343, 9356, 9368, 9370, 9383, 9392, 9404, 9410, 9421, 9428, 9432, 
                        9437, 9468, 9479, 9483, 9485, 9492, 9495, 9497, 9498, 9500, 9510, 
                        9527, 9529, 9530, 9538, 9539, 9545, 9557, 9558, 9560, 9564, 9567, 
                        9570, 9591, 9596, 9598, 9616, 9631, 9634, 9635, 9636, 9658, 9666, 
                        9672, 9684, 9692, 9700, 9704, 9706, 9711, 9719, 9727, 9735, 9741, 
                        9744, 9749, 9752, 9753, 9755, 9757, 9764, 9783, 9784, 9788, 9790, 
                        9808, 9820, 9839, 9841, 9843, 9853, 9855, 9859, 9863, 9877, 9879, 
                        9880, 9882, 9883, 9885, 9901, 9904, 9908, 9910, 9912, 9920, 9929, 
                        9930, 9935, 9939, 9958, 9959, 9961, 9983, 10027, 10033, 10038, 
                        10045, 10047, 10048, 10058, 10059, 10067, 10069, 10073, 10075, 
                        10078, 10079, 10081, 10092, 10106, 10110, 10113, 10131
                    ) THEN 2
                    ELSE 0 
                END AS ads_campaign,
                COUNT(action) FILTER (WHERE action = 'cancel_order') OVER (PARTITION BY order_id) AS is_canceled
            FROM user_actions
        ) t1
        WHERE ads_campaign IN (1, 2)
          AND is_canceled = 0
    ) t2
    LEFT JOIN (
        SELECT 
            order_id,
            UNNEST(product_ids) AS product_id
        FROM orders
    ) t3 USING(order_id)
    LEFT JOIN products USING(product_id)
) t4
GROUP BY ads_campaign
ORDER BY roi DESC;
```
### Explanation:
Campaign Identification:  
User IDs are mapped to Campaign 1 or Campaign 2 based on predefined lists.  
Revenue Calculation:  
Revenue is aggregated from completed orders for each campaign, joining with the products table to retrieve prices.
ROI Calculation:  
ROI is calculated using the formula   
(Total Revenue − Campaign Cost)/Campaign Cost × 100 to express it as a percentage, rounded to two decimal places.








## Task 3
### Description
Calculate the **average order value** for users acquired through each advertising campaign during their first week of app usage (from September 1 to September 7, 2022).

**Metric Calculation**:
1. For each user, calculate their average order value during the specified week.
2. Then, find the average of these user-specific averages for each campaign.


### SQL Query

```sql
SELECT 
    CONCAT('Campaign № ', ads_campaign) AS ads_campaign,
    ROUND(AVG(user_avg_check), 2) AS avg_check
FROM (
    SELECT 
        ads_campaign,
        user_id,
        ROUND(AVG(order_price), 2) AS user_avg_check
    FROM (
        SELECT 
            ads_campaign,
            user_id,
            order_id,
            SUM(price) AS order_price
        FROM (
            SELECT 
                ads_campaign,
                user_id,
                order_id,
                product_id,
                price
            FROM (
                SELECT 
                    ads_campaign,
                    user_id,
                    order_id,
                    time
                FROM (
                    SELECT 
                        user_id,
                        order_id,
                        time,
                        CASE 
                            WHEN user_id IN (
                                8631, 8632, 8638, 8643, 8657, 8673, 8706, 8707, 8715, 8723, 8732, 
                                8739, 8741, 8750, 8751, 8752, 8770, 8774, 8788, 8791, 8804, 8810, 
                                8815, 8828, 8830, 8845, 8853, 8859, 8867, 8869, 8876, 8879, 8883, 
                                8896, 8909, 8911, 8933, 8940, 8972, 8976, 8988, 8990, 9002, 9004, 
                                9009, 9019, 9020, 9035, 9036, 9061, 9069, 9071, 9075, 9081, 9085, 
                                9089, 9108, 9113, 9144, 9145, 9146, 9162, 9165, 9167, 9175, 9180, 
                                9182, 9197, 9198, 9210, 9223, 9251, 9257, 9278, 9287, 9291, 9313, 
                                9317, 9321, 9334, 9351, 9391, 9398, 9414, 9420, 9422, 9431, 9450, 
                                9451, 9454, 9472, 9476, 9478, 9491, 9494, 9505, 9512, 9518, 9524, 
                                9526, 9528, 9531, 9535, 9550, 9559, 9561, 9562, 9599, 9603, 9605, 
                                9611, 9612, 9615, 9625, 9633, 9652, 9654, 9655, 9660, 9662, 9667, 
                                9677, 9679, 9689, 9695, 9720, 9726, 9739, 9740, 9762, 9778, 9786, 
                                9794, 9804, 9810, 9813, 9818, 9828, 9831, 9836, 9838, 9845, 9871, 
                                9887, 9891, 9896, 9897, 9916, 9945, 9960, 9963, 9965, 9968, 9971, 
                                9993, 9998, 9999, 10001, 10013, 10016, 10023, 10030, 10051, 10057, 
                                10064, 10082, 10103, 10105, 10122, 10134, 10135
                            ) THEN 1
                            WHEN user_id IN (
                                8629, 8630, 8644, 8646, 8650, 8655, 8659, 8660, 8663, 8665, 8670, 
                                8675, 8680, 8681, 8682, 8683, 8694, 8697, 8700, 8704, 8712, 8713, 
                                8719, 8729, 8733, 8742, 8748, 8754, 8771, 8794, 8795, 8798, 8803, 
                                8805, 8806, 8812, 8814, 8825, 8827, 8838, 8849, 8851, 8854, 8855, 
                                8870, 8878, 8882, 8886, 8890, 8893, 8900, 8902, 8913, 8916, 8923, 
                                8929, 8935, 8942, 8943, 8949, 8953, 8955, 8966, 8968, 8971, 8973, 
                                8980, 8995, 8999, 9000, 9007, 9013, 9041, 9042, 9047, 9064, 9068, 
                                9077, 9082, 9083, 9095, 9103, 9109, 9117, 9123, 9127, 9131, 9137, 
                                9140, 9149, 9161, 9179, 9181, 9183, 9185, 9190, 9196, 9203, 9207, 
                                9226, 9227, 9229, 9230, 9231, 9250, 9255, 9259, 9267, 9273, 9281, 
                                9282, 9289, 9292, 9303, 9310, 9312, 9315, 9327, 9333, 9335, 9337, 
                                9343, 9356, 9368, 9370, 9383, 9392, 9404, 9410, 9421, 9428, 9432, 
                                9437, 9468, 9479, 9483, 9485, 9492, 9495, 9497, 9498, 9500, 9510, 
                                9527, 9529, 9530, 9538, 9539, 9545, 9557, 9558, 9560, 9564, 9567, 
                                9570, 9591, 9596, 9598, 9616, 9631, 9634, 9635, 9636, 9658, 9666, 
                                9672, 9684, 9692, 9700, 9704, 9706, 9711, 9719, 9727, 9735, 9741, 
                                9744, 9749, 9752, 9753, 9755, 9757, 9764, 9783, 9784, 9788, 9790, 
                                9808, 9820, 9839, 9841, 9843, 9853, 9855, 9859, 9863, 9877, 9879, 
                                9880, 9882, 9883, 9885, 9901, 9904, 9908, 9910, 9912, 9920, 9929, 
                                9930, 9935, 9939, 9958, 9959, 9961, 9983, 10027, 10033, 10038, 
                                10045, 10047, 10048, 10058, 10059, 10067, 10069, 10073, 10075, 
                                10078, 10079, 10081, 10092, 10106, 10110, 10113, 10131
                            ) THEN 2
                            ELSE 0 
                        END AS ads_campaign,
                        COUNT(action) FILTER (WHERE action = 'cancel_order') OVER (PARTITION BY order_id) AS is_canceled
                    FROM user_actions
                ) t1
                WHERE ads_campaign IN (1, 2)
                  AND is_canceled = 0
                  AND time::date >= '2022-09-01'
                  AND time::date < '2022-09-08'
            ) t2
            LEFT JOIN (
                SELECT 
                    order_id,
                    UNNEST(product_ids) AS product_id
                FROM orders
            ) t3 USING(order_id)
            LEFT JOIN products USING(product_id)
        ) t4
        GROUP BY ads_campaign, user_id, order_id
    ) t5
    GROUP BY ads_campaign, user_id
) t6
GROUP BY ads_campaign
ORDER BY avg_check DESC;
```
### Explanation
Campaign Identification:  
Users are mapped to Campaign 1 or Campaign 2 based on the provided lists.
Average Order Value:  
For each user, the average cost of each order they placed within the specified week is calculated.  
The overall average of these user-level averages is then calculated for each campaign, giving the average order value (avg_check) for each campaign during the first week.







## Task 4

Calculate the **Daily Retention** rate for all users based on data from the `user_actions` table. Group users into cohorts by the date of their first interaction with the app.

**Result Columns**:
1. **Start Month** (`start_month`) — the month of the user's first interaction.
2. **Start Date** (`start_date`) — the exact date of the user's first interaction.
3. **Day Number** (`day_number`) — the number of days since the first interaction, starting from 0.
4. **Retention** (`retention`) — the percentage of users from each cohort that returned on a given day.

### SQL Query

```sql
SELECT 
    date_trunc('month', start_date)::date AS start_month,
    start_date,
    date - start_date AS day_number,
    ROUND(users::decimal / MAX(users) OVER (PARTITION BY start_date), 2) AS retention
FROM (
    SELECT 
        start_date,
        time::date AS date,
        COUNT(DISTINCT user_id) AS users
    FROM (
        SELECT 
            user_id,
            time::date,
            MIN(time::date) OVER (PARTITION BY user_id) AS start_date
        FROM user_actions
    ) t1
    GROUP BY start_date, time::date
) t2;
```
### Explanation:  
Cohort Identification:  
Each user is assigned to a cohort based on their first interaction date (start_date).
Day Number Calculation:  
For each day (date) following the first interaction (start_date), calculate the day number (day_number) as the difference between the date and start_date.
Retention Calculation:  
Retention is calculated as the number of users who returned on a specific day_number divided by the total users in the cohort on start_date.  
This percentage represents the Daily Retention rate for each cohort.








## Task 5

Calculate the **1-day and 7-day Retention** for users acquired through each advertising campaign. Group users by their first interaction date and track their retention on the 1st and 7th days.

**Result Columns**:
1. **Campaign Name** (`ads_campaign`) — Label as "Campaign № 1" and "Campaign № 2."
2. **Start Date** (`start_date`) — The date of the user's first interaction with the app.
3. **Day Number** (`day_number`) — The day index, specifically 0 (first interaction), 1, or 7.
4. **Retention** (`retention`) — The percentage of users retained from the cohort on the specified day.

### SQL Query

```sql
SELECT 
    CONCAT('Campaign № ', ads_campaign) AS ads_campaign,
    start_date,
    day_number,
    ROUND(users::decimal / MAX(users) OVER (PARTITION BY ads_campaign, start_date), 2) AS retention
FROM (
    SELECT 
        ads_campaign,
        start_date,
        date - start_date AS day_number,
        COUNT(DISTINCT user_id) AS users
    FROM (
        SELECT 
            ads_campaign,
            user_id,
            date,
            MIN(date) OVER (PARTITION BY ads_campaign, user_id) AS start_date
        FROM (
            SELECT 
                user_id,
                time::date AS date,
                CASE 
                    WHEN user_id IN (
                        8631, 8632, 8638, 8643, 8657, 8673, 8706, 8707, 8715, 8723, 8732, 
                        8739, 8741, 8750, 8751, 8752, 8770, 8774, 8788, 8791, 8804, 8810, 
                        8815, 8828, 8830, 8845, 8853, 8859, 8867, 8869, 8876, 8879, 8883, 
                        8896, 8909, 8911, 8933, 8940, 8972, 8976, 8988, 8990, 9002, 9004, 
                        9009, 9019, 9020, 9035, 9036, 9061, 9069, 9071, 9075, 9081, 9085, 
                        9089, 9108, 9113, 9144, 9145, 9146, 9162, 9165, 9167, 9175, 9180, 
                        9182, 9197, 9198, 9210, 9223, 9251, 9257, 9278, 9287, 9291, 9313, 
                        9317, 9321, 9334, 9351, 9391, 9398, 9414, 9420, 9422, 9431, 9450, 
                        9451, 9454, 9472, 9476, 9478, 9491, 9494, 9505, 9512, 9518, 9524, 
                        9526, 9528, 9531, 9535, 9550, 9559, 9561, 9562, 9599, 9603, 9605, 
                        9611, 9612, 9615, 9625, 9633, 9652, 9654, 9655, 9660, 9662, 9667, 
                        9677, 9679, 9689, 9695, 9720, 9726, 9739, 9740, 9762, 9778, 9786, 
                        9794, 9804, 9810, 9813, 9818, 9828, 9831, 9836, 9838, 9845, 9871, 
                        9887, 9891, 9896, 9897, 9916, 9945, 9960, 9963, 9965, 9968, 9971, 
                        9993, 9998, 9999, 10001, 10013, 10016, 10023, 10030, 10051, 10057, 
                        10064, 10082, 10103, 10105, 10122, 10134, 10135
                    ) THEN 1
                    WHEN user_id IN (
                        8629, 8630, 8644, 8646, 8650, 8655, 8659, 8660, 8663, 8665, 8670, 
                        8675, 8680, 8681, 8682, 8683, 8694, 8697, 8700, 8704, 8712, 8713, 
                        8719, 8729, 8733, 8742, 8748, 8754, 8771, 8794, 8795, 8798, 8803, 
                        8805, 8806, 8812, 8814, 8825, 8827, 8838, 8849, 8851, 8854, 8855, 
                        8870, 8878, 8882, 8886, 8890, 8893, 8900, 8902, 8913, 8916, 8923, 
                        8929, 8935, 8942, 8943, 8949, 8953, 8955, 8966, 8968, 8971, 8973, 
                        8980, 8995, 8999, 9000, 9007, 9013, 9041, 9042, 9047, 9064, 9068, 
                        9077, 9082, 9083, 9095, 9103, 9109, 9117, 9123, 9127, 9131, 9137, 
                        9140, 9149, 9161, 9179, 9181, 9183, 9185, 9190, 9196, 9203, 9207, 
                        9226, 9227, 9229, 9230, 9231, 9250, 9255, 9259, 9267, 9273, 9281, 
                        9282, 9289, 9292, 9303, 9310, 9312, 9315, 9327, 9333, 9335, 9337, 
                        9343, 9356, 9368, 9370, 9383, 9392, 9404, 9410, 9421, 9428, 9432, 
                        9437, 9468, 9479, 9483, 9485, 9492, 9495, 9497, 9498, 9500, 9510, 
                        9527, 9529, 9530, 9538, 9539, 9545, 9557, 9558, 9560, 9564, 9567, 
                        9570, 9591, 9596, 9598, 9616, 9631, 9634, 9635, 9636, 9658, 9666, 
                        9672, 9684, 9692, 9700, 9704, 9706, 9711, 9719, 9727, 9735, 9741, 
                        9744, 9749, 9752, 9753, 9755, 9757, 9764, 9783, 9784, 9788, 9790, 
                        9808, 9820, 9839, 9841, 9843, 9853, 9855, 9859, 9863, 9877, 9879, 
                        9880, 9882, 9883, 9885, 9901, 9904, 9908, 9910, 9912, 9920, 9929, 
                        9930, 9935, 9939, 9958, 9959, 9961, 9983, 10027, 10033, 10038, 
                        10045, 10047, 10048, 10058, 10059, 10067, 10069, 10073, 10075, 
                        10078, 10079, 10081, 10092, 10106, 10110, 10113, 10131
                    ) THEN 2
                    ELSE 0 
                END AS ads_campaign
            FROM user_actions
        ) t1
        WHERE ads_campaign IN (1, 2)
    ) t2
    GROUP BY ads_campaign, start_date, date
) t3
WHERE day_number IN (0, 1, 7);
```
### Explanation
Campaign Identification:  
Users are grouped under Campaign 1 or Campaign 2 based on specified user IDs.
Day Number Calculation:  
For each interaction day, calculate the day number (difference from the start_date) and filter for 0, 1, and 7.
Retention Calculation:  
For each day (0, 1, and 7), calculate the Retention Rate as the ratio of returning users to the initial number of users on start_date.





## Task 6

For each advertising campaign, calculate the following metrics for each day:

1. **Cumulative ARPPU** (Average Revenue Per Paying User) — the cumulative average revenue generated by paying users up to each day.
2. **CAC** (Customer Acquisition Cost) — the cost per customer acquisition, set as a constant value for all days for visualization purposes.

**Result Columns**:
- **Campaign Name** (`ads_campaign`) — labeled as "Campaign № 1" and "Campaign № 2."
- **Day** (`day`) — the day number starting from Day 0.
- **Cumulative ARPPU** (`cumulative_arppu`) — the cumulative average revenue per paying user.
- **CAC** (`cac`) — the acquisition cost per user, consistent for each day.

### SQL Query

```sql

with main_table as (SELECT ads_campaign,
                           user_id,
                           order_id,
                           time,
                           product_id,
                           price
                    FROM   (SELECT ads_campaign,
                                   user_id,
                                   order_id,
                                   time
                            FROM   (SELECT user_id,
                                           order_id,
                                           time,
                                           case when user_id in (8631, 8632, 8638, 8643, 8657, 8673, 8706, 8707, 8715, 8723, 8732,
                                                                 8739, 8741, 8750, 8751, 8752, 8770, 8774, 8788, 8791,
                                                                 8804, 8810, 8815, 8828, 8830, 8845, 8853, 8859, 8867,
                                                                 8869, 8876, 8879, 8883, 8896, 8909, 8911, 8933, 8940,
                                                                 8972, 8976, 8988, 8990, 9002, 9004, 9009, 9019, 9020,
                                                                 9035, 9036, 9061, 9069, 9071, 9075, 9081, 9085, 9089,
                                                                 9108, 9113, 9144, 9145, 9146, 9162, 9165, 9167, 9175,
                                                                 9180, 9182, 9197, 9198, 9210, 9223, 9251, 9257, 9278,
                                                                 9287, 9291, 9313, 9317, 9321, 9334, 9351, 9391, 9398,
                                                                 9414, 9420, 9422, 9431, 9450, 9451, 9454, 9472, 9476,
                                                                 9478, 9491, 9494, 9505, 9512, 9518, 9524, 9526, 9528,
                                                                 9531, 9535, 9550, 9559, 9561, 9562, 9599, 9603, 9605,
                                                                 9611, 9612, 9615, 9625, 9633, 9652, 9654, 9655, 9660,
                                                                 9662, 9667, 9677, 9679, 9689, 9695, 9720, 9726, 9739,
                                                                 9740, 9762, 9778, 9786, 9794, 9804, 9810, 9813, 9818,
                                                                 9828, 9831, 9836, 9838, 9845, 9871, 9887, 9891, 9896,
                                                                 9897, 9916, 9945, 9960, 9963, 9965, 9968, 9971, 9993,
                                                                 9998, 9999, 10001, 10013, 10016, 10023, 10030, 10051,
                                                                 10057, 10064, 10082, 10103, 10105, 10122, 10134, 10135) then 1
                                                when user_id in (8629, 8630, 8644, 8646, 8650, 8655, 8659, 8660, 8663, 8665, 8670,
                                                                 8675, 8680, 8681, 8682, 8683, 8694, 8697, 8700, 8704,
                                                                 8712, 8713, 8719, 8729, 8733, 8742, 8748, 8754, 8771,
                                                                 8794, 8795, 8798, 8803, 8805, 8806, 8812, 8814, 8825,
                                                                 8827, 8838, 8849, 8851, 8854, 8855, 8870, 8878, 8882,
                                                                 8886, 8890, 8893, 8900, 8902, 8913, 8916, 8923, 8929,
                                                                 8935, 8942, 8943, 8949, 8953, 8955, 8966, 8968, 8971,
                                                                 8973, 8980, 8995, 8999, 9000, 9007, 9013, 9041, 9042,
                                                                 9047, 9064, 9068, 9077, 9082, 9083, 9095, 9103, 9109,
                                                                 9117, 9123, 9127, 9131, 9137, 9140, 9149, 9161, 9179,
                                                                 9181, 9183, 9185, 9190, 9196, 9203, 9207, 9226, 9227,
                                                                 9229, 9230, 9231, 9250, 9255, 9259, 9267, 9273, 9281,
                                                                 9282, 9289, 9292, 9303, 9310, 9312, 9315, 9327, 9333,
                                                                 9335, 9337, 9343, 9356, 9368, 9370, 9383, 9392, 9404,
                                                                 9410, 9421, 9428, 9432, 9437, 9468, 9479, 9483, 9485,
                                                                 9492, 9495, 9497, 9498, 9500, 9510, 9527, 9529, 9530,
                                                                 9538, 9539, 9545, 9557, 9558, 9560, 9564, 9567, 9570,
                                                                 9591, 9596, 9598, 9616, 9631, 9634, 9635, 9636, 9658,
                                                                 9666, 9672, 9684, 9692, 9700, 9704, 9706, 9711, 9719,
                                                                 9727, 9735, 9741, 9744, 9749, 9752, 9753, 9755, 9757,
                                                                 9764, 9783, 9784, 9788, 9790, 9808, 9820, 9839, 9841,
                                                                 9843, 9853, 9855, 9859, 9863, 9877, 9879, 9880, 9882,
                                                                 9883, 9885, 9901, 9904, 9908, 9910, 9912, 9920, 9929,
                                                                 9930, 9935, 9939, 9958, 9959, 9961, 9983, 10027, 10033,
                                                                 10038, 10045, 10047, 10048, 10058, 10059, 10067, 10069,
                                                                 10073, 10075, 10078, 10079, 10081, 10092, 10106, 10110,
                                                                 10113, 10131) then 2
                                                else 0 end as ads_campaign,
                                           count(action) filter (WHERE action = 'cancel_order') OVER (PARTITION BY order_id) as is_canceled
                                    FROM   user_actions) t1
                            WHERE  ads_campaign in (1, 2)
                               and is_canceled = 0) t2
                        LEFT JOIN (SELECT order_id,
                                          unnest(product_ids) as product_id
                                   FROM   orders) t3 using(order_id)
                        LEFT JOIN products using(product_id))
SELECT concat('Campaign № ', ads_campaign) as ads_campaign,
       concat('Day ', row_number() OVER (PARTITION BY ads_campaign
                                         ORDER BY date) - 1) as day,
       round(sum(revenue) OVER (PARTITION BY ads_campaign
                                ORDER BY date) / paying_users::decimal, 2) as cumulative_arppu,
       cac
FROM   (SELECT ads_campaign,
               time::date as date,
               sum(price) as revenue
        FROM   main_table
        GROUP BY ads_campaign, time::date) t1
    LEFT JOIN (SELECT ads_campaign,
                      count(distinct user_id) as paying_users,
                      round(250000.0 / count(distinct user_id), 2) as cac
               FROM   main_table
               GROUP BY ads_campaign) t2 using (ads_campaign);
```
### Explanation:
Campaign Identification:  
Each user is assigned to Campaign 1 or Campaign 2 based on specified user IDs.
Daily Revenue Aggregation:  
Revenue is calculated daily for each campaign, accumulating over time for cumulative metrics.
Cumulative ARPPU Calculation:  
Cumulative ARPPU is the cumulative sum of daily revenue divided by the number of unique paying users.
CAC Calculation:  
CAC is set as a fixed value for each campaign, based on the cost per paying user, to facilitate consistent visualization.
