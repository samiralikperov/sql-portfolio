<details>
  <summary><strong>SELECT</strong></summary>

<details>
  <summary> 1768. Recyclable and Low Fat Products</summary> 

> **Table: Products**  
>   
> | Column Name | Type    |  
> |-------------|---------|  
> | product_id  | int     |  
> | low_fats    | enum    |  
> | recyclable  | enum    |  
>   
> - `product_id` is the primary key (column with unique values) for this table.  
> - `low_fats` is an `ENUM` (category) of type `('Y', 'N')` where `'Y'` means this product is low fat and `'N'` means it is not.  
> - `recyclable` is an `ENUM` (category) of types `('Y', 'N')` where `'Y'` means this product is recyclable and `'N'` means it is not.  
>   
> **Problem Statement:**  
> Write a solution to find the IDs of products that are both low fat and recyclable.  
> Return the result table in any order.  
> The result format is in the following example.  
> 
> **Solution:**
> 
> ```sql  
> SELECT  
>     product_id  
> FROM Products  
> WHERE low_fats = 'Y'   -- Filters only products that are low fat  
>   AND recyclable = 'Y'; -- Filters only products that are recyclable  
> ```  
>   
> **Output:**  
>   
> | product_id |  
> |------------|  
> | 1          |  
> | 3          |  
>   
> **Explanation:**  
> - The query selects the `product_id` from the `Products` table.  
> - It uses the `WHERE` clause to filter the rows where both `low_fats` and `recyclable` columns have the value `'Y'`.  
> - This ensures that only products that are both low fat and recyclable are returned.

</details>


<details>
  <summary> 584. Find Customer Referee</summary> 

> **Table: Customer**  
>   
> | Column Name | Type    |  
> |-------------|---------|  
> | id          | int     |  
> | name        | varchar |  
> | referee_id  | int     |  
>   
> - `id` is the primary key column for this table.  
> - Each row of this table indicates the `id` of a customer, their `name`, and the `id` of the customer who referred them.  
>   
> **Problem Statement:**  
> Find the names of the customers that are not referred by the customer with `id = 2`.  
> Return the result table in any order.  
> The result format is in the following example.  
> 
> **Solution:**
> 
> ```sql  
> SELECT name  
> FROM Customer  
> WHERE 1=1
> AND referee_id IS NULL -- Filters customers who were not referred
> OR referee_id != 2;  -- by customer with id = 2 or have no referee 
> ```  
>   
> **Output:**  
>   
> | name |  
> |------|  
> | Will |  
> | Jane |  
> | Bill |  
> | Zack |  
>   
> **Explanation:**  
> - The query selects the `name` from the `Customer` table.  
> - It uses the `WHERE` clause to filter rows where `referee_id` is either `NULL` or not equal to `2`.  
> - This ensures that only customers who are not referred by the customer with `id = 2` are returned.

</details>

<details>
  <summary> 595. Big Countries</summary> 

> **Table: World**  
>   
> | Column Name | Type    |  
> |-------------|---------|  
> | name        | varchar |  
> | continent   | varchar |  
> | area        | int     |  
> | population  | int     |  
> | gdp         | bigint  |  
>   
> - `name` is the primary key (column with unique values) for this table.  
> - Each row of this table gives information about the name of a country, the continent to which it belongs, its area, the population, and its GDP value.  
>   
> **Problem Statement:**  
> A country is considered big if:  
> - it has an area of at least three million (i.e., 3000000 km²), or  
> - it has a population of at least twenty-five million (i.e., 25000000).  
> Write a solution to find the `name`, `population`, and `area` of the big countries.  
> Return the result table in any order.  
> The result format is in the following example.  
> 
> **Solution:**
> 
> ```sql  
> SELECT name, population, area  
> FROM World  
> WHERE area >= 3000000 OR population >= 25000000;  -- Filters countries with area >= 3,000,000 or population >= 25,000,000  
> ```  
>   
> **Output:**  
>   
> | name        | population | area    |  
> |-------------|------------|---------|  
> | Afghanistan | 25500100   | 652230  |  
> | Algeria     | 37100000   | 2381741 |  
>   
> **Explanation:**  
> - The query selects the `name`, `population`, and `area` from the `World` table.  
> - It uses the `WHERE` clause to filter the rows where either `area` is greater than or equal to `3000000` or `population` is greater than or equal to `25000000`.  
> - This ensures that only the big countries are returned.

