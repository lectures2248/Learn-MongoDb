# MongoDB Lecture — Aggregation Pipeline
### Session 3

---

## 1. What is Aggregation?

Aggregation means processing a large number of documents and transforming them into meaningful, summarized results — things like totals, averages, groupings, and rankings.

So far, `find()` has helped you fetch and filter records. But `find()` can't calculate an average, count how many records fall into each category, or combine data from multiple stages of processing. That's exactly where aggregation comes in.

You use aggregation to answer real business questions, such as:

- How many students are from each city?
- What is the average score of students?
- What is the total sales per month?
- Who are the top 5 highest earners in the company?

**A simple way to remember the difference:** `find()` shows you records. Aggregation shows you insights.

---

## 2. What is a Pipeline?

An aggregation pipeline is a sequence of stages through which your documents pass, one after another — similar to an assembly line.

Each stage performs one specific operation on the documents it receives, and then passes the transformed result on to the next stage in line.

**Here's how to picture it:**

1. You start with your full collection of documents
2. You send them through a pipeline made up of multiple stages
3. Each stage transforms the data a little — filtering it, grouping it, reshaping it, sorting it, and so on
4. At the very end of the pipeline, you get your final, transformed result

The key idea is that the output of one stage becomes the input of the next. You're not writing one giant query — you're building a chain of small, simple steps that together produce a powerful result.

---

## 3. Aggregation Pipeline Syntax

Every aggregation query follows the same basic shape: an array of stages, each written as its own object.

```javascript
db.collection.aggregate([
  { stage1 },
  { stage2 },
  { stage3 }
])
```

The order of the stages matters. Data flows from top to bottom, so a `$sort` written before a `$limit` behaves very differently than a `$sort` written after it.

---

## 4. Common Aggregation Stages — Quick Reference

| Stage | Purpose |
|---|---|
| `$match` | Filters documents — works like a `WHERE` clause in SQL |
| `$group` | Groups documents together and applies calculations like SUM or AVG |
| `$sort` | Sorts documents by one or more fields |
| `$project` | Reshapes documents — pick, rename, or add fields |
| `$count` | Counts the number of documents |
| `$limit` | Limits how many documents are returned |
| `$skip` | Skips a certain number of documents |
| `$sortByCount` | Groups, counts, and sorts in a single step |
| `$sample` | Randomly selects a set of documents |

---

## 5. Setting Up Sample Data

Before trying out any of these stages, insert this sample dataset into your `students` collection.

```javascript
db.students.insertMany([
  { _id: 1, name: "Dua",   age: 22, gender: "F", score: 85, city: "Islamabad" },
  { _id: 2, name: "Azeem", age: 25, gender: "M", score: 90, city: "Karachi" },
  { _id: 3, name: "Ali",   age: 23, gender: "M", score: 70, city: "Peshawar" },
  { _id: 4, name: "Zain",  age: 22, gender: "F", score: 95, city: "Lahore" }
])
```

We'll use this same dataset throughout every example below.

---

## 6. `$match` — Filtering Documents

`$match` filters documents so that only the ones meeting your condition move forward in the pipeline. It works exactly like a `WHERE` clause in SQL.

**Syntax:**

```javascript
{ $match: { field: value } }
```

**Example — Find students from New York**

```javascript
db.students.aggregate([
  { $match: { city: "New York" } }
])
```

**How to think about it:** Whatever passes through `$match` is the only data that continues to the next stage. Anything that doesn't match the condition is dropped from the pipeline entirely.

`$match` also supports comparison operators, just like `find()` does:

```javascript
db.students.aggregate([
  { $match: { score: { $gte: 75 } } }
])
```

---

## 7. `$count` — Counting Documents

`$count` counts how many documents have made it through the pipeline up to that point, and returns that number under a field name you choose.

**Syntax:**

```javascript
{ $count: "fieldName" }
```

**Example — Count how many students are from New York**

```javascript
db.students.aggregate([
  { $match: { city: "New York" } },
  { $count: "total_students_in_ny" }
])
```

**How to think about it:** This combines two stages — first `$match` filters down to just the New York students, and then `$count` tallies up whatever remains. This is a good early example of how pipelines chain together: each stage builds directly on the last.

---

## 8. `$sort` — Sorting Documents

`$sort` arranges documents based on one or more fields.

- `1` means ascending order (smallest to largest)
- `-1` means descending order (largest to smallest)

**Syntax:**

```javascript
{ $sort: { field: 1 or -1 } }
```

**Example — Sort students by score, highest first**

```javascript
db.students.aggregate([
  { $sort: { score: -1 } }
])
```

---

## 9. `$sortByCount` — Group and Sort by Frequency

