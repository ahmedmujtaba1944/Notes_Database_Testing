# Notes_Database_Testing
### Knowledge of SQL 
#### Database Operations
- **Create Database**: Initializes a new database.
- **Drop Database**: Permanently removes an existing database.

#### Data Definition Language (DDL) ( This is totally on Schema, database Structure) 
- **Create Table**: Defines a new table with specified columns.
- **Drop Table**: Deletes a table and all its data.
- **Truncate Table**: Removes all rows from a table without deleting the table structure.
- **Alter Table**:
  - **Add**: Introduces a new column to an existing table.
  - **Drop**: Removes a column from a table.
  - **Rename Column**: Changes the name of an existing column.
- **Comments**
- **Create View**
- **Drop View**
#### Data Manipulation Language (DML) ( This is totally on data side)
- **Select**: Retrieves data from one or more tables.
- **Insert Into Table**: Adds new rows to a table.
- **Delete Row From Table**: Removes specific rows from a table based on conditions.
- **Update Row**: Modifies existing data in a table.
- **Merge**
- Call ( For PL-SQL)

#### Data Query Language (DQL)
- **Select**: Fetches data from the database.
  - **Where**: Filters records that meet specified conditions.
  - **Order By ASC|DESC**: Sorts the result set in ascending or descending order.
  - **Distinct**: Ensures unique results by eliminating duplicate rows.
  - **AND, OR, NOT**: Logical operators used to refine queries.
  - **LIKE**: Searches for a specified pattern in a column.
  - **IN**: Matches any value in a list of specified values.
  - **BETWEEN**: Selects values within a given range.
  - **Aliases**: Provides a temporary name to columns or tables.
  - **Select Top**: Limits the number of rows returned.

#### Aggregate Functions
- **Min**: Returns the smallest value.
- **Max**: Returns the largest value.
- **Sum**: Calculates the total of a numeric column.
- **Avg**: Computes the average of a numeric column.
- **Count**: Counts the number of rows.

#### Grouping and Filtering
- **Group By**: Groups rows sharing a property for aggregate functions.
- **Having**: Filters groups based on aggregate criteria.
- **Exists**: Checks for the existence of rows in a subquery.

#### Joins
- **Inner Join**: Returns records with matching values in both tables.
- **Left Join**: Returns all records from the left table and matching records from the right table.
- **Right Join**: Returns all records from the right table and matching records from the left table.
- **Full Join**: Returns all records when there is a match in either table.
- **Self Join**: Joins a table with itself.
- **Union**: Combines the result sets of two or more queries, removing duplicates.

# SQL Interview Prep — for SQA / QA Engineer Role

A study guide covering SQL fundamentals through QA-specific query scenarios. Practice writing these by hand — most interviewers will ask you to write a query on a whiteboard or shared doc, not just explain the theory.

---

## Part 1: Core Concepts (Theory Questions)

1. **What is SQL, and what are its main sub-languages?**
   - DDL (Data Definition Language): `CREATE`, `ALTER`, `DROP`, `TRUNCATE`
   - DML (Data Manipulation Language): `SELECT`, `INSERT`, `UPDATE`, `DELETE`
   - DCL (Data Control Language): `GRANT`, `REVOKE`
   - TCL (Transaction Control Language): `COMMIT`, `ROLLBACK`, `SAVEPOINT`

2. **What is the difference between `WHERE` and `HAVING`?**
   - `WHERE` filters rows *before* grouping/aggregation.
   - `HAVING` filters groups *after* aggregation (used with `GROUP BY`).

3. **What is the difference between `DELETE`, `TRUNCATE`, and `DROP`?**
   - `DELETE` — removes rows one at a time, can use `WHERE`, logged, can be rolled back.
   - `TRUNCATE` — removes all rows at once, resets identity, minimal logging, faster.
   - `DROP` — removes the entire table structure and data.

4. **What is a Primary Key vs a Foreign Key vs a Unique Key?**
   - Primary Key: uniquely identifies each row, cannot be NULL, one per table.
   - Foreign Key: references a Primary Key in another table, enforces referential integrity.
   - Unique Key: ensures all values in a column are distinct, but allows one NULL.

5. **What is a JOIN? Name the types.**
   - `INNER JOIN` — only matching rows in both tables.
   - `LEFT JOIN` — all rows from left table, matched rows from right (NULL if no match).
   - `RIGHT JOIN` — all rows from right table, matched rows from left.
   - `FULL OUTER JOIN` — all rows from both, matched where possible.
   - `SELF JOIN` — table joined with itself.
   - `CROSS JOIN` — Cartesian product of both tables.