</details>

<details>
  <summary> 1148. Article Views I</summary> 

> **Table: Views**  
>   
> | Column Name | Type    |  
> |-------------|---------|  
> | article_id  | int     |  
> | author_id   | int     |  
> | viewer_id   | int     |  
> | view_date   | date    |  
>   
> - There is no primary key (column with unique values) for this table; the table may have duplicate rows.  
> - Each row of this table indicates that some viewer viewed an article (written by some author) on some date.  
> - Note that equal `author_id` and `viewer_id` indicate the same person.  
>   
> **Problem Statement:**  
> Write a solution to find all the authors that viewed at least one of their own articles.  
> Return the result table sorted by `id` in ascending order.  
> The result format is in the following example.
> 
> **Solution:**
> 
> ```sql  
> SELECT DISTINCT author_id AS id  
> FROM Views  
> WHERE author_id = viewer_id  -- Filters rows where the author viewed their own article  
> ORDER BY author_id;  -- Orders the result by id in ascending order  
> ```  
>   
> **Output:**  
>   
> | id |  
> |----|  
> | 4  |  
> | 7  |  
>   
> **Explanation:**  
> - The query selects distinct `author_id` from the `Views` table where the `author_id` is the same as `viewer_id`.  
> - This condition checks if an author has viewed their own article.  
> - The result is sorted in ascending order by `id`.

</details>

<details>
  <summary> 1683. Invalid Tweets</summary> 

> **Table: Tweets**  
>   
> | Column Name | Type    |  
> |-------------|---------|  
> | tweet_id    | int     |  
> | content     | varchar |  
>   
> - `tweet_id` is the primary key (column with unique values) for this table.  
> - This table contains all the tweets in a social media app.  
>   
> **Problem Statement:**  
> Write a solution to find the IDs of the invalid tweets. A tweet is considered invalid if the number of characters used in the content of the tweet is strictly greater than 15.  
> Return the result table in any order.  
> The result format is in the following example.
> 
> **Solution:**
> 
> ```sql  
> SELECT tweet_id  
> FROM Tweets  
> WHERE LENGTH(content) > 15;  -- Filters tweets where the content length is greater than 15 characters  
> ```  
>   
> **Output:**  
>   
> | tweet_id |  
> |----------|  
> | 2        |  
>   
> **Explanation:**  
> - The query selects `tweet_id` from the `Tweets` table where the length of `content` is greater than 15 characters.  
> - This ensures that only the IDs of invalid tweets are returned.

</details>
</details>
















<details>
  <summary><strong>BASIC JOINS</strong></summary>

<details>
  <summary> 1378. Replace Employee ID With The Unique Identifier</summary> 

> **Table: Employees**  
>   
> | Column Name | Type    |  
> |-------------|---------|  
> | id          | int     |  
> | name        | varchar |  
>   
> - `id` is the primary key (column with unique values) for this table.  
> - Each row of this table contains the `id` and the `name` of an employee in a company.  
>   
> **Table: EmployeeUNI**  
>   
> | Column Name | Type    |  
> |-------------|---------|  
> | id          | int     |  
> | unique_id   | int     |  
>   
> - `(id, unique_id)` is the primary key (combination of columns with unique values) for this table.  
> - Each row of this table contains the `id` and the corresponding `unique_id` of an employee in the company.  
>   
> **Problem Statement:**  
> Write a solution to show the `unique_id` of each user. If a user does not have a `unique_id`, show `null`.  
> Return the result table in any order.  
> The result format is in the following example.
> 
> **Solution:**
> 
> ```sql  
> SELECT u.unique_id, e.name  
> FROM Employees AS e  
> LEFT JOIN EmployeeUNI AS u ON u.id = e.id;  -- Joins the tables on employee id and retrieves unique_id; null if not found  
> ```  
>   
> **Output:**  
>   
> | unique_id | name     |  
> |-----------|----------|  
> | null      | Alice    |  
> | null      | Bob      |  
> | 2         | Meir     |  
> | 3         | Winston  |  
> | 1         | Jonathan |  
>   
> **Explanation:**  
> - The query performs a `LEFT JOIN` between the `Employees` and `EmployeeUNI` tables on the `id` column.  
> - It selects the `unique_id` and `name` of each employee. If an employee does not have a `unique_id`, the result is `null`.

</details>

