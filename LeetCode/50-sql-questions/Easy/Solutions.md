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

> **Solution:**

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

> **Solution:**

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

> **Solution:**

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

> **Solution:**

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

> **Solution:**

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

> **Solution:**

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

