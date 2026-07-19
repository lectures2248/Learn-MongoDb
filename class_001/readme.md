# Introduction to MongoDB
---

### 1. What is MongoDB?

MongoDB is a NoSQL database used to store and manage data in a flexible, document-based format.

Think about it this way: instead of storing data in rigid tables made of rows and columns like SQL databases do, MongoDB stores everything as JSON-like documents called **BSON documents**. This gives you a lot more freedom in how you structure your data.

MongoDB is widely used today in web applications, mobile apps, and real-time systems, mainly because it is fast, scalable, and flexible enough to handle constantly changing data.

- **Developed by:** MongoDB Inc.
- **Official website:** https://www.mongodb.com

---

### 2. What is a Database?

A database is simply a collection of organized data that can be easily accessed, managed, and updated whenever needed.

**Everyday examples:**
- Student records
- Employee records
- Banking data

---

### 3. What is NoSQL?

NoSQL stands for **"Not Only SQL."**

It is a category of database that does not rely on the traditional table structure that SQL databases use.

**Key characteristics of NoSQL:**
- Does not use tables like SQL does
- Stores data in flexible formats — documents, key-value pairs, graphs, and more
- Handles large amounts of unstructured data with ease
- Highly scalable and fast

MongoDB falls under this NoSQL category.

---

### 4. SQL vs NoSQL — A Quick Comparison

| Feature | SQL Database | NoSQL Database |
|---|---|---|
| Data Storage | Stored in tables | Stored in collections and documents |
| Schema | Fixed schema (defined upfront) | Flexible schema (no strict structure) |
| Format | Rows and columns | JSON-like documents |
| Examples | MySQL, SQL Server, Oracle | MongoDB |
| Best For | Structured data | Large and dynamic data |

---

### 5. Understanding BSON (Binary JSON)

MongoDB stores data internally in a format called BSON.

**BSON** stands for **Binary JSON** — essentially a binary representation of JSON documents.

**Why BSON is useful:**
- Faster read and write operations
- Supports more data types than plain JSON, such as `Date` and `ObjectId`
- More efficient storage and processing overall

**A simple JSON example:**

```json
{
  "name": "Ali",
  "age": 20
}
```

Behind the scenes, MongoDB converts and stores this as BSON.

---

### 6. Installing MongoDB

**Step 1 — Install MongoDB Server**

Download the MongoDB Community Server from:
https://www.mongodb.com/try/download/community

This installation gives you two things:
- The MongoDB Database Server
- The MongoDB Shell (`mongosh`)

**Step 2 — Install MongoDB Compass**

Download MongoDB Compass from:
https://www.mongodb.com/products/tools/compass

MongoDB Compass is a GUI (graphical) tool that lets you:
- View your databases visually, without typing commands
- Insert and update data through a simple interface
- Run queries directly from the interface

---

### 7. Core Components of MongoDB

| Component | Description |
|---|---|
| Database | A container that holds collections |
| Collection | A group of documents (similar to a table in SQL) |
| Document | A single record, stored in JSON-like format |

**Example structure:**

```
Database:   school
Collection: students
Document:
{
  name: "Ali",
  age: 20,
  city: "Karachi"
}
```

---

### 8. Opening the MongoDB Shell

Open your Command Prompt and type:

```bash
mongosh
```

This opens the MongoDB interactive shell where you can start running commands directly.

---

### 9. Basic MongoDB Commands

**Step 1 — Check available databases**

```javascript
show dbs
```

This displays all the databases that currently exist.

**Step 2 — Create or switch to a database**

```javascript
use school
```

If a database called `school` doesn't already exist, MongoDB will automatically create it the moment you insert data into it. No extra setup step needed.

**Step 3 — Check the current database**

```javascript
db
```

This shows you which database is currently selected.

**Step 4 — Create a collection**

```javascript
db.createCollection("students")
```

This creates a collection named `students` inside the current database.

**Step 5 — Insert a single document**

```javascript
db.students.insertOne({
  name: "Ali",
  age: 20,
  city: "Karachi"
})
```

