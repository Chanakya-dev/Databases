### **1. Filtering Data Using `WHERE`, `AND`, and `OR`**
The `WHERE` clause is used to filter records based on specific conditions.

#### **Example Table: `employees`**
| id  | name   | department  | salary  | age |
|-----|--------|------------|---------|-----|
| 1   | Alice  | HR         | 50000   | 28  |
| 2   | Bob    | IT         | 60000   | 32  |
| 3   | Charlie| Finance    | 55000   | 29  |
| 4   | David  | IT         | 65000   | 35  |
| 5   | Eve    | HR         | 48000   | 25  |

#### **`Table Creation Query`**
````sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    department VARCHAR(50),
    salary INT,
    age INT
);
````

#### **Query 1: Using `AND` for Filtering**
```sql
SELECT * FROM employees 
WHERE department = 'IT' AND salary > 60000;
```
**Explanation:**  
- This query selects employees who **work in the IT department** **and** have a salary greater than `60000`.

#### **Query 2: Using `OR` for Filtering**
```sql
SELECT * FROM employees 
WHERE department = 'HR' OR salary > 55000;
```
**Explanation:**  
- This query selects employees who are either **in the HR department** **or** have a **salary greater than 55000**.

#### **Query 3: Combining `AND` and `OR`**
```sql
SELECT * FROM employees 
WHERE (department = 'IT' OR department = 'Finance') AND salary > 55000;
```
**Explanation:**  
- This query selects employees who are **either in IT or Finance** but **only if their salary is greater than 55000**.

---

### **2. Sorting Data Using `ORDER BY`**
The `ORDER BY` clause is used to sort the result set in **ascending (`ASC`)** or **descending (`DESC`)** order.

#### **Query 4: Sorting by Salary in Ascending Order**
```sql
SELECT * FROM employees 
ORDER BY salary ASC;
```
**Explanation:**  
- This query sorts all employees in **ascending order of salary**.

#### **Query 5: Sorting by Salary in Descending Order**
```sql
SELECT * FROM employees 
ORDER BY salary DESC;
```
**Explanation:**  
- This query sorts all employees in **descending order of salary**.

#### **Query 6: Sorting by Multiple Columns**
```sql
SELECT * FROM employees 
ORDER BY department ASC, salary DESC;
```
**Explanation:**  
- This query sorts employees **first by department (A-Z) and then by salary (highest to lowest) within each department**.

---

### **3. Using `CASE` for Conditional Sorting (`WHEN` in `ORDER BY`)**
The `CASE` statement allows conditional sorting.

#### **Query 7: Sorting Employees Based on a Custom Order**
```sql
SELECT * FROM employees 
ORDER BY 
  CASE 
    WHEN department = 'IT' THEN 1
    WHEN department = 'Finance' THEN 2
    WHEN department = 'HR' THEN 3
    ELSE 4
  END, salary DESC;
```
**Explanation:**  
- This query sorts employees **first by department** in the order of `IT → Finance → HR`, and within each department, it sorts by **salary in descending order**.

---
