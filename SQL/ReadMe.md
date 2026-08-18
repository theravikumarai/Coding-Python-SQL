# SQL – Complete Revision Notes

A concise revision guide covering **Basic SQL, Intermediate SQL, Advanced SQL, and Window Functions**.

---

# 📚 Table of Contents

1. [What is SQL?](#1-what-is-sql)
2. [Basic SQL](#2-basic-sql)
3. [Filtering Data](#3-filtering-data)
4. [Sorting and Limiting](#4-sorting-and-limiting)
5. [Aggregate Functions](#5-aggregate-functions)
6. [GROUP BY and HAVING](#6-group-by-and-having)
7. [SQL Joins](#7-sql-joins)
8. [Subqueries](#8-subqueries)
9. [CTEs](#9-ctes)
10. [Set Operations](#10-set-operations)
11. [CASE Statements](#11-case-statements)
12. [NULL Handling](#12-null-handling)
13. [Date and String Functions](#13-date-and-string-functions)
14. [Advanced SQL](#14-advanced-sql)
15. [Window Functions](#15-window-functions)
16. [Ranking Functions](#16-ranking-functions)
17. [Analytical Window Functions](#17-analytical-window-functions)
18. [Common Advanced SQL Patterns](#18-common-advanced-sql-patterns)
19. [SQL Execution Order](#19-sql-execution-order)
20. [Interview Revision Checklist](#20-interview-revision-checklist)

---

# 1. What is SQL?

**SQL (Structured Query Language)** is used to:

* Store data
* Retrieve data
* Update data
* Delete data
* Manage databases
* Analyze data

## Main SQL Categories

| Category | Purpose                   | Commands                              |
| -------- | ------------------------- | ------------------------------------- |
| DDL      | Define database structure | `CREATE`, `ALTER`, `DROP`, `TRUNCATE` |
| DML      | Modify data               | `INSERT`, `UPDATE`, `DELETE`          |
| DQL      | Retrieve data             | `SELECT`                              |
| DCL      | Control permissions       | `GRANT`, `REVOKE`                     |
| TCL      | Manage transactions       | `COMMIT`, `ROLLBACK`, `SAVEPOINT`     |

---

# 2. Basic SQL

## SELECT

Retrieve data from a table.

```sql
SELECT *
FROM employees;
```

Select specific columns:

```sql
SELECT employee_id, name, salary
FROM employees;
```

## DISTINCT

Remove duplicate values.

```sql
SELECT DISTINCT department
FROM employees;
```

## Aliases

Rename columns or tables temporarily.

```sql
SELECT
    name AS employee_name,
    salary AS employee_salary
FROM employees;
```

---

# 3. Filtering Data

## WHERE

```sql
SELECT *
FROM employees
WHERE salary > 50000;
```

## Comparison Operators

```text
=       Equal
!=      Not equal
<>      Not equal
>       Greater than
<       Less than
>=      Greater than or equal
<=      Less than or equal
```

## AND

```sql
SELECT *
FROM employees
WHERE department = 'IT'
AND salary > 50000;
```

## OR

```sql
SELECT *
FROM employees
WHERE department = 'IT'
OR department = 'HR';
```

## IN

```sql
SELECT *
FROM employees
WHERE department IN ('IT', 'HR', 'Finance');
```

## BETWEEN

```sql
SELECT *
FROM employees
WHERE salary BETWEEN 50000 AND 100000;
```

## LIKE

```sql
-- Starts with A
WHERE name LIKE 'A%'

-- Ends with n
WHERE name LIKE '%n'

-- Contains ra
WHERE name LIKE '%ra%'

-- Exactly one character
WHERE name LIKE '_a%'
```

## IS NULL

```sql
SELECT *
FROM employees
WHERE manager_id IS NULL;
```

---

# 4. Sorting and Limiting

## ORDER BY

```sql
SELECT *
FROM employees
ORDER BY salary DESC;
```

Multiple columns:

```sql
SELECT *
FROM employees
ORDER BY department ASC, salary DESC;
```

## LIMIT

```sql
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 5;
```

---

# 5. Aggregate Functions

Aggregate functions perform calculations on multiple rows.

## COUNT()

```sql
SELECT COUNT(*)
FROM employees;
```

## SUM()

```sql
SELECT SUM(salary)
FROM employees;
```

## AVG()

```sql
SELECT AVG(salary)
FROM employees;
```

## MIN() and MAX()

```sql
SELECT
    MIN(salary),
    MAX(salary)
FROM employees;
```

### Important

```sql
COUNT(*)
```

Counts all rows.

```sql
COUNT(column_name)
```

Does **not** count `NULL` values.

---

# 6. GROUP BY and HAVING

## GROUP BY

Used to create groups.

```sql
SELECT
    department,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```

## HAVING

Filters groups after aggregation.

```sql
SELECT
    department,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 60000;
```

### WHERE vs HAVING

| WHERE                                    | HAVING                         |
| ---------------------------------------- | ------------------------------ |
| Filters rows                             | Filters groups                 |
| Before `GROUP BY`                        | After `GROUP BY`               |
| Cannot directly filter aggregate results | Used with aggregate conditions |

---

# 7. SQL Joins

Joins combine data from multiple tables.

## INNER JOIN

Returns matching records.

```sql
SELECT
    e.name,
    d.department_name
FROM employees e
INNER JOIN departments d
    ON e.department_id = d.department_id;
```

## LEFT JOIN

Returns all records from the left table.

```sql
SELECT *
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id;
```

## RIGHT JOIN

Returns all records from the right table.

```sql
SELECT *
FROM employees e
RIGHT JOIN departments d
    ON e.department_id = d.department_id;
```

## CROSS JOIN

Returns every possible combination.

```sql
SELECT *
FROM employees
CROSS JOIN departments;
```

## SELF JOIN

A table joined with itself.

```sql
SELECT
    e.name AS employee,
    m.name AS manager
FROM employees e
LEFT JOIN employees m
    ON e.manager_id = m.employee_id;
```

---

# 8. Subqueries

A query inside another query.

## Single Value Subquery

```sql
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

## IN Subquery

```sql
SELECT *
FROM employees
WHERE department_id IN (
    SELECT department_id
    FROM departments
    WHERE location = 'Pune'
);
```

## EXISTS

Checks whether a subquery returns rows.

```sql
SELECT *
FROM customers c
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```

### Important

Use `EXISTS` when checking whether related records exist.

---

# 9. CTEs

A **CTE (Common Table Expression)** creates a temporary named result set.

```sql
WITH department_salary AS (
    SELECT
        department,
        AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department
)
SELECT *
FROM department_salary
WHERE avg_salary > 60000;
```

## Multiple CTEs

```sql
WITH
employee_count AS (
    SELECT
        department_id,
        COUNT(*) AS total_employees
    FROM employees
    GROUP BY department_id
),
department_salary AS (
    SELECT
        department_id,
        AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
)
SELECT *
FROM employee_count
JOIN department_salary
    USING (department_id);
```

---

# 10. Set Operations

## UNION

Combines results and removes duplicates.

```sql
SELECT email FROM customers
UNION
SELECT email FROM employees;
```

## UNION ALL

Keeps duplicates.

```sql
SELECT email FROM customers
UNION ALL
SELECT email FROM employees;
```

## INTERSECT

Returns common rows.

```sql
SELECT customer_id FROM orders
INTERSECT
SELECT customer_id FROM payments;
```

## EXCEPT

Returns rows from the first query that do not exist in the second.

```sql
SELECT customer_id FROM customers
EXCEPT
SELECT customer_id FROM orders;
```

---

# 11. CASE Statements

Used for conditional logic.

```sql
SELECT
    name,
    salary,
    CASE
        WHEN salary >= 100000 THEN 'High'
        WHEN salary >= 50000 THEN 'Medium'
        ELSE 'Low'
    END AS salary_category
FROM employees;
```

## Conditional Aggregation

```sql
SELECT
    department,
    SUM(
        CASE
            WHEN salary > 50000 THEN 1
            ELSE 0
        END
    ) AS high_salary_employees
FROM employees
GROUP BY department;
```

---

# 12. NULL Handling

## COALESCE()

Returns the first non-null value.

```sql
SELECT
    name,
    COALESCE(bonus, 0) AS bonus
FROM employees;
```

## IFNULL() – MySQL

```sql
SELECT
    IFNULL(bonus, 0)
FROM employees;
```

## NULLIF()

Returns `NULL` when two values are equal.

```sql
SELECT NULLIF(10, 10);
```

Useful for avoiding division by zero.

```sql
SELECT
    revenue / NULLIF(quantity, 0)
FROM sales;
```

---

# 13. Date and String Functions

## String Functions

```sql
UPPER(name)
LOWER(name)
LENGTH(name)
CONCAT(first_name, ' ', last_name)
TRIM(name)
SUBSTRING(name, 1, 5)
REPLACE(name, 'a', 'A')
```

## MySQL Date Functions

```sql
CURDATE()
NOW()
YEAR(order_date)
MONTH(order_date)
DAY(order_date)
DATEDIFF(date1, date2)
DATE_ADD(date, INTERVAL 7 DAY)
DATE_SUB(date, INTERVAL 1 MONTH)
```

---

# 14. Advanced SQL

Advanced SQL focuses on:

* Complex joins
* Subqueries
* CTEs
* Recursive CTEs
* Window functions
* Analytical queries
* Query optimization
* Transactions
* Indexes

---

## Recursive CTE

Useful for hierarchical data.

```sql
WITH RECURSIVE employee_hierarchy AS (

    SELECT
        employee_id,
        name,
        manager_id,
        1 AS level
    FROM employees
    WHERE manager_id IS NULL

    UNION ALL

    SELECT
        e.employee_id,
        e.name,
        e.manager_id,
        h.level + 1
    FROM employees e
    JOIN employee_hierarchy h
        ON e.manager_id = h.employee_id
)
SELECT *
FROM employee_hierarchy;
```

Common uses:

* Employee hierarchy
* Organizational structure
* Category trees
* Generating sequences

---

# 15. Window Functions

Window functions perform calculations across related rows **without collapsing rows**.

## Syntax

```sql
function_name()
OVER (
    PARTITION BY column
    ORDER BY column
)
```

## Example

```sql
SELECT
    employee_id,
    department,
    salary,
    AVG(salary) OVER (
        PARTITION BY department
    ) AS department_avg_salary
FROM employees;
```

---

# 16. Ranking Functions

## ROW_NUMBER()

Assigns a unique number to each row.

```sql
ROW_NUMBER() OVER (
    PARTITION BY department
    ORDER BY salary DESC
)
```

Example use cases:

* Top N per group
* Latest record per user
* Duplicate detection

---

## RANK()

Same values receive the same rank.

Ranks may have gaps.

```text
Salary: 100, 100, 90

Rank:
1
1
3
```

```sql
RANK() OVER (
    ORDER BY salary DESC
)
```

---

## DENSE_RANK()

Same values receive the same rank.

No gaps.

```text
Salary: 100, 100, 90

Dense Rank:
1
1
2
```

---

## NTILE()

Divides rows into buckets.

```sql
NTILE(4) OVER (
    ORDER BY salary
)
```

Common use:

* Quartiles
* Customer segmentation
* Percentile grouping

---

# 17. Analytical Window Functions

## LAG()

Accesses the previous row.

```sql
SELECT
    customer_id,
    order_date,
    LAG(order_date) OVER (
        PARTITION BY customer_id
        ORDER BY order_date
    ) AS previous_order
FROM orders;
```

Use cases:

* Consecutive days
* Gap analysis
* Sessionization
* Retention analysis
* Churn analysis

---

## LEAD()

Accesses the next row.

```sql
SELECT
    customer_id,
    order_date,
    LEAD(order_date) OVER (
        PARTITION BY customer_id
        ORDER BY order_date
    ) AS next_order
FROM orders;
```

---

## FIRST_VALUE()

Returns the first value in the window.

```sql
FIRST_VALUE(price) OVER (
    PARTITION BY product_id
    ORDER BY order_date
)
```

---

## LAST_VALUE()

Important: specify the window frame.

```sql
LAST_VALUE(price) OVER (
    PARTITION BY product_id
    ORDER BY order_date
    ROWS BETWEEN
        UNBOUNDED PRECEDING
        AND UNBOUNDED FOLLOWING
)
```

---

## Running Total

```sql
SELECT
    order_date,
    revenue,
    SUM(revenue) OVER (
        ORDER BY order_date
    ) AS running_revenue
FROM sales;
```

---

## Moving Average

```sql
SELECT
    order_date,
    revenue,
    AVG(revenue) OVER (
        ORDER BY order_date
        ROWS BETWEEN 6 PRECEDING
        AND CURRENT ROW
    ) AS moving_avg
FROM sales;
```

---

# 18. Common Advanced SQL Patterns

## 1. Top N Per Group

```sql
WITH ranked AS (
    SELECT
        *,
        ROW_NUMBER() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS rn
    FROM employees
)
SELECT *
FROM ranked
WHERE rn <= 3;
```

---

## 2. Latest Record Per User

```sql
WITH ranked AS (
    SELECT
        *,
        ROW_NUMBER() OVER (
            PARTITION BY user_id
            ORDER BY created_at DESC
        ) AS rn
    FROM user_activity
)
SELECT *
FROM ranked
WHERE rn = 1;
```

---

## 3. Duplicate Detection

```sql
WITH duplicates AS (
    SELECT
        *,
        ROW_NUMBER() OVER (
            PARTITION BY email
            ORDER BY customer_id
        ) AS rn
    FROM customers
)
SELECT *
FROM duplicates
WHERE rn > 1;
```

---

## 4. Consecutive Days

```sql
WITH previous_login AS (
    SELECT
        user_id,
        login_date,
        LAG(login_date) OVER (
            PARTITION BY user_id
            ORDER BY login_date
        ) AS previous_date
    FROM logins
)
SELECT *
FROM previous_login
WHERE DATEDIFF(login_date, previous_date) = 1;
```

---

## 5. Gap Analysis

Find the gap between events.

```sql
SELECT
    user_id,
    event_date,
    LAG(event_date) OVER (
        PARTITION BY user_id
        ORDER BY event_date
    ) AS previous_event,
    DATEDIFF(
        event_date,
        LAG(event_date) OVER (
            PARTITION BY user_id
            ORDER BY event_date
        )
    ) AS gap_days
FROM events;
```

---

## 6. Sessionization

Create a new session when inactivity exceeds a threshold.

```sql
WITH event_gaps AS (
    SELECT
        user_id,
        event_time,
        LAG(event_time) OVER (
            PARTITION BY user_id
            ORDER BY event_time
        ) AS previous_event
    FROM user_events
),
session_flags AS (
    SELECT
        *,
        CASE
            WHEN previous_event IS NULL THEN 1
            WHEN TIMESTAMPDIFF(
                MINUTE,
                previous_event,
                event_time
            ) > 30 THEN 1
            ELSE 0
        END AS new_session
    FROM event_gaps
)
SELECT
    *,
    SUM(new_session) OVER (
        PARTITION BY user_id
        ORDER BY event_time
    ) AS session_id
FROM session_flags;
```

---

## 7. Cohort Analysis

Identify users based on their first activity.

```sql
WITH user_cohort AS (
    SELECT
        user_id,
        MIN(order_date) AS cohort_date
    FROM orders
    GROUP BY user_id
)
SELECT *
FROM user_cohort;
```

Common functions:

```text
MIN()
FIRST_VALUE()
LAG()
```

---

## 8. Retention Analysis

Determine whether users return.

```sql
WITH user_activity AS (
    SELECT
        user_id,
        activity_date,
        LEAD(activity_date) OVER (
            PARTITION BY user_id
            ORDER BY activity_date
        ) AS next_activity
    FROM activities
)
SELECT *
FROM user_activity;
```

Common functions:

```text
LAG()
LEAD()
```

---

## 9. Funnel Analysis

Analyze progression through multiple steps.

Example funnel:

```text
Visit
↓
Product View
↓
Add to Cart
↓
Purchase
```

Common techniques:

```text
ROW_NUMBER()
LEAD()
CASE
Conditional Aggregation
```

---

## 10. Customer Lifetime Value

Calculate total value generated by each customer.

```sql
SELECT
    customer_id,
    SUM(order_amount) AS customer_lifetime_value
FROM orders
GROUP BY customer_id;
```

Can also include:

* First purchase date
* Last purchase date
* Purchase frequency
* Average order value

---

## 11. Churn Analysis

Identify inactive customers.

Example approach:

```sql
WITH customer_activity AS (
    SELECT
        customer_id,
        MAX(order_date) AS last_order_date
    FROM orders
    GROUP BY customer_id
)
SELECT
    *,
    DATEDIFF(CURDATE(), last_order_date) AS inactive_days
FROM customer_activity;
```

---

# 19. SQL Execution Order

Logical execution order:

```text
1. FROM
2. JOIN
3. ON
4. WHERE
5. GROUP BY
6. HAVING
7. SELECT
8. DISTINCT
9. ORDER BY
10. LIMIT
```

### Important Interview Point

This query:

```sql
SELECT
    salary * 12 AS annual_salary
FROM employees
WHERE annual_salary > 100000;
```

Does not work in many SQL dialects because `WHERE` executes before `SELECT`.

Use:

```sql
SELECT *
FROM (
    SELECT
        salary * 12 AS annual_salary
    FROM employees
) t
WHERE annual_salary > 100000;
```

Or use a CTE.

---

# 20. SQL Performance Basics

## Index

Indexes improve data retrieval performance.

Useful for columns frequently used in:

* `WHERE`
* `JOIN`
* `ORDER BY`
* `GROUP BY`

Example:

```sql
CREATE INDEX idx_employee_department
ON employees(department_id);
```

## Avoid

```sql
SELECT *
```

when only a few columns are required.

Prefer:

```sql
SELECT
    employee_id,
    name,
    salary
FROM employees;
```

## Other Performance Considerations

* Filter early when appropriate.
* Use indexes carefully.
* Avoid unnecessary subqueries.
* Understand the execution plan.
* Avoid functions on indexed columns when they prevent efficient index usage.
* Use `UNION ALL` when duplicate removal is unnecessary.
* Select only required columns.

---

# 21. Transactions

A transaction is a group of operations treated as one unit.

```sql
START TRANSACTION;

UPDATE accounts
SET balance = balance - 1000
WHERE account_id = 1;

UPDATE accounts
SET balance = balance + 1000
WHERE account_id = 2;

COMMIT;
```

If something fails:

```sql
ROLLBACK;
```

## ACID Properties

| Property    | Meaning                                   |
| ----------- | ----------------------------------------- |
| Atomicity   | All operations succeed or none do         |
| Consistency | Data remains valid                        |
| Isolation   | Transactions do not interfere incorrectly |
| Durability  | Committed data persists                   |

---

# 🎯 SQL Interview Patterns to Master

## Basic

* [ ] `SELECT`
* [ ] `WHERE`
* [ ] `DISTINCT`
* [ ] `ORDER BY`
* [ ] `LIMIT`
* [ ] `LIKE`
* [ ] `IN`
* [ ] `BETWEEN`
* [ ] `NULL`

## Aggregation

* [ ] `COUNT()`
* [ ] `SUM()`
* [ ] `AVG()`
* [ ] `MIN()`
* [ ] `MAX()`
* [ ] `GROUP BY`
* [ ] `HAVING`

## Joins

* [ ] `INNER JOIN`
* [ ] `LEFT JOIN`
* [ ] `RIGHT JOIN`
* [ ] `CROSS JOIN`
* [ ] `SELF JOIN`

## Intermediate

* [ ] Subqueries
* [ ] Correlated subqueries
* [ ] `EXISTS`
* [ ] CTEs
* [ ] Recursive CTEs
* [ ] `UNION`
* [ ] `UNION ALL`
* [ ] `CASE`
* [ ] Date functions
* [ ] String functions
* [ ] NULL handling

## Advanced

* [ ] Window functions
* [ ] `PARTITION BY`
* [ ] `ORDER BY` inside `OVER()`
* [ ] Window frames
* [ ] `ROW_NUMBER()`
* [ ] `RANK()`
* [ ] `DENSE_RANK()`
* [ ] `NTILE()`
* [ ] `LAG()`
* [ ] `LEAD()`
* [ ] `FIRST_VALUE()`
* [ ] `LAST_VALUE()`

## Advanced Problem Patterns

* [ ] Top N per group
* [ ] Latest record per user
* [ ] Duplicate detection
* [ ] Consecutive dates
* [ ] Gap analysis
* [ ] Sessionization
* [ ] Cohort analysis
* [ ] Retention analysis
* [ ] Churn analysis
* [ ] Funnel analysis
* [ ] Customer lifetime value
* [ ] Running totals
* [ ] Moving averages

---

# 🚀 Recommended Learning Order

```text
SQL Basics
    ↓
Filtering & Sorting
    ↓
Aggregate Functions
    ↓
GROUP BY & HAVING
    ↓
Joins
    ↓
Subqueries
    ↓
CTEs
    ↓
CASE Statements
    ↓
Set Operations
    ↓
Date & String Functions
    ↓
Window Functions
    ↓
Advanced SQL Patterns
    ↓
Query Optimization
    ↓
SQL Interview Problems
```

---

# 💡 Final Revision Formula

```text
Basic SQL
+
Joins
+
Aggregations
+
Subqueries
+
CTEs
+
CASE
+
Window Functions
+
Advanced Problem Patterns
+
Query Optimization
=
Strong SQL Skills
```

**Focus especially on `JOIN`, `GROUP BY`, `CTE`, `Subquery`, and `Window Functions` for SQL interviews.**