6. **What is normalization? Name the normal forms.**
   - Process of organizing data to reduce redundancy.
   - 1NF: atomic columns, no repeating groups.
   - 2NF: 1NF + no partial dependency on a composite key.
   - 3NF: 2NF + no transitive dependency.

7. **What is a subquery? Difference between subquery and JOIN?**
   - A query nested inside another query. JOINs are often more performant for combining columns from multiple tables; subqueries are useful when you need a filtered/aggregated value first.

8. **What is an index? Why does it matter for testing?**
   - A data structure that speeds up row lookup at the cost of extra storage and slower writes. As a QA, you should know that missing indexes can cause slow queries — useful when validating performance-related bugs.

9. **What is a constraint? Name common types.**
   - `NOT NULL`, `UNIQUE`, `PRIMARY KEY`, `FOREIGN KEY`, `CHECK`, `DEFAULT`.

10. **What is the difference between `UNION` and `UNION ALL`?**
    - `UNION` removes duplicate rows (slower, does a distinct sort).
    - `UNION ALL` keeps all rows, including duplicates (faster).

11. **What are aggregate functions?**
    - `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()` — operate on a set of rows and return a single value.

12. **What is a View?**
    - A virtual table based on the result of a `SELECT` query; doesn't store data itself (unless materialized).

13. **What is ACID?**
    - Atomicity, Consistency, Isolation, Durability — properties guaranteeing reliable transaction processing.

