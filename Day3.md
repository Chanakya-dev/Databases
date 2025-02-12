# **SQL Aggregate Functions**
Aggregate functions in SQL perform a calculation on multiple values and return a single value. These functions are commonly used with the `GROUP BY` clause to summarize data.

## **1️⃣ COUNT()**
- Returns the number of rows in a result set.
- Ignores `NULL` values.

🔹 **Syntax:**  
```sql
SELECT COUNT(column_name) FROM table_name WHERE condition;
```
🔹 **Example:**  
```sql
SELECT COUNT(*) FROM employees WHERE department = 'Sales';
```
(Counts the number of employees in the Sales department.)

---

## **2️⃣ SUM()**
- Returns the total sum of a numeric column.

🔹 **Syntax:**  
```sql
SELECT SUM(column_name) FROM table_name WHERE condition;
```
🔹 **Example:**  
```sql
SELECT SUM(salary) FROM employees WHERE department = 'HR';
```
(Calculates the total salary of all HR employees.)

---

## **3️⃣ AVG()**
- Returns the average value of a numeric column.

🔹 **Syntax:**  
```sql
SELECT AVG(column_name) FROM table_name WHERE condition;
```
🔹 **Example:**  
```sql
SELECT AVG(salary) FROM employees WHERE department = 'IT';
```
(Finds the average salary of IT employees.)

---

## **4️⃣ MIN()**
- Returns the smallest value in a column.

🔹 **Syntax:**  
```sql
SELECT MIN(column_name) FROM table_name WHERE condition;
```
🔹 **Example:**  
```sql
SELECT MIN(salary) FROM employees;
```
(Finds the lowest salary in the employees table.)

---

## **5️⃣ MAX()**
- Returns the largest value in a column.

🔹 **Syntax:**  
```sql
SELECT MAX(column_name) FROM table_name WHERE condition;
```
🔹 **Example:**  
```sql
SELECT MAX(salary) FROM employees WHERE department = 'Finance';
```
(Finds the highest salary in the Finance department.)

---

# **GROUP BY with Aggregate Functions**
- The `GROUP BY` clause is used to group rows that have the same values into summary rows.
- It is often used with aggregate functions.

🔹 **Example: Count employees in each department**  
```sql
SELECT department, COUNT(*) 
FROM employees 
GROUP BY department;
```
(Counts the number of employees in each department.)

🔹 **Example: Find the highest salary in each department**  
```sql
SELECT department, MAX(salary) 
FROM employees 
GROUP BY department;
```

---

# **HAVING vs WHERE**
- `WHERE` is used to filter rows **before** aggregation.
- `HAVING` is used to filter groups **after** aggregation.

🔹 **Example: Find departments with more than 5 employees**  
```sql
SELECT department, COUNT(*) 
FROM employees 
GROUP BY department
HAVING COUNT(*) > 5;
```

---

# **Key Points**
✔ Aggregate functions work on **sets of values**.  
✔ `COUNT(*)` includes `NULL` values, but `COUNT(column_name)` ignores `NULL`.  
✔ `GROUP BY` groups data **before** applying aggregate functions.  
✔ Use `HAVING` instead of `WHERE` when filtering aggregated results.

---
