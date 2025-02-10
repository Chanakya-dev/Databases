
## 1. Create Database

```sql
CREATE DATABASE company_db;
```

## 2. Use the Database
```sql
USE company_db;
```

## 3. Create Employee Table
```sql
CREATE TABLE employee (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(35) NOT NULL,
    email VARCHAR(100) UNIQUE,
    age INT
);
```

## 4. Insert Data into Employee Table
```sql
INSERT INTO employee (name, email, age) 
VALUES ("Chanakya", "Manas9391133039@gmail.com", 23);
```

## 5. Retrieve All Records
```sql
SELECT * FROM employee;
```

## 6. Update Employee Age
```sql
UPDATE employee 
SET age = 32 
WHERE id = 2;
```

## 7. Delete an Employee Record
```sql
DELETE FROM employee 
WHERE id = 2;
```

---
