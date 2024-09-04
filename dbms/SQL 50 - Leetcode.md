## Select

1. Recyclable and Low Fat Products(https://leetcode.com/problems/recyclable-and-low-fat-products/)
```sql
SELECT product_id FROM Products WHERE low_fats = 'Y' AND recyclable = 'Y'
```

The keyword `SELECT` is used to specify the columns that we want to retrieve from the table `Products`. In this scenario, we want to retrieve the `product_id` column.

The keyword `WHERE` is used to filter the rows in the table `Products` based on specific conditions, which the `low_fats` column has the value "Y" (indicating low-fat products) and the `recyclable` column has the value "Y" (indicating recyclable products). We use the logical operator `AND` to combine both conditions, ensuring that the final result includes only product IDs for products that are both low fat and recyclable.

---
2. Find Customer Referee(https://leetcode.com/problems/find-customer-referee/)
```sql
select name from Customer where referee_id != 2 or referee_id is null
```
- Approach: Using `<>`(`!=`) and `IS NULL`

==**Intuition**==
Some people come out the following solution by intuition.
```sql
SELECT name FROM customer WHERE referee_Id <> 2;
```
However, this query will only return one result:Zack although there are 4 customers not referred by Jane (including Jane herself). All the customers who were referred by nobody at all (`NULL` value in the referee_id column) don’t show up. But why?

**Algorithm**
MySQL uses three-valued logic -- TRUE, FALSE and UNKNOWN. Anything compared to NULL evaluates to the third value: UNKNOWN. That “anything” includes NULL itself! That’s why MySQL provides the `IS NULL` and `IS NOT NULL` operators to specifically check for NULL.

Thus, one more condition 'referee_id IS NULL' should be added to the WHERE clause as below.

```sql
SELECT name FROM customer WHERE referee_id <> 2 OR referee_id IS NULL;
```

or

```sql
SELECT name FROM customer WHERE referee_id != 2 OR referee_id IS NULL;
```

==**Tips**==
The following solution is also wrong for the same reason as mentioned above. The key is to always use `IS NULL` or `IS NOT NULL` operators to specifically check for NULL value.

```sql
SELECT name FROM customer WHERE referee_id = NULL OR referee_id <> 2;
```

---
3.  Big Countries(https://leetcode.com/problems/big-countries/)

```sql
select name, population, area from world where area >= 3000000 OR population >= 25000000;
```

- Approach: Filtering rows using `WHERE`

==Algorithm==
To determine whether a country is considered `big`, there are two conditions to verify, as stated in the description:

- The country must have an area of at least three million square kilometers, denoted as `area >= 3,000,000`.
    
- The population of the country should be a minimum of twenty-five million, expressed as `population >= 25,000,000`.

```sql
SELECT 
    * 
FROM 
    world 
WHERE 
    area >= 3000000 
    OR population >= 25000000
```

Noting that we need to return three columns according to the requirements of the problem. Thus the next step is selecting the three required columns with the relative order as: `name`, `population`, and `area`. The complete answer is as follows.

```sql
SELECT
    name, population, area
FROM
    world
WHERE
    area >= 3000000 OR population >= 25000000
;
```

---

4. Article Views I(https://leetcode.com/problems/article-views-i/)
```sql
select distinct author_id as id from Views where author_id = viewer_id

order by id;
```

distinct -> to avoid duplicates
order by -> to sort

---

5. Invalid Tweets (https://leetcode.com/problems/invalid-tweets/)
```sql
select tweet_id from Tweets where length(content) > 15
```

OR

```sql
select tweet_id from Tweets where char_length(content) > 15
```

`LENGTH()` returns the length of the string measured in bytes. `CHAR_LENGTH()` returns the length of the string measured in characters.

- Extended Problem Statement:

If the problem statement is modified such that a tweet is considered invalid if it contains more than 2 words, the following solution can be used:

- Modified Solution:

```sql
SELECT tweet_id
FROM Tweets
WHERE LENGTH(content) - LENGTH(REPLACE(content, ' ', '')) + 1 > 2;
```

- LENGTH(content): This returns the total length of the content
- REPLACE(content, ' ', ''): This removes all spaces from the content.
- LENGTH(content) - LENGTH(REPLACE(content, ' ', '')): This calculates the number of spaces in the content
- Adding 1 to the number of spaces gives the number of words.
- The condition LENGTH(content) - LENGTH(REPLACE(content, ' ', '')) + 1 > 2 checks if the number of words is greater than 2.

---
---
## Joins
6. Replace Employee ID With The Unique Identifier (https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/)

```sql
select EmployeeUNI.unique_id, Employees.name
from Employees
LEFT JOIN EmployeeUNI on Employees.id = EmployeeUNI.id
```

OR

```sql
SELECT EmployeeUNI.unique_id, Employees.name 
FROM EmployeeUNI 
RIGHT JOIN Employees ON Employees.id = EmployeeUNI.id
```

- Intuition

The problem asks us to replace the employee ID with the unique identifier for each employee. We need to retrieve the unique identifier and the name for each employee from the respective tables.

- Approach

1. Imagine we have two tables: one for the employees and one for their unique identifiers.
2. We want to combine the information from both tables to get the unique identifier and the name for each employee.
3. To do this, we use a left join, which means we take all the employees from the first table (Employees) and match them with their unique identifiers from the second table (EmployeeUNI) based on their IDs.
4. By performing this left join, we make sure that all the employees from the first table are included in the result, even if they don't have a matching unique identifier in the second table.
5. The result of our query will be a combination of the unique identifier and the name for each employee.

- **Complexity**
- Time complexity:  
    The time complexity of the query depends on the number of rows in the Employees table and the EmployeeUNI table, as well as the efficiency of the join operation. Assuming the join operation has been appropriately indexed, the time complexity is generally O(n+m), where n is the number of rows in the Employees table and m is the number of rows in the EmployeeUNI table.
    
- Space complexity:  
    The space complexity depends on the result set produced by the query. The space required to store the result set will depend on the number of matching rows between the Employees table and the EmployeeUNI table. If the result set is significantly smaller than the original tables, the space complexity can be considered relatively low.

```sql
/* a solution using aliases and USING */
SELECT eu.unique_id AS unique_id, e.name
FROM Employees e
LEFT JOIN EmployeeUNI eu USING(id)
```
- When using `USING(column_name)`, you don't need to qualify the column with table aliases since it's assumed that the column exists in both tables and is the same.
- It simplifies the syntax when the column names are identical in both tables.
- The result set will only include one column named `id` (or the specified column), avoiding duplication.

In SQL, `NULL` represents a missing or undefined value. It is used to indicate that a data value does not exist in the database. 
- `NULL` indicates the absence of a value. It is not the same as an empty string (`''`), zero (`0`), or any other default value.
- `NULL` is used to represent unknown or missing data.
- When performing a `LEFT JOIN`, if there is no matching row in the right table, the columns from the right table will be filled with `NULL` for those rows that don't have a match.
- If an employee in the `Employees` table does not have a corresponding entry in the `EmployeeUNI` table, the `unique_id` from `EmployeeUNI` will be `NULL`.
- Direct comparisons with `NULL` using operators like `=`, `!=`, `<`, or `>` do not work as expected. For instance, `NULL = NULL` returns `FALSE`.
- Instead, use the `IS NULL` or `IS NOT NULL` operators to check for `NULL` values:
```sql
SELECT * FROM Employees WHERE unique_id IS NULL;
SELECT * FROM Employees WHERE unique_id IS NOT NULL;
```

---

7. Product Sales Analysis I (https://leetcode.com/problems/product-sales-analysis-i/)
```sql
select Product.product_name, Sales.year, Sales.price
from Sales
RIGHT JOIN Product ON Product.product_id = Sales.product_id
where Product.product_id = Sales.product_id
```
 
 OR
 
 ```sql
select Product.product_name , Sales.year, Sales.price from Product , Sales
where Product.product_id=Sales.product_id ;
```

---

8. Customer Who Visited but Did Not Make Any Transactions (https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/)

```sql
select Visits.customer_id, COUNT(*) as count_no_trans from Transactions
RIGHT JOIN Visits ON Visits.visit_id = Transactions.visit_id
where Transactions.transaction_id IS NULL
GROUP by customer_id
```

OR 

```sql
select Visits.customer_id, COUNT(Visits.customer_id) as count_no_trans from Transactions
RIGHT JOIN Visits ON Visits.visit_id = Transactions.visit_id
where Transactions.transaction_id IS NULL
GROUP by customer_id
```

The `GROUP BY` statement in SQL is used to arrange identical data into groups. This is particularly useful when combined with aggregate functions like `COUNT()`, `SUM()`, `AVG()`, `MAX()`, and `MIN()`, which perform calculations on each group of data. 
The basic syntax for the `GROUP BY` statement is:

```sql
SELECT column_name(s), aggregate_function(column_name)
FROM table_name
WHERE condition
GROUP BY column_name(s);
```

---

9. Rising Temperature (https://leetcode.com/problems/rising-temperature/)
```sql
select w1.id
from weather w1
join weather w2
ON Date_Sub(w1.recordDate, INTERVAL 1 DAY) = w2.recordDate
where w1.temperature > w2.temperature
```


- **Self-Join**: `JOIN Weather w2 ON DATE_SUB(w1.recordDate, INTERVAL 1 DAY) = w2.recordDate`
    - This joins the `Weather` table with itself. For each row in `w1`, it finds the row in `w2` where `recordDate` is one day before `w1.recordDate`.
- **Condition**: `WHERE w1.temperature > w2.temperature`
    - This filters the joined rows to retain only those where the temperature of `w1` is higher than the temperature of `w2`.
- The `INTERVAL` keyword in SQL is used to perform date and time arithmetic. When we say `INTERVAL 1 DAY`, we are specifying a time interval of one day. This can be used in combination with date functions to add or subtract a certain amount of time to or from a date.

OR

```sql
select w1.id
from weather w1
join weather w2
ON DateDiff(w1.recordDate, w2.recordDate) = 1
where w1.temperature > w2.temperature
```

OR

```sql
select w1.id
from weather w1
join weather w2
ON Date_Add(w2.recordDate, INTERVAL 1 DAY) = w1.recordDate
where w1.temperature > w2.temperature
```

---
10. Average Time of Process per Machine (https://leetcode.com/problems/average-time-of-process-per-machine/)

```sql

SELECT a1.machine_id, ROUND(AVG(a2.timestamp - a1.timestamp), 3) AS processing_time
FROM Activity a1
JOIN Activity a2
ON a1.machine_id = a2.machine_id
AND a1.process_id = a2.process_id
AND a1.activity_type = 'start'
AND a2.activity_type = 'end'
GROUP BY a1.machine_id;
```

- Step-by-Step Explanation

1. **Table Aliases and Self-Join**:
    
    - The query uses table aliases `a1` and `a2` to refer to the `Activity` table twice. This is known as a self-join.
2. **Join Condition**:
    
    - `a1` and `a2` are joined on three conditions:
        - `a1.machine_id = a2.machine_id`: Ensures we are comparing records from the same machine.
        - `a1.process_id = a2.process_id`: Ensures we are comparing records of the same process.
        - `a1.activity_type = 'start'` and `a2.activity_type = 'end'`: Ensures that `a1` refers to the 'start' activity and `a2` refers to the 'end' activity.
3. **Timestamp Difference**:
    
    - `a2.timestamp - a1.timestamp`: Calculates the time difference between the 'end' and 'start' timestamps for each process.
4. **Average and Round**:
    
    - `AVG(a2.timestamp - a1.timestamp)`: Calculates the average processing time for each machine.
    - `ROUND(..., 3)`: Rounds the average processing time to 3 decimal places.
5. **Group By**:
    
    - `GROUP BY a1.machine_id`: Groups the results by `machine_id` to calculate the average processing time for each machine.


---

11. Employee Bonus (https://leetcode.com/problems/employee-bonus/)
```sql
SELECT Employee.name,Bonus.bonus FROM Employee
LEFT JOIN Bonus ON Employee.empID = Bonus.empID
WHERE bonus < 1000 OR Bonus IS NULL ;
```

---

12. Students and Examinations (https://leetcode.com/problems/students-and-examinations/)
```sql
SELECT s.student_id, s.student_name, sub.subject_name, COUNT(e.student_id) AS attended_exams
FROM Students s
CROSS JOIN Subjects sub
LEFT JOIN Examinations e ON s.student_id = e.student_id AND sub.subject_name = e.subject_name
GROUP BY s.student_id, s.student_name, sub.subject_name
ORDER BY s.student_id, sub.subject_name;
```

---

13. Managers with at Least 5 Direct Reports(https://leetcode.com/problems/managers-with-at-least-5-direct-reports/)

```sql
SELECT a.name
FROM Employee a
JOIN Employee b ON a.id = b.managerId
GROUP BY b.managerId
HAVING COUNT(*) >= 5
```

---
14. Confirmation Rate


---
---
## Basic Aggregate Functions

15. Not Boring Movies (https://leetcode.com/problems/not-boring-movies/)
```sql
SELECT *
FROM Cinema
WHERE
  MOD(id, 2) = 1
  AND description != 'boring'
ORDER BY rating DESC;
```

OR

```sql
SELECT *
FROM Cinema
WHERE
  id%2 = 1
  AND description <> 'boring'
ORDER BY rating DESC;
```

**SELECT FROM Cinema:** This selects all columns (*) from the Cinema table. It retrieves all rows and all columns for those rows that meet the conditions specified in the WHERE clause.

**WHERE MOD(id, 2) = 1:** This condition filters rows where the id column satisfies the condition MOD(id, 2) = 1. This condition checks if the remainder when id is divided by 2 equals 1. In simpler terms, it selects rows where the id is odd. So, it will select rows with ids like 1, 3, 5, etc.

**AND description != 'boring':** This additional condition filters out rows where the description column is 'boring'. It selects rows where the description is anything other than 'boring'.

**ORDER BY rating DESC:** This orders the selected rows by the rating column in descending order (DESC), meaning the rows with the highest rating values will appear first.

---

16. Average Selling Price (https://leetcode.com/problems/average-selling-price/)

```sql
SELECT p.product_id, IFNULL(ROUND(SUM(units*price)/SUM(units),2),0) AS average_price
FROM Prices p LEFT JOIN UnitsSold u
ON p.product_id = u.product_id AND
u.purchase_date BETWEEN start_date AND end_date
group by product_id
```

---
