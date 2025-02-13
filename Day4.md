Here’s how you can structure the equivalent of SQL table creation and data insertion in **MongoDB** using **collections** and `insertMany()` functions.

---

## **1. Creating Collections and Inserting Data**

### **1.1 Student Collection**
Equivalent to the SQL `student` table.
```js
db.Student.insertMany([
    { _id: ObjectId("656b1f9a1c4d3a1b4c8a7a01"), name: "John", age: 22 },
    { _id: ObjectId("656b1f9a1c4d3a1b4c8a7a02"), name: "Alice", age: 21 },
    { _id: ObjectId("656b1f9a1c4d3a1b4c8a7a03"), name: "Bob", age: 23 }
]);
```

---

### **1.2 Course Collection**
Equivalent to the SQL `Course` table.
```js
db.Course.insertMany([
    { _id: ObjectId("656b1f9a1c4d3a1b4c8b7b01"), name: "Math", fee: 1000.00 },
    { _id: ObjectId("656b1f9a1c4d3a1b4c8b7b02"), name: "Science", fee: 1200.00 },
    { _id: ObjectId("656b1f9a1c4d3a1b4c8b7b03"), name: "History", fee: 800.00 }
]);
```

---

### **1.3 Student Details Collection** (One-to-Many Relationship)
Equivalent to the SQL `Studentdetails` table.
```js
db.StudentDetails.insertMany([
    { _id: ObjectId("656b1f9a1c4d3a1b4c8c7c01"), student_id: ObjectId("656b1f9a1c4d3a1b4c8a7a01"), mobile_number: "9876543210" },
    { _id: ObjectId("656b1f9a1c4d3a1b4c8c7c02"), student_id: ObjectId("656b1f9a1c4d3a1b4c8a7a01"), mobile_number: "9876543211" },
    { _id: ObjectId("656b1f9a1c4d3a1b4c8c7c03"), student_id: ObjectId("656b1f9a1c4d3a1b4c8a7a02"), mobile_number: "9876543212" },
    { _id: ObjectId("656b1f9a1c4d3a1b4c8c7c04"), student_id: ObjectId("656b1f9a1c4d3a1b4c8a7a03"), mobile_number: "9876543213" }
]);
```

---

### **1.4 Student-Course Relationship Collection** (Many-to-Many Relationship)
Equivalent to the SQL `StCou` table.
```js
db.StudentCourse.insertMany([
    { _id: ObjectId("656b1f9a1c4d3a1b4c8d7d01"), student_id: ObjectId("656b1f9a1c4d3a1b4c8a7a01"), course_id: ObjectId("656b1f9a1c4d3a1b4c8b7b01") },
    { _id: ObjectId("656b1f9a1c4d3a1b4c8d7d02"), student_id: ObjectId("656b1f9a1c4d3a1b4c8a7a01"), course_id: ObjectId("656b1f9a1c4d3a1b4c8b7b02") },
    { _id: ObjectId("656b1f9a1c4d3a1b4c8d7d03"), student_id: ObjectId("656b1f9a1c4d3a1b4c8a7a02"), course_id: ObjectId("656b1f9a1c4d3a1b4c8b7b01") },
    { _id: ObjectId("656b1f9a1c4d3a1b4c8d7d04"), student_id: ObjectId("656b1f9a1c4d3a1b4c8a7a02"), course_id: ObjectId("656b1f9a1c4d3a1b4c8b7b03") },
    { _id: ObjectId("656b1f9a1c4d3a1b4c8d7d05"), student_id: ObjectId("656b1f9a1c4d3a1b4c8a7a03"), course_id: ObjectId("656b1f9a1c4d3a1b4c8b7b02") },
    { _id: ObjectId("656b1f9a1c4d3a1b4c8d7d06"), student_id: ObjectId("656b1f9a1c4d3a1b4c8a7a03"), course_id: ObjectId("656b1f9a1c4d3a1b4c8b7b03") }
]);
```

---

## **2. Fetching Data Using MongoDB Joins (Aggregation)**

### **2.1 INNER JOIN (Fetching Students & Their Courses)**
```js
db.StudentCourse.aggregate([
    {
        $lookup: {
            from: "Student",
            localField: "student_id",
            foreignField: "_id",
            as: "student"
        }
    },
    { $unwind: "$student" },
    {
        $lookup: {
            from: "Course",
            localField: "course_id",
            foreignField: "_id",
            as: "course"
        }
    },
    { $unwind: "$course" },
    {
        $project: {
            _id: 0,
            "student.name": 1,
            "course.name": 1,
            "course.fee": 1
        }
    }
]);
```
#### **Equivalent SQL Query:**
```sql
SELECT s.sname, c.cname, c.cfee
FROM student s
INNER JOIN StCou sc ON s.sid = sc.fsid
INNER JOIN Course c ON sc.fcid = c.cid;
```

---

### **2.2 LEFT JOIN (Fetching Students with or Without Courses)**
```js
db.Student.aggregate([
    {
        $lookup: {
            from: "StudentCourse",
            localField: "_id",
            foreignField: "student_id",
            as: "enrolled_courses"
        }
    },
    {
        $lookup: {
            from: "Course",
            localField: "enrolled_courses.course_id",
            foreignField: "_id",
            as: "courses"
        }
    },
    {
        $project: {
            _id: 0,
            name: 1,
            courses: { name: 1, fee: 1 }
        }
    }
]);
```
#### **Equivalent SQL Query:**
```sql
SELECT s.sname, c.cname, c.cfee
FROM student s
LEFT JOIN StCou sc ON s.sid = sc.fsid
LEFT JOIN Course c ON sc.fcid = c.cid;
```

---

### **2.3 RIGHT JOIN (Fetching Courses with or Without Students)**
```js
db.Course.aggregate([
    {
        $lookup: {
            from: "StudentCourse",
            localField: "_id",
            foreignField: "course_id",
            as: "enrolled_students"
        }
    },
    {
        $lookup: {
            from: "Student",
            localField: "enrolled_students.student_id",
            foreignField: "_id",
            as: "students"
        }
    },
    {
        $project: {
            _id: 0,
            name: 1,
            students: { name: 1, age: 1 }
        }
    }
]);
```
#### **Equivalent SQL Query:**
```sql
SELECT c.cname, s.sname, s.sage
FROM Course c
RIGHT JOIN StCou sc ON c.cid = sc.fcid
RIGHT JOIN student s ON sc.fsid = s.sid;
```

---