<details>
  <summary>1068. Product Sales Analysis I</summary>  

> **Table: Sales**  
>  
> | Column Name | Type  |  
> |-------------|-------|  
> | sale_id     | int   |  
> | product_id  | int   |  
> | year        | int   |  
> | quantity    | int   |  
> | price       | int   |  
>  
> (sale_id, year) is the primary key (combination of columns with unique values) of this table.  
> product_id is a foreign key (reference column) to Product table.  
> Each row of this table shows a sale on the product product_id in a certain year.  
> Note that the price is per unit.  
>  
> **Table: Product**  
>  
> | Column Name  | Type    |  
> |--------------|---------|  
> | product_id   | int     |  
> | product_name | varchar |  
>  
> product_id is the primary key (column with unique values) of this table.  
> Each row of this table indicates the product name of each product.  
>  
> **Problem Statement:**  
> Write a solution to report the product_name, year, and price for each sale_id in the Sales table.  
> Return the resulting table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT p.product_name, s.year, s.price  
> FROM sales as s  
> LEFT JOIN product as p ON p.product_id = s.product_id;  
> ```  
>  
> **Output:**  
>  
> | product_name | year | price |  
> | ------------ | ---- | ----- |  
> | Nokia        | 2008 | 5000  |  
> | Nokia        | 2009 | 5000  |  
> | Apple        | 2011 | 9000  |  
>  
> **Explanation:**  
> - The query retrieves the product name, the year of the sale, and the price for each sale from the Sales table.  
> - A LEFT JOIN is used to join the Product table with the Sales table on product_id.  
> - The result includes all records from Sales and matches the corresponding product name from Product.
> 
</details>

<details>
  <summary>1581. Customer Who Visited but Did Not Make Any Transactions</summary>  

> **Table: Visits**  
>  
> | Column Name | Type    |  
> |-------------|---------|  
> | visit_id    | int     |  
> | customer_id | int     |  
>  
> visit_id is the column with unique values for this table.  
> This table contains information about the customers who visited the mall.  
>  
> **Table: Transactions**  
>  
> | Column Name    | Type    |  
> |----------------|---------|  
> | transaction_id | int     |  
> | visit_id       | int     |  
> | amount         | int     |  
>  
> transaction_id is the column with unique values for this table.  
> This table contains information about the transactions made during the visit_id.  
>  
> **Problem Statement:**  
> Write a solution to find the IDs of the users who visited without making any transactions and the number of times they made these types of visits.  
> Return the result table sorted in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT v.customer_id, COUNT(v.visit_id) as count_no_trans  
> FROM visits as v  
> LEFT JOIN transactions as t ON v.visit_id = t.visit_id  
> WHERE t.visit_id IS NULL  
> GROUP BY v.customer_id;  
> ```  
>  
> **Output:**  
>  
> | customer_id | count_no_trans |  
> | ----------- | -------------- |  
> | 30          | 1              |  
> | 54          | 2              |  
> | 96          | 1              |  
>  
> **Explanation:**  
> - The query joins the `Visits` table with the `Transactions` table using a LEFT JOIN to keep all visits, even if no transaction was made.  
> - The `WHERE t.visit_id IS NULL` clause filters out any visits that had a transaction.  
> - The result is grouped by customer_id, and the COUNT function calculates how many visits did not result in a transaction.  
> 
</details>


<details>
  <summary>197. Rising Temperature</summary>  

