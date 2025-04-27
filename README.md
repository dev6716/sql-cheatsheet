# 🔹 Data Engineer: SQL Master Cheat Sheet

---

# 🔹 Topic 1: Basic SQL Skills (Foundations)

### Overview
Basic SQL operations are the foundation of any query. These commands allow you to retrieve, filter, sort, and manage your data.

### Key Concepts

**SELECT**: Specifies the columns to retrieve.
```sql
SELECT name, age FROM employees;
```
*Use when you want to view specific fields.*

**FROM**: Indicates the table from which to pull data.

**WHERE**: Filters rows based on conditions.
```sql
SELECT * FROM employees WHERE age > 30;
```
*Use to apply conditions to narrow down results.*

**ORDER BY**: Sorts the output.
```sql
SELECT name FROM employees ORDER BY age DESC;
```
*Use to prioritize how results appear.*

**DISTINCT**: Eliminates duplicate rows.
```sql
SELECT DISTINCT department FROM employees;
```
*Use when you need unique values.*

**LIMIT / OFFSET**: Control how many results are shown.
```sql
SELECT * FROM employees LIMIT 5 OFFSET 10;
```
*Use for pagination or sample previews.*

---

# 🔹 Topic 2: Aggregations

### Overview
Aggregation functions are essential for summarizing data.

### Key Concepts

**GROUP BY**: Groups rows sharing a property.
```sql
SELECT department, COUNT(*) FROM employees GROUP BY department;
```
*Use when you need per-group summaries.*

**HAVING**: Filters groups after aggregation.
```sql
SELECT department, COUNT(*) FROM employees GROUP BY department HAVING COUNT(*) > 10;
```
*Use when needing conditions post-grouping.*

**Common Aggregates**:
- **COUNT()**: Total rows
- **SUM()**: Total value
- **AVG()**: Average value
- **MIN()/MAX()**: Minimum/maximum value

Example:
```sql
SELECT AVG(salary) FROM employees;
```

---

# 🔹 Topic 3: Joins

### Overview
Joins combine rows from two or more tables based on a related column.

### Types of Joins

**INNER JOIN**: Only matching records.
```sql
SELECT e.name, d.department_name 
FROM employees e 
INNER JOIN departments d ON e.department_id = d.id;
```
*Use for strict relational retrieval.*

**LEFT JOIN**: All left table records + matched right.
```sql
SELECT e.name, d.department_name 
FROM employees e 
LEFT JOIN departments d ON e.department_id = d.id;
```
*Use to keep all data from the left table.*

**RIGHT JOIN**: All right table records + matched left.

**FULL OUTER JOIN**: All records from both tables (even unmatched).

**SELF JOIN**: A table joins itself.
```sql
SELECT a.name, b.name AS manager 
FROM employees a 
JOIN employees b ON a.manager_id = b.id;
```

---

# 🔹 Topic 4: Subqueries

### Overview
Subqueries are queries nested inside another query.

### Examples

**In SELECT**:
```sql
SELECT name, 
  (SELECT COUNT(*) FROM sales WHERE sales.emp_id = employees.id) AS total_sales 
FROM employees;
```

**In WHERE**:
```sql
SELECT name 
FROM employees 
WHERE department_id IN (SELECT id FROM departments WHERE location = 'NYC');
```

**In FROM (Derived Table)**:
```sql
SELECT avg_salary 
FROM (SELECT AVG(salary) AS avg_salary FROM employees) AS avg_table;
```
*Use for modular and complex queries.*

---

# 🔹 Topic 5: Common Table Expressions (CTEs)

### Overview
CTEs provide a way to simplify complex queries.

### Examples

**Simple CTE**:
```sql
WITH dept_count AS (
  SELECT department_id, COUNT(*) AS total 
  FROM employees 
  GROUP BY department_id
)
SELECT * 
FROM dept_count 
WHERE total > 10;
```

**Chaining CTEs**:
```sql
WITH first AS (...),
     second AS (...)
SELECT * 
FROM second;
```

**Recursive CTE**:
Used for hierarchical data (e.g., org charts).

---

# 🔹 Topic 6: Window Functions

### Overview
Window functions perform calculations across a set of table rows related to the current row.

### Key Functions

**ROW_NUMBER(), RANK(), DENSE_RANK()**:
```sql
SELECT name, department_id, 
  ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC) AS row_num 
FROM employees;
```

**LEAD(), LAG()**:
```sql
SELECT name, salary, 
  LAG(salary) OVER (ORDER BY salary) AS previous_salary 
FROM employees;
```

**SUM() OVER**:
```sql
SELECT name, 
  SUM(salary) OVER (ORDER BY name) AS running_salary 
FROM employees;
```
*Use for ranking, running totals, and comparisons.*

---

# 🔹 Topic 7: Set Operations

### Overview
Set operations combine results from multiple queries.

### Types

**UNION** (removes duplicates):
```sql
SELECT name FROM employees
UNION
SELECT name FROM managers;
```

**UNION ALL** (keeps duplicates)

**INTERSECT**: Common rows between queries.

**EXCEPT (MINUS)**: Rows in first query not in second.

---

# 🔹 Topic 8: CASE Statements

### Overview
Conditional expressions in SQL.

### Example
```sql
SELECT name,
  CASE
    WHEN salary > 100000 THEN 'High'
    WHEN salary > 50000 THEN 'Medium'
    ELSE 'Low'
  END AS salary_band 
FROM employees;
```
*Use to categorize data dynamically.*

---

# 🔹 Topic 9: String and Date Functions

### String Functions
```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name 
FROM employees;

SELECT LENGTH(name) 
FROM employees;

SELECT LOWER(name) 
FROM employees;
```

### Date Functions
```sql
SELECT NOW(); -- Current timestamp
SELECT CURDATE(); -- Current date
SELECT DATE_ADD(NOW(), INTERVAL 7 DAY);
SELECT DATEDIFF('2024-12-31', NOW());
```
*Use to manipulate text and dates efficiently.*

---

# 🔹 Topic 10: Indexes and Performance

### Overview
Indexes speed up retrieval but may slow inserts/updates.

### Example
```sql
CREATE INDEX idx_department_id ON employees(department_id);
```

**Use indexes when:**
- Frequently filtering/joining large tables.
- SELECT queries are slow.

**Avoid indexes when:**
- Tables are very small.
- Tables have high write (INSERT/UPDATE) activity.

**Check query performance**:
```sql
EXPLAIN SELECT * FROM employees WHERE department_id = 5;
```

---

# 🔹 Topic 11: Transactions (Basic Idea)

### Overview
Transactions group queries into a single logical unit.

### Key Commands
```sql
BEGIN;
UPDATE employees 
SET salary = salary * 1.1 
WHERE department_id = 2;
COMMIT;

-- If something goes wrong
ROLLBACK;
```

### ACID Principles
- **Atomicity**: Complete or nothing.
- **Consistency**: Data remains valid.
- **Isolation**: Transactions don't interfere.
- **Durability**: Once committed, it's permanent.

---

# 🔥 FINAL TIPS for BCG X Data Engineering Interviews

- Practice writing queries **without auto-complete**.
- **Always use table aliases** for clarity.
- **Understand when to use indexes**.
- **Be ready to explain design choices** in SQL.
- **Visualize how SQL fits in ETL/ELT pipelines**.

---

# 🚀 With this sheet, you are READY to MASTER SQL for your Interview!

