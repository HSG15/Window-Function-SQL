
# 📊 Window Functions in PostgreSQL — `employees` Example

This repository demonstrates how to use common **Window Functions** in PostgreSQL using an `employees` table.

## 🗂️ Table Definition & Data

```sql
CREATE TABLE employees (
  emp_id INTEGER PRIMARY KEY,
  name VARCHAR(15),
  department VARCHAR(10),
  salary INTEGER
);

INSERT INTO employees (emp_id, name, department, salary) VALUES
(1, 'Alice', 'Sales', 5000),
(2, 'Bob', 'Sales', 6000),
(3, 'Carol', 'Sales', 5500),
(4, 'Dave', 'HR', 4000),
(5, 'Eve', 'HR', 4500),
(6, 'Frank', 'IT', 7000),
(7, 'Grace', 'IT', 7500);
```

## ✅ Check the Data

```sql
SELECT * FROM employees;
```

## 1️⃣ **ROW_NUMBER()** — Rank employees by salary within each department

```sql
SELECT 
  emp_id, 
  name, 
  department, 
  salary,
  ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS dept_rank
FROM 
  employees;
```

## 2️⃣ **RANK() vs DENSE_RANK()** — See how they handle ties

| Function | How it handles ties |
| -------- | ------------------- |
| RANK() | Skips ranks → 1, 1, 3 |
| DENSE_RANK() | No skipping → 1, 1, 2 |

```sql
SELECT 
  emp_id, 
  name, 
  department, 
  salary,
  RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dept_rank,
  DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dept_dense_rank
FROM 
  employees;
```

## 3️⃣ **SUM()** — Total salary per department

```sql
SELECT 
  emp_id, 
  name, 
  department, 
  salary,
  SUM(salary) OVER (PARTITION BY department) AS dept_total_salary
FROM 
  employees;
```

## 4️⃣ **AVG()** — Average salary per department

```sql
SELECT 
  emp_id, 
  name, 
  department, 
  salary,
  AVG(salary) OVER (PARTITION BY department) AS dept_avg_salary
FROM 
  employees;
```

## 5️⃣ **Running Total** — Cumulative salary within each department

```sql
SELECT 
  emp_id, 
  name, 
  department, 
  salary,
  SUM(salary) OVER (PARTITION BY department ORDER BY salary) AS running_salary
FROM 
  employees
WHERE 
  department = 'Sales';
```

## 6️⃣ **LAG() & LEAD()** — Previous and next salary

```sql
SELECT 
  emp_id, 
  name, 
  department, 
  salary,
  LAG(salary) OVER (PARTITION BY department ORDER BY salary) AS prev_salary,
  LEAD(salary) OVER (PARTITION BY department ORDER BY salary) AS next_salary
FROM 
  employees
WHERE 
  department = 'Sales';
```

## ✅ **How to use**
1. Copy the `CREATE TABLE` and `INSERT` statements to create the test data.
2. Run each query to practice how different Window Functions work.
