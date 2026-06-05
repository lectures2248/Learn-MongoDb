# MongoDB CRUD Operations – Practical Lecture

---

## Today's Topics

1. `findOne()`
2. `updateOne()`
3. `updateMany()`
4. `deleteOne()`
5. `deleteMany()`

---

## Step 1: Create Database

```js
use school
```

---

## Step 2: Create Collection

```js
db.createCollection("students")
```

---

## Step 3: Insert Sample Data

```js
db.students.insertMany([
  { name: "Ali",   age: 20, city: "Karachi",   course: "MERN" },
  { name: "Ahmed", age: 22, city: "Lahore",     course: "Flutter" },
  { name: "Sara",  age: 19, city: "Islamabad",  course: "Python" },
  { name: "Hina",  age: 25, city: "Karachi",    course: "MERN" },
  { name: "Usman", age: 27, city: "Karachi",    course: "Flutter" }
])
```

### Display All Records

```js
db.students.find()
```

---

## findOne()

Returns only **one** matching document.

**Syntax:**
```js
db.students.findOne({ condition })
```

### Example 1 — Find by name
```js
db.students.findOne({ name: "Ali" })
```

### Example 2 — Find by city
```js
db.students.findOne({ city: "Karachi" })
```

### Example 3 — Find by course
```js
db.students.findOne({ course: "Flutter" })
```

### Example 4 — Find by age
```js
db.students.findOne({ age: 25 })
```

### Practice Tasks

Find:
1. Sara
2. Ahmed
3. Student from Lahore
4. Student whose age is 27

---

## updateOne()

Updates only **one** document.

**Syntax:**
```js
db.students.updateOne(
  { condition },
  { $set: { field: value } }
)
```

### Example 1 — Change Ali's age to 21
```js
db.students.updateOne(
  { name: "Ali" },
  { $set: { age: 21 } }
)
```

### Example 2 — Change Ahmed's city to Karachi
```js
db.students.updateOne(
  { name: "Ahmed" },
  { $set: { city: "Karachi" } }
)
```

### Example 3 — Change Sara's course to AI
```js
db.students.updateOne(
  { name: "Sara" },
  { $set: { course: "AI" } }
)
```

### Example 4 — Add new field: Phone
```js
db.students.updateOne(
  { name: "Ali" },
  { $set: { phone: "03001234567" } }
)
```

**Before:**
```js
{ name: "Ali", age: 21 }
```

**After:**
```js
{ name: "Ali", age: 21, phone: "03001234567" }
```

### Example 5 — Add Status field
```js
db.students.updateOne(
  { name: "Usman" },
  { $set: { status: "Active" } }
)
```

### Practice Tasks

1. Change Hina's city to Lahore
2. Change Usman's age to 30
3. Add Email field to Ahmed
4. Add Semester field to Sara

---

## updateMany()

Updates **multiple** documents.

**Syntax:**
```js
db.students.updateMany(
  { condition },
  { $set: { field: value } }
)
```

### Example 1 — Update all Karachi students
```js
db.students.updateMany(
  { city: "Karachi" },
  { $set: { country: "Pakistan" } }
)
```

### Example 2 — Update all Flutter students
```js
db.students.updateMany(
  { course: "Flutter" },
  { $set: { trainer: "Mujtaba" } }
)
```

### Example 3 — Add Status field to everyone
```js
db.students.updateMany(
  {},
  { $set: { status: "Active" } }
)
```

### Example 4 — Add Institute field to all records
```js
db.students.updateMany(
  {},
  { $set: { institute: "ABC Institute" } }
)
```

---

## $inc Operator

Used to **increase numeric values**.

### Example 1 — Increase Ali's age by 1
```js
db.students.updateOne(
  { name: "Ali" },
  { $inc: { age: 1 } }
)
```

### Example 2 — Increase all ages by 2
```js
db.students.updateMany(
  {},
  { $inc: { age: 2 } }
)
```

### Example 3 — Increase Karachi students' age by 5
```js
db.students.updateMany(
  { city: "Karachi" },
  { $inc: { age: 5 } }
)
```

### Practice Tasks

1. Increase Ahmed's age by 3
2. Increase all ages by 1
3. Increase Lahore students' age by 2

---

## deleteOne()

Deletes only **one** document.

**Syntax:**
```js
db.students.deleteOne({ condition })
```

### Example 1 — Delete Sara
```js
db.students.deleteOne({ name: "Sara" })
```

### Example 2 — Delete Ahmed
```js
db.students.deleteOne({ name: "Ahmed" })
```

### Example 3 — Delete first Karachi student
```js
db.students.deleteOne({ city: "Karachi" })
```

### Check Data
```js
db.students.find()
```

### Practice Tasks

1. Delete Hina
2. Delete Ali
3. Delete one Lahore student

---

## deleteMany()

Deletes **multiple** documents.

**Syntax:**
```js
db.students.deleteMany({ condition })
```

### Example 1 — Delete all Karachi students
```js
db.students.deleteMany({ city: "Karachi" })
```

### Example 2 — Delete all Flutter students
```js
db.students.deleteMany({ course: "Flutter" })
```

### Example 3 — Delete all students older than 25
```js
db.students.deleteMany({ age: { $gt: 25 } })
```

### Example 4 — Delete all students younger than 20
```js
db.students.deleteMany({ age: { $lt: 20 } })
```

### Example 5 — Delete all students from Lahore
```js
db.students.deleteMany({ city: "Lahore" })
```

### Delete All Documents

```js
db.students.deleteMany({})
```

> ⚠️ **WARNING:** This command removes **all documents** from the collection.

---

## Final Practical

### Insert Data
```js
db.students.insertMany([
  { name: "Ahsan", age: 20, city: "Karachi" },
  { name: "Bilal", age: 22, city: "Lahore" },
  { name: "Hamza", age: 25, city: "Karachi" },
  { name: "Saad",  age: 28, city: "Islamabad" },
  { name: "Zara",  age: 19, city: "Karachi" }
])
```

### Task 1 — Find Zara
```js
db.students.findOne({ name: "Zara" })
```

### Task 2 — Update Zara's city to Lahore
```js
db.students.updateOne(
  { name: "Zara" },
  { $set: { city: "Lahore" } }
)
```

### Task 3 — Add Status field to all
```js
db.students.updateMany(
  {},
  { $set: { status: "Active" } }
)
```

### Task 4 — Increase all ages by 1
```js
db.students.updateMany(
  {},
  { $inc: { age: 1 } }
)
```

### Task 5 — Delete all Karachi students
```js
db.students.deleteMany({ city: "Karachi" })
```

### Task 6 — View remaining data
```js
db.students.find()
```

---

> 📸 Take a **screenshot of the output** after every command.
