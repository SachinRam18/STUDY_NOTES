# SQL — Interview Notes

> Examples use common SQL syntax; date functions, pagination, and some DDL behavior vary by database.

## Contents

- [SQL basics](#section-1--basics)
- [Joins](#section-2--joins)
- [Subqueries and set operators](#section-3--subqueries-and-set-operators)
- [Window functions](#section-4--window-functions)
- [Data modification](#section-5--data-modification)
- [Table management](#section-6--table-management)
- [Common patterns and performance](#section-7--common-patterns-and-techniques)
- [Key concepts](#section-8--key-concepts)
- [Quick references](#quick-reference--sql-clauses-order)
- [Interview query practice](#sql-interview-questions)

---

# SECTION 1 — Basics

---

## 1.1 What is SQL?

SQL (Structured Query Language) is a standard language for managing and querying relational databases. It allows analysts to retrieve, filter, aggregate, update, and manage data.

---

## 1.2 SELECT Queries

```sql
-- All columns
SELECT * FROM sales;

-- Distinct values
SELECT DISTINCT product_category FROM sales;

-- Top N records
SELECT amount FROM sales ORDER BY amount DESC LIMIT 5;

-- Last N records
SELECT * FROM sales ORDER BY sale_date DESC LIMIT 10;
```

---

## 1.3 Filtering — WHERE

```sql
SELECT * FROM sales WHERE amount > 1000;
```

- `WHERE` filters **rows before** aggregation.
- Use `AND`, `OR`, `NOT`, `IN`, `BETWEEN`, `LIKE` for complex conditions.

---

## 1.4 Sorting — ORDER BY

```sql
SELECT * FROM sales ORDER BY amount DESC;   -- descending
SELECT * FROM sales ORDER BY amount ASC;    -- ascending (default)
```

---

## 1.5 Aggregation Functions

| Function | Purpose |
|----------|---------|
| `COUNT(*)` | Count rows |
| `SUM(col)` | Total sum |
| `AVG(col)` | Average |
| `MIN(col)` | Minimum value |
| `MAX(col)` | Maximum value |

```sql
SELECT COUNT(*) FROM sales WHERE product_category = 'Electronics';
SELECT AVG(amount) FROM sales;
SELECT MIN(amount) AS MinAmount, MAX(amount) AS MaxAmount FROM sales;
```

---

## 1.6 GROUP BY

Groups rows with the same values so aggregation functions can be applied per group.

```sql
SELECT product_category, SUM(amount)
FROM sales
GROUP BY product_category;
```

---

## 1.7 HAVING

Filters **groups after** aggregation. Use when you need a condition on an aggregate.

```sql
SELECT product_category, SUM(amount) AS total_amount
FROM sales
GROUP BY product_category
HAVING SUM(amount) > 5000;
```

**WHERE vs HAVING:**

| | WHERE | HAVING |
|---|-------|--------|
| When | Before aggregation | After aggregation |
| Works on | Individual rows | Groups |

---

## 1.8 CASE Statement

Conditional logic inside a query.

```sql
SELECT product_name,
       CASE
           WHEN amount > 1000 THEN 'High'
           ELSE 'Low'
       END AS sales_category
FROM sales;
```

---

## 1.9 NULL Handling

```sql
-- Check for NULL
SELECT * FROM sales WHERE discount IS NULL;
SELECT * FROM sales WHERE discount IS NOT NULL;

-- Replace NULL with default
SELECT COALESCE(discount, 0) FROM sales;
```

`COALESCE(a, b, c)` returns the **first non-NULL** value from its arguments.

---

## 1.10 LIMIT

Restricts the number of rows returned.

```sql
SELECT * FROM sales LIMIT 10;
```

---

# SECTION 2 — Joins

---

## 2.1 INNER JOIN

Returns only rows with a match in **both** tables.

```sql
SELECT s.product_name, p.price
FROM sales s
INNER JOIN products p ON s.product_id = p.product_id;
```

---

## 2.2 LEFT JOIN

Returns **all rows from the left** table + matching rows from the right. Unmatched right rows → NULL.

```sql
SELECT s.product_name, p.price
FROM sales s
LEFT JOIN products p ON s.product_id = p.product_id;
```

---

## 2.3 RIGHT JOIN

Returns all rows from the right table + matching rows from the left.

---

## 2.4 CROSS JOIN

Returns the **Cartesian product** — every row from table A combined with every row from table B.

```sql
SELECT * FROM products CROSS JOIN categories;
-- If products has 10 rows and categories has 5 → 50 rows returned
```

---

## 2.5 Self Join

A table joined with itself. Useful for hierarchical data (e.g., employees and managers).

```sql
SELECT e1.employee_name AS Employee,
       e2.employee_name AS Manager
FROM employees e1
LEFT JOIN employees e2 ON e1.manager_id = e2.employee_id;
```

---

## 2.6 Multi-table JOIN

Chain JOIN operations for more than two tables.

```sql
SELECT orders.order_id, customers.customer_name, products.product_name
FROM orders
JOIN customers ON orders.customer_id = customers.customer_id
JOIN products  ON orders.product_id  = products.product_id;
```

---

## 2.7 JOIN to Include Non-matching Rows

Use LEFT JOIN or RIGHT JOIN.

```sql
SELECT employees.name, departments.department_name
FROM employees
LEFT JOIN departments ON employees.department_id = departments.department_id;
-- Employees with no department will have NULL for department_name
```

---

# SECTION 3 — Subqueries and Set Operators

---

## 3.1 Subquery in WHERE

A query nested inside another query.

```sql
-- Products with sales above average
SELECT product_name
FROM sales
WHERE amount > (SELECT AVG(amount) FROM sales);
```

---

## 3.2 EXISTS

Checks if a subquery returns any rows.

```sql
SELECT product_name
FROM sales
WHERE EXISTS (
    SELECT * FROM returns
    WHERE returns.product_id = sales.product_id
);
```

---

## 3.3 UNION vs UNION ALL

```sql
-- UNION: removes duplicates
SELECT product_name FROM sales
UNION
SELECT product_name FROM returns;

-- UNION ALL: keeps duplicates (faster)
SELECT product_name FROM sales
UNION ALL
SELECT product_name FROM returns;
```

---

# SECTION 4 — Window Functions

---

## 4.1 What are Window Functions?

Window functions perform calculations across a set of rows **related to the current row**, without collapsing rows like `GROUP BY` does.

Syntax: `FUNCTION() OVER (PARTITION BY ... ORDER BY ...)`

---

## 4.2 RANK() and DENSE_RANK()

| | `RANK()` | `DENSE_RANK()` |
|---|----------|----------------|
| Ties | Same rank, next rank **skipped** | Same rank, next rank **continues** |
| Example | 1, 2, 2, **4** | 1, 2, 2, **3** |

```sql
SELECT product_name, amount,
       RANK()       OVER (ORDER BY amount DESC) AS rank,
       DENSE_RANK() OVER (ORDER BY amount DESC) AS dense_rank
FROM sales;
```

---

## 4.3 ROW_NUMBER()

Assigns a unique sequential number to each row within a partition.

```sql
SELECT product_name, amount,
       ROW_NUMBER() OVER (ORDER BY amount DESC) AS row_num
FROM sales;
```

---

## 4.4 Running Total

```sql
SELECT order_date, amount,
       SUM(amount) OVER (ORDER BY order_date) AS running_total
FROM orders;
```

---

## 4.5 Percentage of Total

```sql
SELECT product_name,
       SUM(amount) AS total_sales,
       (SUM(amount) / SUM(SUM(amount)) OVER ()) * 100 AS percentage
FROM sales
GROUP BY product_name;
```

---

## 4.6 Median (using ROW_NUMBER + CTE)

```sql
WITH OrderedSales AS (
    SELECT amount,
           ROW_NUMBER() OVER (ORDER BY amount) AS rn,
           COUNT(*) OVER () AS total_count
    FROM sales
)
SELECT AVG(amount) AS median
FROM OrderedSales
WHERE rn IN ((total_count + 1) / 2, (total_count + 2) / 2);
```

---

# SECTION 5 — Data Modification

---

## 5.1 UPDATE

```sql
UPDATE sales
SET amount = amount * 1.1
WHERE product_name = 'Laptop';
```

---

## 5.2 DELETE vs TRUNCATE

| | `DELETE` | `TRUNCATE` |
|---|----------|-----------|
| Removes | Rows matching condition | All rows |
| WHERE clause | ✅ | ❌ |
| Rollback | Often transactional; depends on database | Behavior depends on database and transaction |
| Slower/Faster | Slower | Faster |

```sql
DELETE FROM sales WHERE amount < 500;

TRUNCATE TABLE sales;
```

---

## 5.3 INSERT

```sql
-- Single row
INSERT INTO sales (product_name, amount) VALUES ('Laptop', 1200);

-- Multiple rows
INSERT INTO sales (product_name, amount)
VALUES ('Laptop', 1200), ('Smartphone', 800);

-- Bulk load (MySQL)
LOAD DATA INFILE 'file.csv' INTO TABLE sales FIELDS TERMINATED BY ',';
```

---

# SECTION 6 — Table Management

---

## 6.1 ALTER TABLE

Modify existing table structure.

```sql
ALTER TABLE sales ADD COLUMN discount DECIMAL(10, 2);
ALTER TABLE sales DROP COLUMN discount;
ALTER TABLE sales RENAME COLUMN old_name TO new_name;
```

---

## 6.2 DROP TABLE

Deletes the entire table and its data.

```sql
DROP TABLE old_sales;
```

---

## 6.3 TEMPORARY TABLE

Exists only for the duration of the session.

```sql
CREATE TEMPORARY TABLE temp_sales AS
SELECT * FROM sales WHERE amount > 1000;
```

---

## 6.4 VIEW

A virtual table based on a saved query. Simplifies complex queries and can restrict access.

```sql
CREATE VIEW high_value_sales AS
SELECT * FROM sales WHERE amount > 1000;

-- Query the view like a table
SELECT * FROM high_value_sales;
```

---

## 6.5 INDEX

Speeds up data retrieval by creating a lookup structure on one or more columns.

```sql
CREATE INDEX idx_product_name ON sales(product_name);
```

- Good for columns used in `WHERE`, `JOIN`, `ORDER BY`
- Slows down `INSERT`/`UPDATE`/`DELETE` (index must be updated)

---

# SECTION 7 — Common Patterns and Techniques

---

## 7.1 Find Duplicates

```sql
SELECT product_name, COUNT(*)
FROM sales
GROUP BY product_name
HAVING COUNT(*) > 1;
```

---

## 7.2 Orders Per Customer

```sql
SELECT customer_id, COUNT(order_id)
FROM orders
GROUP BY customer_id;
```

---

## 7.3 Date Functions

```sql
-- Difference between two dates
SELECT DATEDIFF(day, start_date, end_date) AS date_difference
FROM projects;

-- Extract part of a date
SELECT DATEPART(year, sale_date) AS sale_year FROM sales;
SELECT YEAR(sale_date), MONTH(sale_date) FROM sales;  -- MySQL syntax
```

---

## 7.4 Query Performance — EXPLAIN

Shows execution plan: how the query is run, which indexes are used, order of operations.

```sql
EXPLAIN SELECT * FROM sales WHERE amount > 1000;
```

---

# SECTION 8 — Key Concepts

---

## 8.1 Normalization

Organizing data to reduce redundancy and improve integrity.

- **1NF** — atomic values, no repeating groups
- **2NF** — 1NF + no partial dependency on composite key
- **3NF** — 2NF + no transitive dependency

---

## 8.2 CHAR vs VARCHAR

| | `CHAR(n)` | `VARCHAR(n)` |
|---|-----------|--------------|
| Length | Fixed (always n chars) | Variable (only actual chars) |
| Padding | Pads with spaces | No padding |
| Speed | Faster for fixed-size data | More space-efficient |
| Use case | Codes, fixed IDs | Names, descriptions |

---

## 8.3 Performance Optimization Tips

- Use indexes on frequently filtered/joined columns
- Avoid `SELECT *` — select only needed columns
- Use `EXISTS` for existence checks; compare query plans instead of assuming it is always faster than `IN`
- Avoid functions on indexed columns in `WHERE` (prevents index use)
- Use `LIMIT` to reduce result size
- Analyze with `EXPLAIN` to identify full table scans

---

# Quick Reference — SQL Clauses Order

```sql
SELECT   columns
FROM     table
JOIN     other_table ON condition
WHERE    row_filter
GROUP BY columns
HAVING   group_filter
ORDER BY columns
LIMIT    n;
```

**Execution order (not writing order):**
`FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT`

---

# Quick Reference — Window Functions

```sql
FUNCTION() OVER (
    PARTITION BY col   -- optional: split into groups
    ORDER BY col       -- optional: row ordering within group
)
```

| Function | Use |
|----------|-----|
| `ROW_NUMBER()` | Unique row number |
| `RANK()` | Rank with gaps on ties |
| `DENSE_RANK()` | Rank without gaps |
| `SUM() OVER` | Running total |
| `AVG() OVER` | Moving average |
| `LAG(col, n)` | Value from n rows before |
| `LEAD(col, n)` | Value from n rows ahead |

---

# Quick Reference — JOIN Types

```
INNER JOIN  →  A ∩ B  (matching rows only)
LEFT JOIN   →  All of A + matching B
RIGHT JOIN  →  All of B + matching A
FULL JOIN   →  All of A + All of B (with NULLs)
CROSS JOIN  →  A × B  (every combination)
SELF JOIN   →  Table joined with itself
```

## SQL Interview Questions

Assume these tables unless a question says otherwise:

- `Employee(emp_id, name, salary, department_id, manager_id, joining_date)`
- `Department(department_id, department_name)`

`LIMIT` examples use MySQL/PostgreSQL/SQLite-style syntax; pagination varies by database.

### Highest and Nth-highest salaries

#### 1. Find the highest salary

```sql
SELECT MAX(salary) AS highest_salary
FROM Employee;
```

#### 2. Find the second-highest distinct salary

```sql
SELECT MAX(salary) AS second_highest_salary
FROM Employee
WHERE salary < (SELECT MAX(salary) FROM Employee);
```

Returns `NULL` if there is no second distinct salary.

#### 3. Find the third-highest distinct salary

```sql
WITH RankedSalaries AS (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS salary_rank
    FROM Employee
)
SELECT DISTINCT salary
FROM RankedSalaries
WHERE salary_rank = 3;
```

#### 4. Find the Nth-highest distinct salary ⭐

Bind `:n` to the requested rank using the parameter syntax supported by your database driver.

```sql
WITH RankedSalaries AS (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS salary_rank
    FROM Employee
)
SELECT DISTINCT salary
FROM RankedSalaries
WHERE salary_rank = :n;
```

#### 5. Find all employees with the highest salary

```sql
SELECT e.*
FROM Employee AS e
WHERE e.salary = (SELECT MAX(salary) FROM Employee);
```

Returns every employee tied for the highest salary.

#### 6. Find all employees with the second-highest distinct salary

```sql
WITH RankedEmployees AS (
    SELECT e.*, DENSE_RANK() OVER (ORDER BY salary DESC) AS salary_rank
    FROM Employee AS e
)
SELECT *
FROM RankedEmployees
WHERE salary_rank = 2;
```

### Duplicates and aggregates

#### 7. Find duplicate employee names

```sql
SELECT name, COUNT(*) AS employee_count
FROM Employee
GROUP BY name
HAVING COUNT(*) > 1;
```

This checks duplicate names, not necessarily duplicate employee records.

#### 8. Find duplicate salary values

```sql
SELECT salary, COUNT(*) AS employee_count
FROM Employee
GROUP BY salary
HAVING COUNT(*) > 1;
```

#### 9. Find salary values held by only one employee

```sql
SELECT salary
FROM Employee
GROUP BY salary
HAVING COUNT(*) = 1;
```

#### 10. Find average salary

```sql
SELECT AVG(salary) AS average_salary
FROM Employee;
```

#### 11. Find average salary by department

```sql
SELECT department_id, AVG(salary) AS average_salary
FROM Employee
GROUP BY department_id;
```

#### 12. Find maximum salary in each department

```sql
SELECT department_id, MAX(salary) AS maximum_salary
FROM Employee
GROUP BY department_id;
```

#### 13. Find minimum salary in each department

```sql
SELECT department_id, MIN(salary) AS minimum_salary
FROM Employee
GROUP BY department_id;
```

#### 14. Find total salary by department

```sql
SELECT department_id, SUM(salary) AS total_salary
FROM Employee
GROUP BY department_id;
```

#### 15. Count employees in each department

```sql
SELECT department_id, COUNT(*) AS employee_count
FROM Employee
GROUP BY department_id;
```

#### 16. Find departments with more than five employees ⭐

```sql
SELECT department_id, COUNT(*) AS employee_count
FROM Employee
GROUP BY department_id
HAVING COUNT(*) > 5;
```

#### 17. Find departments with average salary above 50,000

```sql
SELECT department_id, AVG(salary) AS average_salary
FROM Employee
GROUP BY department_id
HAVING AVG(salary) > 50000;
```

### Joins and managers

#### 18. Show each employee with their department name ⭐

```sql
SELECT e.name, d.department_name
FROM Employee AS e
JOIN Department AS d ON d.department_id = e.department_id;
```

#### 19. Find employees who do not belong to a department

```sql
SELECT e.*
FROM Employee AS e
LEFT JOIN Department AS d ON d.department_id = e.department_id
WHERE d.department_id IS NULL;
```

#### 20. Find departments with no employees

```sql
SELECT d.*
FROM Department AS d
LEFT JOIN Employee AS e ON e.department_id = d.department_id
WHERE e.emp_id IS NULL;
```

#### 21. Show each employee and their manager

```sql
SELECT e.name AS employee_name, m.name AS manager_name
FROM Employee AS e
LEFT JOIN Employee AS m ON m.emp_id = e.manager_id;
```

A `LEFT JOIN` keeps employees who do not have a manager.

#### 22. Find employees earning more than their manager ⭐⭐⭐

```sql
SELECT e.name AS employee_name, e.salary AS employee_salary,
       m.name AS manager_name, m.salary AS manager_salary
FROM Employee AS e
JOIN Employee AS m ON m.emp_id = e.manager_id
WHERE e.salary > m.salary;
```

### Top-N and ranking

#### 23. Find the three highest distinct salary values

```sql
SELECT DISTINCT salary
FROM Employee
ORDER BY salary DESC
LIMIT 3;
```

#### 24. Find the three highest-paid employees

```sql
SELECT *
FROM Employee
ORDER BY salary DESC, emp_id
LIMIT 3;
```

This returns at most three employees. Use a ranking function if ties at the cutoff must all be included.

#### 25. Find the top three employees in each department ⭐⭐⭐

`ROW_NUMBER` returns at most three employees per department. The unique key makes tie ordering deterministic.

```sql
WITH RankedEmployees AS (
    SELECT e.*,
           ROW_NUMBER() OVER (
               PARTITION BY department_id
               ORDER BY salary DESC, emp_id
           ) AS row_num
    FROM Employee AS e
)
SELECT *
FROM RankedEmployees
WHERE row_num <= 3;
```

#### 26. Rank employees by salary

```sql
SELECT name, salary, RANK() OVER (ORDER BY salary DESC) AS salary_rank
FROM Employee;
```

#### 27. Compare `ROW_NUMBER`, `RANK`, and `DENSE_RANK`

For salaries `100000, 90000, 90000, 80000`:

| Salary | `ROW_NUMBER()` | `RANK()` | `DENSE_RANK()` |
|---:|---:|---:|---:|
| 100000 | 1 | 1 | 1 |
| 90000 | 2 | 2 | 2 |
| 90000 | 3 | 2 | 2 |
| 80000 | 4 | 4 | 3 |

- `ROW_NUMBER`: assigns a unique sequence, including to ties.
- `RANK`: ties share a rank; the next rank is skipped.
- `DENSE_RANK`: ties share a rank; the next rank is not skipped.

#### 28. Find employees with the second-highest salary using `DENSE_RANK`

```sql
WITH RankedEmployees AS (
    SELECT e.*, DENSE_RANK() OVER (ORDER BY salary DESC) AS salary_rank
    FROM Employee AS e
)
SELECT *
FROM RankedEmployees
WHERE salary_rank = 2;
```

### Dates and NULLs

#### 29. Find employees who joined after a date

```sql
SELECT *
FROM Employee
WHERE joining_date > '2025-01-01';
```

#### 30. Find the most recently hired employee(s)

```sql
SELECT *
FROM Employee
WHERE joining_date = (SELECT MAX(joining_date) FROM Employee);
```

Returns all employees tied for the latest joining date.

#### 31. Find employees who joined during 2025

```sql
SELECT *
FROM Employee
WHERE joining_date >= '2025-01-01'
  AND joining_date <  '2026-01-01';
```

The half-open date range includes all times on December 31 and can be easier to optimize than applying a function to the column.

#### 32. Find employees without a manager

```sql
SELECT *
FROM Employee
WHERE manager_id IS NULL;
```

#### 33. Display salary with NULL replaced by zero

```sql
SELECT name, COALESCE(salary, 0) AS salary
FROM Employee;
```

### Subqueries and existence checks

#### 34. Find employees earning above the company average

```sql
SELECT *
FROM Employee
WHERE salary > (SELECT AVG(salary) FROM Employee);
```

#### 35. Find employees earning below the company average

```sql
SELECT *
FROM Employee
WHERE salary < (SELECT AVG(salary) FROM Employee);
```

#### 36. Find employees earning the maximum salary in their department

```sql
SELECT e.*
FROM Employee AS e
WHERE e.salary = (
    SELECT MAX(e2.salary)
    FROM Employee AS e2
    WHERE e2.department_id = e.department_id
);
```

#### 37. Find departments that have at least one employee

```sql
SELECT d.*
FROM Department AS d
WHERE EXISTS (
    SELECT 1
    FROM Employee AS e
    WHERE e.department_id = d.department_id
);
```

#### 38. Find departments with more than three employees

```sql
SELECT d.department_id, d.department_name, COUNT(*) AS employee_count
FROM Department AS d
JOIN Employee AS e ON e.department_id = d.department_id
GROUP BY d.department_id, d.department_name
HAVING COUNT(*) > 3;
```

### Filtering patterns

#### 39. Find employees whose name starts with `A`

```sql
SELECT *
FROM Employee
WHERE name LIKE 'A%';
```

#### 40. Find employees whose name ends with `n`

```sql
SELECT *
FROM Employee
WHERE name LIKE '%n';
```

#### 41. Find employees whose name contains `an`

```sql
SELECT *
FROM Employee
WHERE name LIKE '%an%';
```

Case sensitivity depends on the database and collation.

#### 42. Find employees earning between 30,000 and 60,000

```sql
SELECT *
FROM Employee
WHERE salary BETWEEN 30000 AND 60000;
```

`BETWEEN` includes both endpoints.

#### 43. Find employees in selected departments

```sql
SELECT *
FROM Employee
WHERE department_id IN (1, 2, 3);
```

### Interview reminders

- Clarify whether “Nth highest” means the Nth distinct salary or the Nth employee after sorting.
- Use `DENSE_RANK` for distinct salary ranks; account for ties.
- `COUNT(*)` counts rows; `COUNT(column)` ignores rows where that column is `NULL`.
- Use `IS NULL` / `IS NOT NULL`; `= NULL` does not test for NULL.
- Use `WHERE` for rows and `HAVING` for groups after aggregation.
- Qualify column names with table aliases when joining tables.