`$sortByCount` is a shortcut that combines three operations into one: grouping, counting, and sorting.

**Syntax:**

```javascript
{ $sortByCount: "$fieldName" }
```

**Example — Count how many students are in each city**

```javascript
db.students.aggregate([
  { $sortByCount: "$city" }
])
```

**How to think about it:** Normally, getting this same result would take a `$group` stage followed by a `$sort` stage. `$sortByCount` does both in a single, simple line — perfect for quick frequency breakdowns like "how many per category."

---

## 10. `$project` — Reshaping Documents

`$project` is used to reshape your documents — you can include fields, exclude fields, rename them, or calculate brand new fields on the fly.

**Syntax:**

```javascript
{ $project: { field: 1, field2: 0 } }
```

**Example — Show only name and score, and calculate a "passed" status (score of 75 or higher)**

```javascript
db.students.aggregate([
  {
    $project: {
      name: 1,
      score: 1,
      passed: { $gte: ["$score", 75] }
    }
  }
])
```

**How to think about it:** `passed` isn't a field that already exists in your data — it's being calculated right inside the pipeline using the `$gte` (greater than or equal to) comparison. This is one of the most powerful things about `$project`: it doesn't just select fields, it can create entirely new ones based on logic.

---

## 11. `$limit` — Restricting the Number of Documents

`$limit` caps how many documents are allowed to pass through at that point in the pipeline.

**Syntax:**

```javascript
{ $limit: number }
```

**Example — Return only the top 2 scorers**

```javascript
db.students.aggregate([
  { $sort: { score: -1 } },
  { $limit: 2 }
])
```

**How to think about it:** Sorting first and then limiting is what actually gives you a "top N" result. If you limited before sorting, you'd just get 2 random documents instead of the 2 highest scorers.

---

## 12. `$skip` — Skipping Documents

`$skip` skips over a set number of documents before returning the rest. This is especially useful for pagination — showing "page 2" of a result set, for example.

**Syntax:**

```javascript
{ $skip: number }
```

**Example — Skip the top 2 scorers and show the rest**

```javascript
db.students.aggregate([
  { $sort: { score: -1 } },
  { $skip: 2 }
])
```

---

## 13. `$sample` — Randomly Selecting Documents

`$sample` selects a random set of documents from your collection — useful for testing or getting a quick, unbiased sense of your data.

**Syntax:**

```javascript
{ $sample: { size: number } }
```

**Example — Randomly pick 2 students**

```javascript
db.students.aggregate([
  { $sample: { size: 2 } }
])
```

---

## 14. `$group` — Grouping and Calculating

`$group` is one of the most important stages in aggregation. It groups documents that share a common value, and then lets you calculate something for each group — a total, an average, a count, and more.

**Syntax:**

```javascript
{
  $group: {
    _id: "$fieldToGroupBy",
    resultField: { $sum: 1 }
  }
}
```

The `_id` in a `$group` stage is not the document's `_id` field — it defines what you're grouping by.

**Example — Count how many students belong to each city**

```javascript
db.students.aggregate([
  {
    $group: {
      _id: "$city",
      totalStudents: { $sum: 1 }
    }
  }
])
```

**Example — Find the average score per city**

```javascript
db.students.aggregate([
  {
    $group: {
      _id: "$city",
      averageScore: { $avg: "$score" }
    }
  }
])
```

**Common accumulator operators used inside `$group`:**

| Operator | Purpose |
|---|---|
| `$sum` | Adds up values (use `$sum: 1` just to count documents) |
| `$avg` | Calculates the average |
| `$max` | Finds the highest value |
| `$min` | Finds the lowest value |
| `$push` | Collects values into an array |

**Example — Find the highest score in each city**

```javascript
db.students.aggregate([
  {
    $group: {
      _id: "$city",
      topScore: { $max: "$score" }
    }
  }
])
```

---

## 15. Combining Operators — A Full Example

This is where aggregation really shows its power — chaining multiple stages together to answer one specific question in a single query.

**Goal:** Find the top 2 highest-scoring students who scored 75 or above, and show only their name and score.

```javascript
db.students.aggregate([
  { $match: { score: { $gte: 75 } } },          // Step 1: Keep only students scoring 75+
  { $sort: { score: -1 } },                     // Step 2: Sort by score, highest first
  { $limit: 2 },                                // Step 3: Keep only the top 2
  { $project: { name: 1, score: 1, _id: 0 } }   // Step 4: Show only name and score
])
```

Read this from top to bottom — that's exactly the order your data flows through. Each stage takes what the previous stage produced and narrows or reshapes it a little further, until you're left with exactly the answer you were looking for.

---