This inserts one record into the `students` collection.

**Step 6 — Insert multiple documents**

```javascript
db.students.insertMany([
  { name: "Sara",  age: 21, city: "Lahore" },
  { name: "Ahmed", age: 22, city: "Islamabad" },
  { name: "Zain",  age: 23, city: "Peshawar" }
])
```

**Step 7 — View all data**

```javascript
db.students.find()
```

To make the output easier to read:

```javascript
db.students.find().pretty()
```

**Step 8 — Count documents**

```javascript
db.students.countDocuments()
```

This returns the total number of records currently in the collection.

---
---

## MongoDB CRUD Operations — Practical 

### Today's Topics

- `findOne()`
- `updateOne()`
- `updateMany()`
- `deleteOne()`
- `deleteMany()`

---

### Step 1 — Create the Database

```javascript
use school
```

### Step 2 — Create the Collection

```javascript
db.createCollection("students")
```

### Step 3 — Insert Sample Data

```javascript
db.students.insertMany([
  { name: "Ali",   age: 20, city: "Karachi",   course: "MERN" },
  { name: "Ahmed", age: 22, city: "Lahore",     course: "Flutter" },
  { name: "Sara",  age: 19, city: "Islamabad",  course: "Python" },
  { name: "Hina",  age: 25, city: "Karachi",    course: "MERN" },
  { name: "Usman", age: 27, city: "Karachi",    course: "Flutter" }
])
```

**Display all records:**

```javascript
db.students.find()
```

---

### `findOne()`

This returns only **one** matching document, even if multiple documents match the condition.

**Syntax:**

```javascript
db.students.findOne({ condition })
```

**Example 1 — Find by name**

```javascript
db.students.findOne({ name: "Ali" })
```

**Example 2 — Find by city**

```javascript
db.students.findOne({ city: "Karachi" })
```

**Example 3 — Find by course**

```javascript
db.students.findOne({ course: "Flutter" })
```

**Example 4 — Find by age**

```javascript
db.students.findOne({ age: 25 })
```

**Practice Tasks:**
1. Find Sara
2. Find Ahmed
3. Find the student from Lahore
4. Find the student whose age is 27

---

### `updateOne()`

This updates only **one** document that matches the given condition.

**Syntax:**

```javascript
db.students.updateOne(
  { condition },
  { $set: { field: value } }
)
```

**Example 1 — Change Ali's age to 21**

```javascript
db.students.updateOne(
  { name: "Ali" },
  { $set: { age: 21 } }
)
```

**Example 2 — Change Ahmed's city to Karachi**

```javascript
db.students.updateOne(
  { name: "Ahmed" },
  { $set: { city: "Karachi" } }
)
```

**Example 3 — Change Sara's course to AI**

```javascript
db.students.updateOne(
  { name: "Sara" },
  { $set: { course: "AI" } }
)
```

**Example 4 — Add a new field: phone**

```javascript
db.students.updateOne(
  { name: "Ali" },
  { $set: { phone: "03001234567" } }
)
```

Before:
```javascript
{ name: "Ali", age: 21 }
```

After:
```javascript
{ name: "Ali", age: 21, phone: "03001234567" }
```

Notice how `$set` doesn't just update existing fields — it can also add a brand new field to a document that didn't have it before.

**Example 5 — Add a status field**

```javascript
db.students.updateOne(
  { name: "Usman" },
  { $set: { status: "Active" } }
)
```

**Practice Tasks:**
1. Change Hina's city to Lahore
2. Change Usman's age to 30
3. Add an email field to Ahmed
4. Add a semester field to Sara

---

### `updateMany()`

This updates **multiple** documents at once — every document that matches the condition gets updated.

**Syntax:**

```javascript
db.students.updateMany(
  { condition },
  { $set: { field: value } }
)
```

**Example 1 — Update all Karachi students**

```javascript
db.students.updateMany(
  { city: "Karachi" },
  { $set: { country: "Pakistan" } }
)
```

