
# 📘 MongoDB & Mongoose Notes

## 1. What is MongoDB?
- **Type**: NoSQL Database (document-oriented).  
- **Data Format**: Stores data in **BSON** (Binary JSON).  
- **Key Features**:
  - Schema-less (flexible structure).  
  - Stores data as **collections** (like tables in SQL).  
  - Each collection has **documents** (like rows).  
  - Each document is a **JSON-like object** (key-value pairs).  

👉 Example MongoDB Document:
\`\`\`json
{
  "_id": "64abc123",
  "name": "Sumit",
  "age": 21,
  "skills": ["JavaScript", "React", "Node.js"]
}
\`\`\`

---

## 2. MongoDB Basics
- **Database** → Holds collections.  
- **Collection** → Holds documents.  
- **Document** → Key-value pairs (JSON-like).  

🔑 **Common Commands** (in Mongo shell):
\`\`\`bash
show dbs               # Show databases
use myDatabase         # Switch/create database
db.createCollection("users")   # Create collection
db.users.insertOne({ name: "Sumit", age: 21 })   # Insert document
db.users.find()        # Get all documents
db.users.findOne({ name: "Sumit" })  # Query single doc
db.users.updateOne({ name: "Sumit" }, { $set: { age: 22 } }) # Update
db.users.deleteOne({ name: "Sumit" }) # Delete
\`\`\`

---

## 3. What is Mongoose?
- **ODM (Object Data Modeling) library** for MongoDB in Node.js.  
- Helps define **schemas & models** for structured data.  
- Provides validation, query building, middleware.  

👉 Why use Mongoose?
- MongoDB is schema-less → Mongoose adds **schema & structure**.  
- Easier CRUD operations.  
- Supports middlewares, hooks, validation.  

---

## 4. Mongoose Basics

### 🔹 Connect to MongoDB
\`\`\`js
import mongoose from "mongoose";

mongoose.connect("mongodb://localhost:27017/myDatabase")
  .then(() => console.log("MongoDB Connected"))
  .catch(err => console.log(err));
\`\`\`

### 🔹 Define Schema
\`\`\`js
const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  age: Number,
  email: { type: String, unique: true },
  createdAt: { type: Date, default: Date.now }
});
\`\`\`

### 🔹 Create Model
\`\`\`js
const User = mongoose.model("User", userSchema);
\`\`\`

### 🔹 CRUD Operations
\`\`\`js
// Create
await User.create({ name: "Sumit", age: 21, email: "sumit@example.com" });

// Read
const users = await User.find(); 
const user = await User.findOne({ name: "Sumit" });

// Update
await User.updateOne({ name: "Sumit" }, { age: 22 });

// Delete
await User.deleteOne({ name: "Sumit" });
\`\`\`

---

## 5. Mongoose Features
- **Validation**: Enforce rules on data (e.g., required, unique).  
- **Middleware (Hooks)**: Functions that run before/after save, update, delete.  
- **Population**: Join data across collections.  
- **Schema Methods**: Add custom methods to documents.  

👉 Example Schema with Methods:
\`\`\`js
userSchema.methods.greet = function () {
  return \`Hello, my name is \${this.name}\`;
};

const User = mongoose.model("User", userSchema);
const sumit = await User.findOne({ name: "Sumit" });
console.log(sumit.greet()); // "Hello, my name is Sumit"
\`\`\`

---

## 6. MongoDB vs Mongoose

| Feature        | MongoDB (Native Driver) | Mongoose |
|----------------|--------------------------|----------|
| Schema         | Flexible (no schema)    | Schema enforced |
| Validation     | Manual                  | Built-in |
| Data Modeling  | Not provided            | Provided |
| Ease of Use    | Low-level               | High-level API |
| Population     | Manual \`$lookup\`        | Easy \`.populate()\` |

---

✅ **Summary**:  
- **MongoDB** = Database (raw storage).  
- **Mongoose** = Abstraction layer for structure, validation, and easy CRUD.  
