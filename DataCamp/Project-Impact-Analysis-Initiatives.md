## PROJECT: Impact Analysis of GT NGO Initiatives

### **Situation:**
GT NGO has been working tirelessly to drive positive change through education, healthcare, and sustainable development initiatives worldwide. However, the NGO is facing challenges in optimizing its funding allocation and measuring the impact of its various assignments. The management team wants to ensure that their resources are being used effectively to achieve maximum impact.

### **Task:**
As a data analyst, my responsibility was to analyze donation and assignment data to identify which initiatives received the most funding and which assignments had the highest impact across different regions. The goal was to help the management team make informed decisions on where to allocate future resources.

### **Action:**
To address the problem, I created SQL queries to:
Calculate the top five assignments based on the highest total donation amounts.
Identify which assignments had the greatest impact in each region by ranking the assignments according to their impact score.
Use window functions and nested queries to accurately group and sort the data by region, donation count, and impact score, providing meaningful insights to the NGO.

### **Result:**
The analysis successfully highlighted the top-funded assignments and those with the highest regional impact. This allowed the GT NGO to focus its future efforts on the areas where their initiatives had the most positive outcomes, ensuring more efficient use of their resources.



---

### **SQL Queries:**

#### **Highest Donation Assignments:**

```sql
WITH donation_details AS (
    SELECT
        d.assignment_id,
        ROUND(SUM(d.amount), 2) AS rounded_total_donation_amount,
        dn.donor_type
    FROM
        donations d
    JOIN donors dn ON d.donor_id = dn.donor_id
    GROUP BY
        d.assignment_id, dn.donor_type
)
SELECT
    a.assignment_name,
    a.region,
    dd.rounded_total_donation_amount,
    dd.donor_type
FROM
    assignments a
JOIN
    donation_details dd ON a.assignment_id = dd.assignment_id
ORDER BY
    dd.rounded_total_donation_amount DESC
LIMIT 5;
```

#### **Output:**

| assignment_name  | region | rounded_total_donation_amount | donor_type  |
|------------------|--------|-------------------------------|-------------|
| Assignment_3033  | East   | 3840.66                        | Individual  |
| Assignment_300   | West   | 3133.98                        | Organization|
| Assignment_4114  | North  | 2778.57                        | Organization|
| Assignment_1765  | West   | 2626.98                        | Organization|
| Assignment_268   | East   | 2488.69                        | Individual  |

---

#### **Highest Donation Assignments:**
```sql
WITH donation_counts AS (
    SELECT
        assignment_id,
        COUNT(donation_id) AS num_total_donations
    FROM
        donations
    GROUP BY
        assignment_id
),
ranked_assignments AS (
    SELECT
        a.assignment_name,
        a.region,
        a.impact_score,
        dc.num_total_donations,
        ROW_NUMBER() OVER (PARTITION BY a.region ORDER BY a.impact_score DESC) AS rank_in_region
    FROM
        assignments a
    JOIN
        donation_counts dc ON a.assignment_id = dc.assignment_id
    WHERE
        dc.num_total_donations > 0
)
SELECT
    assignment_name,
    region,
    impact_score,
    num_total_donations
FROM
    ranked_assignments
WHERE
    rank_in_region = 1
ORDER BY
    region ASC;
```

#### **Output:**
| assignment_name  | region | impact_score | num_total_donations |
|------------------|--------|--------------|---------------------|
| Assignment_316   | East   | 10           | 2                   |
| Assignment_2253  | North  | 9.99         | 1                   |
| Assignment_3547  | South  | 10           | 1                   |
| Assignment_2794  | West   | 9.99         | 2                   |