**Example 2 — Update all Flutter students**

```javascript
db.students.updateMany(
  { course: "Flutter" },
  { $set: { trainer: "Mujtaba" } }
)
```

**Example 3 — Add a status field to everyone**

```javascript
db.students.updateMany(
  {},
  { $set: { status: "Active" } }
)
```

An empty condition `{}` means "match everything" — so this applies to every document in the collection.

**Example 4 — Add an institute field to all records**

```javascript
db.students.updateMany(
  {},
  { $set: { institute: "ABC Institute" } }
)
```

---

### The `$inc` Operator

`$inc` is used to increase (or decrease) numeric values in a document.

**Example 1 — Increase Ali's age by 1**

```javascript
db.students.updateOne(
  { name: "Ali" },
  { $inc: { age: 1 } }
)
```

**Example 2 — Increase everyone's age by 2**

```javascript
db.students.updateMany(
  {},
  { $inc: { age: 2 } }
)
```

**Example 3 — Increase Karachi students' age by 5**

```javascript
db.students.updateMany(
  { city: "Karachi" },
  { $inc: { age: 5 } }
)
```

**Practice Tasks:**
1. Increase Ahmed's age by 3
2. Increase everyone's age by 1
3. Increase Lahore students' age by 2

---

### `deleteOne()`

This deletes only **one** document that matches the condition.

**Syntax:**

```javascript
db.students.deleteOne({ condition })
```

**Example 1 — Delete Sara**

```javascript
db.students.deleteOne({ name: "Sara" })
```

**Example 2 — Delete Ahmed**

```javascript
db.students.deleteOne({ name: "Ahmed" })
```

**Example 3 — Delete the first Karachi student found**

```javascript
db.students.deleteOne({ city: "Karachi" })
```

**Check remaining data:**

```javascript
db.students.find()
```

**Practice Tasks:**
1. Delete Hina
2. Delete Ali
3. Delete one Lahore student

---

### `deleteMany()`

This deletes **multiple** documents at once — every document matching the condition gets removed.

**Syntax:**

```javascript
db.students.deleteMany({ condition })
```

**Example 1 — Delete all Karachi students**

```javascript
db.students.deleteMany({ city: "Karachi" })
```

**Example 2 — Delete all Flutter students**

```javascript
db.students.deleteMany({ course: "Flutter" })
```

**Example 3 — Delete all students older than 25**

```javascript
db.students.deleteMany({ age: { $gt: 25 } })
```

**Example 4 — Delete all students younger than 20**

```javascript
db.students.deleteMany({ age: { $lt: 20 } })
```

**Example 5 — Delete all students from Lahore**

```javascript
db.students.deleteMany({ city: "Lahore" })
```

**Delete all documents in the collection:**

```javascript
db.students.deleteMany({})
```

> **Warning:** This command removes every single document from the collection. Use it carefully, since there's no undo.

---

##  Exercises

**Insert Data**

```javascript
db.students.insertMany([
  { name: "Ahsan", age: 20, city: "Karachi" },
  { name: "Bilal", age: 22, city: "Lahore" },
  { name: "Hamza", age: 25, city: "Karachi" },
  { name: "Saad",  age: 28, city: "Islamabad" },
  { name: "Zara",  age: 19, city: "Karachi" }
])
```

**Task 1 — Find Zara**

```javascript
db.students.findOne({ name: "Zara" })
```

**Task 2 — Update Zara's city to Lahore**

```javascript
db.students.updateOne(
  { name: "Zara" },
  { $set: { city: "Lahore" } }
)
```

**Task 3 — Add a status field to all documents**

```javascript
db.students.updateMany(
  {},
  { $set: { status: "Active" } }
)
```

**Task 4 — Increase everyone's age by 1**

```javascript
db.students.updateMany(
  {},
  { $inc: { age: 1 } }
)
```

**Task 5 — Delete all Karachi students**

```javascript
db.students.deleteMany({ city: "Karachi" })
```

**Task 6 — View the remaining data**

```javascript
db.students.find()
```

---