> **Table: Weather**  
>  
> | Column Name   | Type    |  
> |---------------|---------|  
> | id            | int     |  
> | recordDate    | date    |  
> | temperature   | int     |  
>  
> id is the column with unique values for this table.  
> There are no different rows with the same recordDate.  
> This table contains information about the temperature on a certain day.  
>  
> **Problem Statement:**  
> Write a solution to find all dates' `id` with higher temperatures compared to the previous day.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT w1.id  
> FROM weather as w1  
> JOIN weather as w2 ON w1.recordDate = DATEADD(DAY, 1, w2.recordDate)  
> WHERE w1.temperature > w2.temperature;  
> ```  
>  
> **Output:**  
>  
> | id |  
> |----|  
> | 2  |  
> | 4  |  
>  
> **Explanation:**  
> - The query joins the `Weather` table with itself to compare the temperature on each day with the temperature of the previous day.  
> - The `DATEADD(DAY, 1, w2.recordDate)` condition ensures that we are comparing consecutive days.  
> - The `WHERE w1.temperature > w2.temperature` filters out the rows where the temperature of the current day is lower or equal to the previous day, and only selects those where the temperature increased.
>  
</details>


<details>
  <summary>1661. Average Time of Process per Machine</summary> 

> **Table: Activity**  
>   
> | Column Name    | Type    |  
> |----------------|---------|  
> | machine_id     | int     |  
> | process_id     | int     |  
> | activity_type  | enum    |  
> | timestamp      | float   |  
>   
> - `machine_id`: The ID of the machine.  
> - `process_id`: The ID of the process running on the machine.  
> - `activity_type`: An ENUM of either `'start'` or `'end'`, indicating the beginning and end of a process.  
> - `timestamp`: A float representing the time in seconds when the activity occurred.  
> 
> **Problem Statement:**  
> Write a solution to find the average time each machine takes to complete a process.  
> The time for a process is calculated by subtracting the 'start' timestamp from the 'end' timestamp.  
> The result should include the `machine_id` and the average processing time, rounded to 3 decimal places.
> 
> **Solution:**
> 
> ```sql
> SELECT 
>     a.machine_id, 
>     ROUND(AVG(b.timestamp - a.timestamp), 3) AS processing_time
> FROM 
>     Activity AS a
> JOIN 
>     Activity AS b
> ON 
>     a.machine_id = b.machine_id
>     AND a.process_id = b.process_id
>     AND a.activity_type = 'start'
>     AND b.activity_type = 'end'
> GROUP BY 
>     a.machine_id;
> ```
> **Output:**  
> | machine_id | processing_time |
> | ---------- | --------------- |
> | 0          | 0.894           |
> | 1          | 0.995           |
> | 2          | 1.456           |
>
> **Explanation:**
>
> The query joins the Activity table to itself to match the start and end times of each process.
> It then calculates the time difference between the 'start' and 'end' timestamps for each process.
> The result is grouped by machine_id, and the average processing time for each machine is returned.
> The AVG function calculates the average processing time for each machine, and the ROUND function rounds it to 3 decimal places.

</details>

<details>
  <summary>577. Employee Bonus</summary> 

> **Table: Employee**  
>   
> | Column Name | Type    |  
> |-------------|---------|  
> | empId       | int     |  
> | name        | varchar |  
> | supervisor  | int     |  
> | salary      | int     |  
>   
> - `empId` is the primary key of the Employee table.  
> - Each row represents an employee's information, including their `empId`, `name`, `supervisor`, and `salary`.  

> **Table: Bonus**  
>   
> | Column Name | Type    |  
> |-------------|---------|  
> | empId       | int     |  
> | bonus       | int     |  
>   
> - `empId` is the primary key of the Bonus table and a foreign key to `empId` in the Employee table.  
> - Each row represents the `empId` of an employee and their respective `bonus`.  
>   
> **Problem Statement:**  
> Write a query to return the `name` and `bonus` of each employee whose bonus is less than 1000 or is missing (null).  
> Return the result in any order.
> 
> **Solution:**
> 
> ```sql
> SELECT 
>     e.name, 
>     b.bonus 
> FROM 
>     Employee e
> LEFT JOIN 
>     Bonus b ON e.empID = b.empID
> WHERE 
>     b.bonus < 1000 OR b.bonus IS NULL;
> ```
> **Output:**  
> | name | bonus |
> | ---- | ----- |
> | Brad | null  |
> | John | null  |
> | Dan  | 500   |
>
> **Explanation:**  
> The query performs a LEFT JOIN between the Employee and Bonus tables to ensure that all employees are included, even if they have no bonus (null).  
> The WHERE clause filters the employees who either have a bonus less than 1000 or no bonus at all.  
> The result returns the names of employees along with their respective bonus amounts, or null if they have no bonus.  


</details>


<details>
  <summary>1280. Students and Examinations</summary>

> **Table: Students**  
> 
> | Column Name   | Type    |  
> |---------------|---------|  
> | student_id    | int     |  
> | student_name  | varchar |  
> 
> - `student_id` is the primary key (column with unique values) for this table.  
> - Each row of this table contains the ID and the name of one student in the school.  
> 
> **Table: Subjects**  
> 
> | Column Name  | Type    |  
> |--------------|---------|  
> | subject_name | varchar |  
> 
> - `subject_name` is the primary key (column with unique values) for this table.  
> - Each row of this table contains the name of one subject in the school.  
> 
> **Table: Examinations**  
> 
> | Column Name  | Type    |  
> |--------------|---------|  
> | student_id   | int     |  
> | subject_name | varchar |  
> 
> - There is no primary key (column with unique values) for this table. It may contain duplicates.  
> - Each student from the `Students` table takes every course from the `Subjects` table.  
> - Each row of this table indicates that a student with ID `student_id` attended the exam of `subject_name`.  
> 
> **Problem Statement:**  
> Write a solution to find the number of times each student attended each exam.  
> Return the result table ordered by `student_id` and `subject_name`.  
> 
> **Solution:**  
> 
> ```sql
> WITH att AS (
>     SELECT st.student_id, st.student_name, sb.subject_name
>     FROM Students AS st
>     CROSS JOIN Subjects AS sb
> )
> SELECT a.student_id, a.student_name, a.subject_name, COUNT(e.subject_name) AS attended_exams
> FROM att a
> LEFT JOIN Examinations AS e
>     ON a.student_id = e.student_id
>     AND a.subject_name = e.subject_name
> GROUP BY a.student_id, a.subject_name
> ORDER BY a.student_id, a.subject_name;
> ```  
> 
> **Output:**  
> 
> | student_id | student_name | subject_name | attended_exams |  
> |------------|--------------|--------------|----------------|  
> | 1          | Alice        | Math         | 3              |  
> | 1          | Alice        | Physics      | 2              |  
> | 1          | Alice        | Programming  | 1              |  
> | 2          | Bob          | Math         | 1              |  
> | 2          | Bob          | Physics      | 0              |  
> | 2          | Bob          | Programming  | 1              |  
> | 6          | Alex         | Math         | 0              |  
> | 6          | Alex         | Physics      | 0              |  
> | 6          | Alex         | Programming  | 0              |  
> | 13         | John         | Math         | 1              |  
> | 13         | John         | Physics      | 1              |  
> | 13         | John         | Programming  | 1              |
>
> 
> **Explanation:**  
> CROSS JOIN: We perform a CROSS JOIN between the Students and Subjects tables to ensure that each student is paired with every subject, even if they haven't attended any exams for that subject.  
> LEFT JOIN: We then use a LEFT JOIN to link this full list of student-subject combinations to the Examinations table. This ensures that even if a student has not attended an exam for a subject, the student-subject pair will still appear in the results.  
> COUNT: We count how many times each student attended the exam for each subject using COUNT(e.subject_name). If a student did not attend an exam for a particular subject, the count will be 0.  
> GROUP BY and ORDER BY: Finally, we group the results by student_id and subject_name to aggregate the exam attendances, and order the output by these columns for a clear and organized result.  

</details>


<details>
  <summary>570. Managers with at Least 5 Direct Reports</summary>  

> **Table: Employee**  
>  
> | Column Name | Type    |  
> |-------------|---------|  
> | id          | int     |  
> | name        | varchar |  
> | department  | varchar |  
> | managerId   | int     |  
>  
> id is the primary key (column with unique values) for this table.  
> Each row of this table indicates the name of an employee, their department, and the id of their manager.  
> If managerId is null, then the employee does not have a manager.  
> No employee will be the manager of themself.  
>  
> **Problem Statement:**  
> Write a solution to find managers with at least five direct reports.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT m.name  
> FROM Employee as m  
> LEFT JOIN Employee as e   
>     ON m.id = e.managerId  
> GROUP BY e.managerId  
> HAVING COUNT(e.managerId) >= 5;  
> ```  
>  
> **Output:**  
>  
> | name |  
> |------|  
> | John |  
>  
> **Explanation:**  
> - The query joins the `Employee` table with itself to count how many employees each manager (identified by `managerId`) directly supervises.  
> - We group the results by `e.managerId` and use the `HAVING` clause to filter only those managers who have 5 or more direct reports.  
> - The `LEFT JOIN` ensures that managers with no employees reporting to them are still considered, though filtered out by the `HAVING` clause.  
>  
</details>


