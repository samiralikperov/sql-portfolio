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
