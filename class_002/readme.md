# Logical Operators

---

## 1. What Are Logical Operators?

When you query a MongoDB collection, sometimes a single condition isn't enough. You might need to check two, three, or more conditions together — and decide whether **all** of them should match, or just **one**, or **none** at all.

That's exactly what logical operators are for. They let you combine multiple conditions inside a query, the same way `AND`, `OR`, and `NOT` work in everyday logic or in SQL.

**The four logical operators we'll cover:**

| Operator | Meaning |
|---|---|
| `$and` | Selects documents that satisfy **all** the specified conditions |
| `$or` | Selects documents that satisfy **at least one** of the specified conditions |
| `$not` | Selects documents that do **not** match the specified condition |
| `$nor` | Selects documents that do **not** match **any** of the specified conditions |

Think of them as tools for asking more precise questions from your data — instead of one simple filter, you're now able to combine conditions the way you'd naturally think about a problem.

---

## 2. Setting Up Sample Data

Before we go through each operator, let's set up an `employee` collection so every example has real data to run against.

**Step 1 — Create the collection**

```javascript
db.createCollection("employee")
```

**Step 2 — Insert a single document**

```javascript
db.employee.insertOne({ name: "Ahmed", age: 18, address: "karachi" })
```

**Step 3 — View the data**

```javascript
db.employee.find()
```

**Step 4 — Find and update in one step**

`findOneAndUpdate()` is a useful command that finds a matching document, updates it, and returns the original document as its result — all in a single call.

```javascript
db.employee.findOneAndUpdate(
  { name: "Talha" },
  { $set: { name: "Danish" } }
)
```

Since no employee named "Talha" exists yet at this point, this command won't find a match. We'll insert one in the next step, and you're welcome to try this again afterward.

**Step 5 — Insert a bigger dataset**

This is the dataset we'll use for every logical operator example below.

```javascript
db.employee.insertMany([
  { name: "Ali",     age: 28, department: "HR" },
  { name: "Talha",   age: 35, department: "IT" },
  { name: "Muzamil", age: 25, department: "Finance" },
  { name: "Ahmed",   age: 30, department: "HR" },
  { name: "Zain",    age: 22, department: "IT" }
])
```

Keep this dataset in mind as you go through the examples — it'll help you predict what each query should return before you even run it.

---

## 3. `$and` Operator

`$and` returns documents that satisfy **every single condition** you list. If even one condition fails, the document is excluded.

**Syntax:**

```javascript
db.collection.find({
  $and: [
    { condition1 },
    { condition2 }
  ]
})
```

**Example — Find employees older than 25 who work in the HR department**

```javascript
db.employee.find({
  $and: [
    { age: { $gt: 25 } },
    { department: "HR" }
  ]
})
```

**How to think about it:** Both conditions must be true at the same time — age greater than 25, **and** department equal to HR. Looking at our dataset, Ali (28, HR) and Ahmed (30, HR) both qualify, since they satisfy both conditions together.

**A quick note:** In many cases, MongoDB lets you write an implicit `$and` just by listing multiple fields in one object, without the `$and` keyword:

```javascript
db.employee.find({ age: { $gt: 25 }, department: "HR" })
```

This works the same way for simple field-based conditions. You'll usually reach for the explicit `$and` syntax when you need to repeat the same field with different conditions, or combine more complex expressions.

---

## 4. `$or` Operator

`$or` returns documents that satisfy **at least one** of the listed conditions. The document doesn't need to match all of them — just one is enough.

**Syntax:**

```javascript
db.collection.find({
  $or: [
    { condition1 },
    { condition2 }
  ]
})
```

**Example — Find employees working in HR or IT department**

```javascript
db.employee.find({
  $or: [
    { department: "HR" },
    { department: "IT" }
  ]
})
```

**How to think about it:** A document only needs to match one of the two conditions to show up. From our dataset, this returns Ali, Talha, Ahmed, and Zain — everyone except Muzamil, who's in Finance.

---

## 5. `$not` Operator

`$not` returns documents that do **not** match a given condition. It's used to negate a single comparison, effectively flipping its meaning.

**Syntax:**

```javascript
db.collection.find({
  field: { $not: { comparison } }
})
```

**Example — Find employees who are not younger than 25**

```javascript
db.employee.find({
  age: { $not: { $lt: 25 } }
})
```

**How to think about it:** `$lt: 25` on its own means "less than 25." Wrapping it inside `$not` flips that meaning to "not less than 25" — which is effectively the same as "25 or older." From our dataset, this returns Ali (28), Talha (35), Muzamil (25), and Ahmed (30). Zain (22) is excluded, since 22 is less than 25.

**A quick note:** `$not` is a bit different from `$and`, `$or`, and `$nor` — it works on a single field's condition, rather than combining multiple full conditions together.

---

## 6. `$nor` Operator

`$nor` returns documents that do **not** match **any** of the listed conditions. It's essentially the exact opposite of `$or`.

**Syntax:**

```javascript
db.collection.find({
  $nor: [
    { condition1 },
    { condition2 }
  ]
})
```

**Example — Find employees not working in HR or Finance department**

```javascript
db.employee.find({
  $nor: [
    { department: "HR" },
    { department: "Finance" }
  ]
})
```

**How to think about it:** A document is only included if it fails **both** conditions — meaning department is neither HR nor Finance. From our dataset, this returns Talha and Zain, since they're both in IT.

---