<details>
  <summary>1934. Confirmation Rate</summary>  

> **Table: Signups**  
>  
> | Column Name | Type     |  
> |-------------|----------|  
> | user_id     | int      |  
> | time_stamp  | datetime |  
>  
> user_id is the column of unique values for this table.  
> Each row contains information about the signup time for the user with ID user_id.  
>  
> **Table: Confirmations**  
>  
> | Column Name | Type     |  
> |-------------|----------|  
> | user_id     | int      |  
> | time_stamp  | datetime |  
> | action      | ENUM     |  
>  
> (user_id, time_stamp) is the primary key (combination of columns with unique values) for this table.  
> user_id is a foreign key (reference column) to the Signups table.  
> action is an ENUM (category) of the type ('confirmed', 'timeout').  
> Each row of this table indicates that the user with ID user_id requested a confirmation message at time_stamp and that confirmation message was either confirmed ('confirmed') or expired without confirming ('timeout').  
>  
> **Problem Statement:**  
> The confirmation rate of a user is the number of 'confirmed' messages divided by the total number of requested confirmation messages. The confirmation rate of a user that did not request any confirmation messages is 0.  
> Round the confirmation rate to two decimal places.  
> Write a solution to find the confirmation rate of each user.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT s.user_id  
>       ,ROUND(SUM(CASE WHEN c.action = 'confirmed' THEN 1 ELSE 0 END)/COUNT(*), 2) as confirmation_rate  
> FROM Signups as s  
> LEFT JOIN Confirmations as c  
>     ON s.user_id = c.user_id  
> GROUP BY s.user_id;  
> ```  
>  
> **Output:**  
>  
> | user_id | confirmation_rate |  
> |---------|-------------------|  
> | 3       | 0                 |  
> | 7       | 1                 |  
> | 2       | 0.5               |  
> | 6       | 0                 |  
>  
> **Explanation:**  
> - The query joins the `Signups` table with the `Confirmations` table to count how many confirmation requests each user made and how many were successfully confirmed.  
> - For each user, we calculate the confirmation rate by dividing the number of 'confirmed' messages by the total number of confirmation messages.  
> - The `LEFT JOIN` ensures that users who did not request any confirmation messages still appear with a confirmation rate of 0.
>  
</details>
</details>





<details>
  <summary><strong>BASIC AGGREGATE FUNCTIONS</strong></summary>
  <details>
  <summary>620. Not Boring Movies</summary>  

> **Table: Cinema**  
>  
> | Column Name | Type     |  
> |-------------|----------|  
> | id          | int      |  
> | movie       | varchar  |  
> | description | varchar  |  
> | rating      | float    |  
>  
> id is the primary key (column with unique values) for this table.  
> Each row contains information about the name of a movie, its genre, and its rating.  
> rating is a 2 decimal places float in the range [0, 10].  
>  
> **Problem Statement:**  
> Write a solution to report the movies with an odd-numbered ID and a description that is not "boring".  
> Return the result table ordered by `rating` in descending order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT *  
> FROM Cinema  
> WHERE id % 2 = 1 AND description != 'boring'  
> ORDER BY rating DESC;  
> ```  
>  
> **Output:**  
>  
> | id | movie      | description | rating |  
> |----|------------|-------------|--------|  
> | 5  | House card | Interesting | 9.1    |  
> | 1  | War        | great 3D    | 8.9    |  
>  
> **Explanation:**  
> - The query selects all movies with an odd-numbered ID using the condition `id % 2 = 1`.  
> - It filters out movies where the `description` is "boring" and orders the results by `rating` in descending order.
>  
</details>


