### **Aggregate Functions in NoSQL (MongoDB) 📌**  
---

## **1️⃣ $count** (Counts the number of documents)  
🔹 **Equivalent to SQL's `COUNT()`**  
🔹 Returns the number of documents that match a query.  

🔹 **Example:** Count the number of employees in the collection:  

```js
db.employees.aggregate([
  { $count: "total_employees" }
])
```

✅ Output:  
```json
{ "total_employees": 150 }
```

---

## **2️⃣ $sum** (Calculates the sum of values)  
🔹 **Equivalent to SQL's `SUM()`**  
🔹 Computes the sum of a numeric field.

🔹 **Example:** Find the total salary of all employees:  
```json
db.employees.aggregate([
  { $group: { _id: null, totalSalary: { $sum: "$salary" } } }
])
```
✅ Output:  
```json
{ "_id": null, "totalSalary": 500000 }
```

🔹 **Example:** Find the total salary per department:  
```json
db.employees.aggregate([
  { $group: { _id: "$department", totalSalary: { $sum: "$salary" } } }
])
```
✅ Output:  
```json
[
  { "_id": "IT", "totalSalary": 200000 },
  { "_id": "HR", "totalSalary": 100000 }
]
```

---

## **3️⃣ $avg** (Calculates the average value)  
🔹 **Equivalent to SQL's `AVG()`**  
🔹 Computes the average of a numeric field.

🔹 **Example:** Find the average salary per department:  
```json
db.employees.aggregate([
  { $group: { _id: "$department", avgSalary: { $avg: "$salary" } } }
])
```
✅ Output:  
```json
[
  { "_id": "IT", "avgSalary": 75000 },
  { "_id": "HR", "avgSalary": 50000 }
]
```

---

## **4️⃣ $min** (Finds the minimum value)  
🔹 **Equivalent to SQL's `MIN()`**  
🔹 Returns the smallest value in a field.

🔹 **Example:** Find the minimum salary per department:  
```json
db.employees.aggregate([
  { $group: { _id: "$department", minSalary: { $min: "$salary" } } }
])
```
✅ Output:  
```json
[
  { "_id": "IT", "minSalary": 50000 },
  { "_id": "HR", "minSalary": 40000 }
]
```

---

## **5️⃣ $max** (Finds the maximum value)  
🔹 **Equivalent to SQL's `MAX()`**  
🔹 Returns the largest value in a field.

🔹 **Example:** Find the maximum salary per department:  
```json
db.employees.aggregate([
  { $group: { _id: "$department", maxSalary: { $max: "$salary" } } }
])
```
✅ Output:  
```json
[
  { "_id": "IT", "maxSalary": 100000 },
  { "_id": "HR", "maxSalary": 60000 }
]
```

---

# **Grouping with Aggregation in NoSQL ($group)**
The `$group` stage is used to perform aggregation **based on a specific field**.  

🔹 **Example: Count employees in each department:**  
```json
db.employees.aggregate([
  { $group: { _id: "$department", employeeCount: { $sum: 1 } } }
])
```
✅ Output:  
```json
[
  { "_id": "IT", "employeeCount": 5 },
  { "_id": "HR", "employeeCount": 3 }
]
```

---

# **Filtering Aggregated Data ($match vs. $having in SQL)**
- In SQL, `HAVING` filters aggregated data.  
- In NoSQL, `$match` is used **before** grouping, and `$match` after `$group` works like `HAVING`.  

🔹 **Example: Find departments with more than 3 employees:**  
```json
db.employees.aggregate([
  { $group: { _id: "$department", employeeCount: { $sum: 1 } } },
  { $match: { employeeCount: { $gt: 3 } } }
])
```

✅ Output:  
```json
[
  { "_id": "IT", "employeeCount": 5 }
]
```

---

# **Key Takeaways 🚀**
✔ MongoDB’s **Aggregation Framework** replaces SQL’s aggregate functions.  
✔ `$group` is like `GROUP BY`, `$match` works like `WHERE` and `HAVING`.  
✔ Aggregations can work on **nested fields** and **arrays**.  
✔ Useful for data analytics and summarizing information efficiently.  
