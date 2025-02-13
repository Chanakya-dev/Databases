# Fetching Data Using Joins in SQL

## **Database Schema**

### **1. Student Table**
```sql
CREATE TABLE student (
    sid INT PRIMARY KEY AUTO_INCREMENT,
    sname VARCHAR(35) NOT NULL,
    sage INT
);
```

### **2. Course Table**
```sql
CREATE TABLE Course (
    cid INT PRIMARY KEY AUTO_INCREMENT,
    cname VARCHAR(35) NOT NULL,
    cfee DOUBLE
);
```

### **3. Student-Course Relationship Table**
```sql
CREATE TABLE StCou (
    scid INT PRIMARY KEY AUTO_INCREMENT,
    fsid INT,
    fcid INT,
    FOREIGN KEY (fsid) REFERENCES student(sid) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (fcid) REFERENCES course(cid) ON DELETE CASCADE ON UPDATE CASCADE
);
```

### **4. Student Details Table**
```sql
CREATE TABLE Studentdetails (
    Sdid INT PRIMARY KEY AUTO_INCREMENT,
    Mnum VARCHAR(35),
    fsid INT,
    FOREIGN KEY (fsid) REFERENCES student(sid) ON DELETE CASCADE ON UPDATE CASCADE
);
```

## **Insert Sample Data**

### **Insert Students**
```sql
INSERT INTO student (sname, sage) VALUES ('John', 22), ('Alice', 21), ('Bob', 23);
```

### **Insert Courses**
```sql
INSERT INTO Course (cname, cfee) VALUES ('Math', 1000.00), ('Science', 1200.00), ('History', 800.00);
```

### **Insert Student Details** (One-to-Many Relationship)
```sql
INSERT INTO Studentdetails (Mnum, fsid) VALUES ('9876543210', 1), ('9876543211', 1), ('9876543212', 2), ('9876543213', 3);
```

### **Insert Student-Course Relations** (Many-to-Many Relationship)
```sql
INSERT INTO StCou (fsid, fcid) VALUES (1, 1), (1, 2), (2, 1), (2, 3), (3, 2), (3, 3);
```

---

## **Types of Joins**

### **1. INNER JOIN (Fetching Students & Their Courses)**
```sql
SELECT s.sid, s.sname, c.cname, c.cfee
FROM student s
INNER JOIN StCou sc ON s.sid = sc.fsid
INNER JOIN Course c ON sc.fcid = c.cid;
```
#### **Explanation:**
- Joins `student` and `StCou` (Many-to-Many relation table).
- Then joins `StCou` with `Course` to get course names.

---

### **2. LEFT JOIN (Fetching Students with or Without Courses)**
```sql
SELECT s.sid, s.sname, c.cname, c.cfee
FROM student s
LEFT JOIN StCou sc ON s.sid = sc.fsid
LEFT JOIN Course c ON sc.fcid = c.cid;
```
#### **Explanation:**
- Ensures all students are included.
- If a student has no course, `NULL` is displayed for `cname` & `cfee`.

---

### **3. RIGHT JOIN (Fetching Courses with or Without Students)**
```sql
SELECT s.sid, s.sname, c.cname, c.cfee
FROM student s
RIGHT JOIN StCou sc ON s.sid = sc.fsid
RIGHT JOIN Course c ON sc.fcid = c.cid;
```
#### **Explanation:**
- Ensures all `Course` records appear, even if no student is enrolled.
- If no students enrolled in a course, `NULL` appears for `sname`.

---

### **4. FULL OUTER JOIN (Fetching All Students & Courses)**
```sql
SELECT s.sid, s.sname, c.cname, c.cfee
FROM student s
FULL OUTER JOIN StCou sc ON s.sid = sc.fsid
FULL OUTER JOIN Course c ON sc.fcid = c.cid;
```
#### **Explanation:**
- Ensures **all students and all courses** appear.
- If no course is assigned, `NULL` for `cname`.
- If no student is enrolled, `NULL` for `sname`.

---

### **5. Joining StudentDetails with Student**
```sql
SELECT s.sid, s.sname, sd.Mnum
FROM student s
INNER JOIN Studentdetails sd ON s.sid = sd.fsid;
```
#### **Explanation:**
- One student can have **multiple contact numbers** (One-to-Many).

### **5. Fetching Single Student Data**
```sql
SELECT s.sid, s.sname, s.sage, c.cname, c.cfee, sd.Mnum
FROM student s
LEFT JOIN StCou sc ON s.sid = sc.fsid
LEFT JOIN Course c ON sc.fcid = c.cid
LEFT JOIN Studentdetails sd ON s.sid = sd.fsid
WHERE s.sid = 1; -- Replace 1 with the student's ID
```

#### **Explanation:**
- FFetching Entire Details of One Student

---

## **Conclusion**
- **`INNER JOIN`** → Returns only matched records.
- **`LEFT JOIN`** → Returns all left table records & matching right table records.
- **`RIGHT JOIN`** → Returns all right table records & matching left table records.
- **`FULL JOIN`** → Returns all records from both tables.

