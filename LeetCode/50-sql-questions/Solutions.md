<details>
  <summary> 1757. Recyclable and Low Fat Products</summary> 

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
  <summary>280. Students and Examinations</summary>

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

