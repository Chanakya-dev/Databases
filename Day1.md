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

### **Key Differences from MySQL**
1. **No `AUTO_INCREMENT` in MongoDB**
   - You need to **manually** handle `id` auto-increment (shown below).
  
2. **Unique `email` Constraint Must Be Added Separately**
   ```javascript
   db.employees.createIndex({ email: 1 }, { unique: true });
   ```

3. **MongoDB Uses `string`, Not `VARCHAR`**
   - `VARCHAR(35)` in MySQL → Becomes `bsonType: "string", maxLength: 35`.

---

### **Manually Handling `AUTO_INCREMENT` for `id`**
Since MongoDB does **not** have `AUTO_INCREMENT`, you must handle it using a **counter collection**.

1️⃣ **Create a Counter Collection:**
```javascript
db.counters.insertOne({ _id: "employee_id", seq: 0 });
```

2️⃣ **Define an Auto-Increment Function:**
```javascript
function getNextSequence(name) {
    var counter = db.counters.findOneAndUpdate(
        { _id: name },
        { $inc: { seq: 1 } },
        { returnNewDocument: true }
    );
    return counter.seq;
}
```

3️⃣ **Insert Data Using Auto-Increment ID:**
```javascript
db.employees.insertOne({
    id: getNextSequence("employee_id"),
    name: "Alice",
    email: "alice@example.com",
    age: 28
});
```

---

### **Verifying the Collection and Data**
- **Check Collections:**
  ```javascript
  show collections;
  ```
- **Retrieve Data:**
  ```javascript
  db.employees.find().pretty();
  ```

---
