# **MySQL Subqueries: A Comprehensive Guide**

## **Introduction to Subqueries**
A subquery is a query nested inside another SQL query. It is used to retrieve data that will be used by the main query. Subqueries are commonly used in **SELECT**, **INSERT**, **UPDATE**, and **DELETE** statements.

## **Types of Subqueries**
1. **Single-row Subqueries** - Returns only one row.
2. **Multi-row Subqueries** - Returns multiple rows.
3. **Correlated Subqueries** - Uses outer query values inside the subquery.
4. **Nested Subqueries** - Subquery inside another subquery.

---
## **Subqueries in CRUD Operations**

### **1. SELECT with Subqueries**
#### Example 1: Using `=`
Find employees who have the same salary as at least one employee from department 10.
```sql
SELECT name, salary
FROM employees
WHERE salary = (SELECT salary FROM employees WHERE department_id = 10);
```
#### Example 2: Using `IN`
Find employees who earn any salary from department 10.
```sql
SELECT name, salary
FROM employees
WHERE salary IN (SELECT salary FROM employees WHERE department_id = 10);
```
#### Example 3: Using `ANY`
Find employees who earn **less than the highest** salary in department 10.
```sql
SELECT name, salary
FROM employees
WHERE salary < ANY (SELECT salary FROM employees WHERE department_id = 10);
```
#### Example 4: Using `ALL`
Find employees who earn **less than the lowest** salary in department 10.
```sql
SELECT name, salary
FROM employees
WHERE salary < ALL (SELECT salary FROM employees WHERE department_id = 10);
```
#### Example 5: Using `EXISTS`
Find employees **only if department 10 has employees**.
```sql
SELECT name, salary
FROM employees
WHERE EXISTS (SELECT 1 FROM employees WHERE department_id = 10);
```
#### Example 6: Using `NOT EXISTS`
Find employees **only if department 10 has no employees**.
```sql
SELECT name, salary
FROM employees
WHERE NOT EXISTS (SELECT 1 FROM employees WHERE department_id = 10);
```

---
### **3. UPDATE with Subqueries**
Increase salary by 10% for employees who have a salary less than the department average.
```sql
UPDATE employees
SET salary = salary * 1.10
WHERE salary < (SELECT AVG(salary) FROM employees);
```

---
### **4. DELETE with Subqueries**
Delete employees who earn less than the minimum salary in department 10.
```sql
DELETE FROM employees
WHERE salary < (SELECT MIN(salary) FROM employees WHERE department_id = 10);
```

---
## **Summary Table of Subquery Operations**
| Operation | Example |
|-----------|------------|
| `=` | Find employees earning the same salary as department 10. |
| `IN` | Find employees with any salary that matches department 10. |
| `ANY` | Find employees earning less than at least one salary in department 10. |
| `ALL` | Find employees earning less than the lowest salary in department 10. |
| `EXISTS` | Return employees if department 10 has employees. |
| `NOT EXISTS` | Return employees if department 10 is empty. |
| `INSERT` | Insert employees who earn above the company average. |
| `UPDATE` | Increase salary for employees earning below the department average. |
| `DELETE` | Remove employees earning below department 10’s minimum salary. |

---
