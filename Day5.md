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
Find students who have the same age as at least one student from course ID 1.
```sql
SELECT sname, sage
FROM student
WHERE sage = (SELECT sage FROM student WHERE sid IN (SELECT fsid FROM StCou WHERE fcid = 1));
```
#### Example 2: Using `IN`
Find students who have any age matching a student from course ID 1.
```sql
SELECT sname, sage
FROM student
WHERE sage IN (SELECT sage FROM student WHERE sid IN (SELECT fsid FROM StCou WHERE fcid = 1));
```
#### Example 3: Using `ANY`
Find students who are **younger than the oldest** student in course ID 1.
```sql
SELECT sname, sage
FROM student
WHERE sage < ANY (SELECT sage FROM student WHERE sid IN (SELECT fsid FROM StCou WHERE fcid = 1));
```
#### Example 4: Using `ALL`
Find students who are **younger than the youngest** student in course ID 1.
```sql
SELECT sname, sage
FROM student
WHERE sage < ALL (SELECT sage FROM student WHERE sid IN (SELECT fsid FROM StCou WHERE fcid = 1));
```
#### Example 5: Using `EXISTS`
Find students **only if course ID 1 has students enrolled**.
```sql
SELECT sname, sage
FROM student
WHERE EXISTS (SELECT 1 FROM StCou WHERE fcid = 1);
```
#### Example 6: Using `NOT EXISTS`
Find students **only if course ID 1 has no students enrolled**.
```sql
SELECT sname, sage
FROM student
WHERE NOT EXISTS (SELECT 1 FROM StCou WHERE fcid = 1);
```

---
### **2. INSERT with Subqueries**
Insert students who have enrolled in at least one course into a new table.
```sql
INSERT INTO enrolled_students (sid, sname, sage)
SELECT sid, sname, sage FROM student WHERE sid IN (SELECT DISTINCT fsid FROM StCou);
```

---
### **3. UPDATE with Subqueries**
Increase course fee by 10% for courses where the fee is below the average course fee.
```sql
UPDATE Course
SET cfee = cfee * 1.10
WHERE cfee < (SELECT AVG(cfee) FROM Course);
```

---
### **4. DELETE with Subqueries**
Delete students who are enrolled in the least expensive course.
```sql
DELETE FROM student
WHERE sid IN (SELECT fsid FROM StCou WHERE fcid = (SELECT cid FROM Course ORDER BY cfee ASC LIMIT 1));
```

---
## **Summary Table of Subquery Operations**
| Operation | Example |
|-----------|------------|
| `=` | Find students of the same age as a student in course ID 1. |
| `IN` | Find students with any age matching a student in course ID 1. |
| `ANY` | Find students younger than at least one student in course ID 1. |
| `ALL` | Find students younger than the youngest student in course ID 1. |
| `EXISTS` | Return students if course ID 1 has students. |
| `NOT EXISTS` | Return students if course ID 1 has no students. |
| `INSERT` | Insert students enrolled in at least one course. |
| `UPDATE` | Increase course fee for courses below the average fee. |
| `DELETE` | Remove students enrolled in the cheapest course. |

---

