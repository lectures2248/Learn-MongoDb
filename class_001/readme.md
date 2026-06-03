# Lecture 1 – MongoDB Introduction

---

## 1. Introduction to MongoDB

MongoDB is a **NoSQL database** used to store and manage data in a flexible, document-based format.

Instead of storing data in tables (rows and columns like SQL), MongoDB stores data in JSON-like documents called **BSON documents**.

MongoDB is widely used in modern web applications, mobile apps, and real-time systems because it is **fast, scalable, and flexible**.

- Developed by: **MongoDB Inc.**
- Official website: [https://www.mongodb.com](https://www.mongodb.com)

---

## 2. What is a Database?

A database is a collection of organized data that can be easily accessed, managed, and updated.

**Examples:** Student records, employee records, banking data.

---

## 3. What is NoSQL?

NoSQL stands for **"Not Only SQL"**.

It is a type of database that does not use traditional table structure.

**Key points of NoSQL:**
- Does not use tables like SQL
- Stores data in flexible formats (documents, key-value, graphs, etc.)
- Easily handles large amounts of unstructured data
- Highly scalable and fast

> MongoDB is a NoSQL database.

---

## 4. SQL vs NoSQL Comparison

| Feature       | SQL Database                          | NoSQL Database                        |
|---------------|---------------------------------------|---------------------------------------|
| Data Storage  | Stored in tables                      | Stored in collections and documents   |
| Schema        | Fixed schema (defined first)          | Flexible schema (no strict structure) |
| Format        | Rows and columns                      | JSON-like documents                   |
| Examples      | MySQL, SQL Server, Oracle             | MongoDB                               |
| Best For      | Structured data                       | Large and dynamic data                |

---

## 5. BSON (Binary JSON)

MongoDB stores data internally in **BSON** format.

BSON stands for **Binary JSON** — a binary representation of JSON documents.

**Advantages of BSON:**
- Faster read and write operations
- Supports more data types than JSON (like `Date`, `ObjectId`)
- Efficient storage and processing

**Example JSON:**
```json
{
  "name": "Ali",
  "age": 20
}
```

> MongoDB stores this internally as BSON.

---

## 6. MongoDB Installation

### Step 1: Install MongoDB Server

Download MongoDB Community Server:
[https://www.mongodb.com/try/download/community](https://www.mongodb.com/try/download/community)

This installs:
- MongoDB Database Server
- MongoDB Shell (`mongosh`)

### Step 2: Install MongoDB Compass

Download MongoDB Compass:
[https://www.mongodb.com/products/tools/compass](https://www.mongodb.com/products/tools/compass)

MongoDB Compass is a **GUI tool** used to:
- View databases visually
- Insert and update data easily
- Run queries without command line

---

## 7. MongoDB Components

MongoDB has the following main components:

| Component      | Description                                      |
|----------------|--------------------------------------------------|
| **Database**   | A container for collections                      |
| **Collection** | A group of documents (like a table in SQL)       |
| **Document**   | A single record stored in JSON-like format       |

**Example Structure:**

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

## 8. Opening MongoDB Shell

Open Command Prompt and type:

```
mongosh
```

This will open the MongoDB interactive shell.

---

## 9. Basic MongoDB Commands

### Step 1: Check Available Databases

```js
show dbs
```

Shows all existing databases.

---

### Step 2: Create or Switch Database

```js
use school
```

> If database `school` does not exist, MongoDB will create it automatically when data is inserted.

---

### Step 3: Check Current Database

```js
db
```

Shows the currently selected database.

---

### Step 4: Create Collection

```js
db.createCollection("students")
```

Creates a collection named `students` inside the current database.

---

### Step 5: Insert Single Document

```js
db.students.insertOne({
  name: "Ali",
  age: 20,
  city: "Karachi"
})
```

Inserts one record into the `students` collection.

---

### Step 6: Insert Multiple Documents

```js
db.students.insertMany([
  { name: "Sara",  age: 21, city: "Lahore" },
  { name: "Ahmed", age: 22, city: "Islamabad" },
  { name: "Zain",  age: 23, city: "Peshawar" }
])
```

---

### Step 7: View All Data

```js
db.students.find()
```

To make output readable:

```js
db.students.find().pretty()
```

---

### Step 8: Count Documents

```js
db.students.countDocuments()
```

Returns the total number of records in the collection.

---