14. **What's the difference between `CHAR` and `VARCHAR`?**
    - `CHAR` is fixed-length (padded with spaces). `VARCHAR` is variable-length (stores only what's needed).

15. **What is a stored procedure vs a function?**
    - A stored procedure performs an action and may or may not return a value, can have side effects (INSERT/UPDATE). A function must return a value and is typically used inside a query (e.g., `SELECT myFunction(col)`).

---

## Part 2: Query-Writing Questions

Assume two tables for these examples:

```sql
Employees (emp_id, emp_name, dept_id, salary, manager_id, hire_date)
Departments (dept_id, dept_name)
```

1. **Get all employees earning more than 50000.**
```sql
SELECT * FROM Employees WHERE salary > 50000;
```

2. **Get the second highest salary.**
```sql
SELECT MAX(salary) AS second_highest
FROM Employees
WHERE salary < (SELECT MAX(salary) FROM Employees);
```

3. **Get the Nth highest salary (using window functions).**
```sql
SELECT emp_name, salary
FROM (
  SELECT emp_name, salary,
         DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
  FROM Employees
) ranked
WHERE rnk = 3;  -- change N here
```

4. **Find duplicate rows in a table (e.g., duplicate emp_name).**
```sql
SELECT emp_name, COUNT(*) AS cnt
FROM Employees
GROUP BY emp_name
HAVING COUNT(*) > 1;
```

5. **Delete duplicate rows, keeping only one copy.**
```sql
DELETE e1 FROM Employees e1
INNER JOIN Employees e2
  ON e1.emp_name = e2.emp_name
  AND e1.emp_id > e2.emp_id;
```

6. **List employees along with their department name.**
```sql
SELECT e.emp_name, d.dept_name
FROM Employees e
INNER JOIN Departments d ON e.dept_id = d.dept_id;
```

7. **List departments with no employees.**
```sql
SELECT d.dept_name
FROM Departments d
LEFT JOIN Employees e ON d.dept_id = e.dept_id
WHERE e.emp_id IS NULL;
```

8. **Find employees who earn more than their manager.**
```sql
SELECT e.emp_name
FROM Employees e
JOIN Employees m ON e.manager_id = m.emp_id
WHERE e.salary > m.salary;
```

9. **Count employees per department.**
```sql
SELECT d.dept_name, COUNT(e.emp_id) AS emp_count
FROM Departments d
LEFT JOIN Employees e ON d.dept_id = e.dept_id
GROUP BY d.dept_name;
```

10. **Get the average salary per department, only for departments with more than 5 employees.**
```sql
SELECT dept_id, AVG(salary) AS avg_salary
FROM Employees
GROUP BY dept_id
HAVING COUNT(*) > 5;
```

11. **Find employees hired in the last 30 days.**
```sql
SELECT emp_name, hire_date
FROM Employees
WHERE hire_date >= CURRENT_DATE - INTERVAL '30 days';
```

12. **Get the top 3 highest paid employees per department (window function).**
```sql
SELECT emp_name, dept_id, salary
FROM (
  SELECT emp_name, dept_id, salary,
         ROW_NUMBER() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS rn
  FROM Employees
) t
WHERE rn <= 3;
```

13. **Swap values of two columns without using a third variable.**
```sql
UPDATE Employees
SET salary = manager_id, manager_id = salary;
-- (contrived example just to test UPDATE logic; use a temp column in practice)
```

14. **Find employees whose name starts with 'A' and ends with 'n'.**
```sql
SELECT emp_name FROM Employees
WHERE emp_name LIKE 'A%n';
```

15. **Get running total of salaries ordered by hire date.**
```sql
SELECT emp_name, hire_date, salary,
       SUM(salary) OVER (ORDER BY hire_date) AS running_total
FROM Employees;
```

---

## Part 3: QA-Specific SQL Scenarios

These are the kinds of questions that show up specifically because you're testing, not developing — interviewers want to know you can validate data, not just write queries.

1. **How would you verify that a migration script moved all records correctly?**
```sql
-- Compare row counts between source and target
SELECT COUNT(*) FROM source_table;
SELECT COUNT(*) FROM target_table;

-- Find records in source but missing in target
SELECT s.id FROM source_table s
LEFT JOIN target_table t ON s.id = t.id
WHERE t.id IS NULL;
```

2. **How would you check for orphaned records (broken foreign keys)?**
```sql
SELECT o.order_id
FROM Orders o
LEFT JOIN Customers c ON o.customer_id = c.customer_id
WHERE c.customer_id IS NULL;
```

3. **How would you check for NULLs in a column that should never be NULL?**
```sql
SELECT * FROM Employees WHERE emp_name IS NULL OR salary IS NULL;
```

4. **How would you validate no duplicate primary-key-equivalent data exists (e.g., email should be unique)?**
```sql
SELECT email, COUNT(*)
FROM Users
GROUP BY email
HAVING COUNT(*) > 1;
```

5. **How would you check data consistency between two environments (e.g., staging vs prod)?**
```sql
SELECT * FROM staging.Orders
EXCEPT
SELECT * FROM prod.Orders;
-- (MySQL doesn't support EXCEPT directly — use NOT IN / NOT EXISTS instead)
```

6. **How would you verify that a date field falls within an expected range (data validation)?**
```sql
SELECT * FROM Orders
WHERE order_date < '2020-01-01' OR order_date > CURRENT_DATE;
```

7. **How would you check referential integrity across a whole schema quickly?**
   - Mention: query `information_schema.KEY_COLUMN_USAGE` / `information_schema.TABLE_CONSTRAINTS` to inspect all FK constraints, then spot-check for orphaned rows on each.

8. **How would you test boundary conditions using SQL (e.g., inventory that hits zero)?**
```sql
SELECT * FROM Inventory WHERE stock_qty <= 0;
```

---

## Part 4: Quick-Fire Concepts You Should Be Able to Say Out Loud

- `GROUP BY` requires that every non-aggregated column in the `SELECT` also appears in the `GROUP BY`.
- `NULL` is never equal to anything, including another `NULL` — use `IS NULL` / `IS NOT NULL`, never `= NULL`.
- Order of SQL execution (conceptually): `FROM` → `WHERE` → `GROUP BY` → `HAVING` → `SELECT` → `ORDER BY` → `LIMIT`.
- Order of SQL *writing* (syntax): `SELECT` → `FROM` → `WHERE` → `GROUP BY` → `HAVING` → `ORDER BY` → `LIMIT`.
- `INNER JOIN` vs subquery with `EXISTS`: `EXISTS` stops scanning as soon as a match is found — can be more efficient for existence checks.
- Always know: an index on a column doesn't help if the query wraps that column in a function (e.g., `WHERE YEAR(hire_date) = 2024` won't use an index on `hire_date`).

---

## Interview Tips (Since You're SQA, Not Dev)

- Interviewers often care more about **your ability to validate/verify data** than write the most optimized query. Talk through your logic out loud.
- Be ready to explain **why** you'd write a query a certain way for testing purposes — e.g., "I'd use `LEFT JOIN ... WHERE right.id IS NULL` to catch orphaned records because that's a common defect in migration testing."
- If you're rusty on syntax, it's fine to say "let me think through the logic first" — structuring your approach out loud is often what's being evaluated.
- Practice actually typing these out, not just reading them — muscle memory matters under interview pressure.