<details>
  <summary>1251. Average Selling Price</summary>  

> **Table: Prices**  
>  
> | Column Name | Type  |  
> |-------------|-------|  
> | product_id  | int   |  
> | start_date  | date  |  
> | end_date    | date  |  
> | price       | int   |  
>  
> (product_id, start_date, end_date) is the primary key (combination of columns with unique values) for this table.  
> Each row of this table indicates the price of the product_id in the period from `start_date` to `end_date`.  
> For each product_id there will be no two overlapping periods.  
>  
> **Table: UnitsSold**  
>  
> | Column Name   | Type |  
> |---------------|------|  
> | product_id    | int  |  
> | purchase_date | date |  
> | units         | int  |  
>  
> This table may contain duplicate rows.  
> Each row of this table indicates the date, units, and product_id of each product sold.  
>  
> **Problem Statement:**  
> Write a solution to find the average selling price for each product. `average_price` should be rounded to 2 decimal places.  
> If a product does not have any sold units, its average selling price is assumed to be 0.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT p.product_id  
>      , IFNULL(ROUND(SUM(p.price * u.units)/SUM(u.units),2),0) as average_price  
> FROM Prices as p  
> LEFT JOIN UnitsSold as u  
>     ON p.product_id = u.product_id  
>     AND u.purchase_date BETWEEN p.start_date AND p.end_date  
> GROUP BY p.product_id;  
> ```  
>  
> **Output:**  
>  
> | product_id | average_price |  
> |------------|---------------|  
> | 1          | 6.96          |  
> | 2          | 16.96         |  
>  
> **Explanation:**  
> - The query calculates the average selling price for each product by multiplying the price by the units sold for the respective date range and dividing by the total number of units sold.  
> - The `IFNULL` ensures that if no units were sold for a product, the `average_price` is set to 0.  
> - The query uses `LEFT JOIN` to ensure that even products without any sold units are included in the result.  
>  
</details>


<details>
  <summary>1075. Project Employees I</summary>  

> **Table: Project**  
>  
> | Column Name | Type |  
> |-------------|------|  
> | project_id  | int  |  
> | employee_id | int  |  
>  
> (project_id, employee_id) is the primary key of this table.  
> employee_id is a foreign key to the Employee table.  
> Each row of this table indicates that the employee with employee_id is working on the project with project_id.  
>  
> **Table: Employee**  
>  
> | Column Name      | Type    |  
> |------------------|---------|  
> | employee_id      | int     |  
> | name             | varchar |  
> | experience_years | int     |  
>  
> employee_id is the primary key of this table. It's guaranteed that experience_years is not NULL.  
> Each row of this table contains information about one employee.  
>  
> **Problem Statement:**  
> Write an SQL query that reports the average experience years of all the employees for each project, rounded to 2 digits.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT p.project_id  
>      , ROUND(AVG(e.experience_years), 2) as average_years  
> FROM Project as p  
> LEFT JOIN Employee as e  
>     ON p.employee_id = e.employee_id  
> GROUP BY p.project_id;  
> ```  
>  
> **Output:**  
>  
> | project_id | average_years |  
> |------------|---------------|  
> | 1          | 2             |  
> | 2          | 2.5           |  
>  
> **Explanation:**  
> - The query joins the `Project` table with the `Employee` table using `employee_id`.  
> - For each project, the query calculates the average experience years of all employees working on that project, rounded to two decimal places.  
> - The `GROUP BY` clause ensures that the results are grouped by `project_id`.  
>  
</details>

