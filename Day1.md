### **Creating a MongoDB Collection Similar to Your MySQL Table**

---

### **Equivalent MongoDB Command**
```javascript
db.createCollection("employees", {
    validator: {
        $jsonSchema: {
            bsonType: "object",
            required: ["name", "email", "age"],  // Equivalent to NOT NULL in MySQL
            properties: {
                id: { bsonType: "int", description: "Primary key (manually handled)" },
                name: { bsonType: "string", maxLength: 35, description: "Name (max 35 chars, NOT NULL)" },
                email: { bsonType: "string", maxLength: 100, description: "Unique email" },
                age: { bsonType: "int", description: "Age" }
            }
        }
    }
});
```

---

### **Verifying the Collection and Data**
- **Check Collections:**
  ```javascript
  show collections;
  ```

### **Inserting Data**

```javascript
db.employees.insertOne({
    name: "Chanakya",
    email: "Manas9391133039@gmail.com",
    age: 23
});
```

This will insert a single document into the `employees` collection. 🚀
- **Retrieve Data:**
  ```javascript
  db.employees.find().pretty();
  ```
### **Update Data**
```javascript
db.employees.updateOne(
    { email: "Manas9391133039@gmail.com" },  
    { $set: { age: 32 } }
);
```

### **Delete Data**
```javascript
db.employees.deleteOne({ email: "aryabhata@example.com" });
```
---