<details>
  <summary>1633. Percentage of Users Attended a Contest</summary>  

> **Table: Users**  
>  
> | Column Name | Type    |  
> |-------------|---------|  
> | user_id     | int     |  
> | user_name   | varchar |  
>  
> user_id is the primary key (column with unique values) for this table.  
> Each row of this table contains the name and the id of a user.  
>  
> **Table: Register**  
>  
> | Column Name | Type    |  
> |-------------|---------|  
> | contest_id  | int     |  
> | user_id     | int     |  
>  
> (contest_id, user_id) is the primary key (combination of columns with unique values) for this table.  
> Each row of this table contains the id of a user and the contest they registered into.  
>  
> **Problem Statement:**  
> Write a solution to find the percentage of the users registered in each contest rounded to two decimals.  
> Return the result table ordered by percentage in descending order. In case of a tie, order it by contest_id in ascending order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT contest_id  
>      , ROUND(COUNT(r.user_id)*100 / (SELECT COUNT(*) FROM Users), 2) as percentage  
> FROM Register as r  
> GROUP BY contest_id  
> ORDER BY percentage DESC, contest_id ASC;  
> ```  
>  
> **Output:**  
>  
> | contest_id | percentage |  
> |------------|------------|  
> | 208        | 100        |  
> | 209        | 100        |  
> | 210        | 100        |  
> | 215        | 66.67      |  
> | 207        | 33.33      |  
>  
> **Explanation:**  
> - The query calculates the number of users registered for each contest (`COUNT(r.user_id)`) and divides it by the total number of users (`COUNT(*) FROM Users`) to get the percentage.  
> - `ROUND` is used to round the result to two decimal places.  
> - The `GROUP BY` ensures that the results are grouped by `contest_id`, and the `ORDER BY` clause sorts the result by `percentage` in descending order and `contest_id` in ascending order for ties.  
>  
</details>





</details>














