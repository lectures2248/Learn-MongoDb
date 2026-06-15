```
MONGODB JOINS (COMPLETE BEGINNER TO PRACTICAL LECTURE)
In MongoDB, we do NOT have traditional SQL joins like:
```

```
INNER JOIN
LEFT JOIN
RIGHT JOIN
```

```
But MongoDB provides a powerful alternative called:
$lookup (Aggregation Pipeline)
```

```
This is how we join two collections in MongoDB.
```

```
==================================================
WHY JOINS ARE NEEDED?
=====================
```

```
In real applications data is split into multiple collections.
```

```
Example:
```

```
1. users collection
```

```
2. orders collection
```

```
User data:
```

```
{
_id: 1,
name: "Ali"
}
```

```
Order data:
```

```
{
_id: 101,
user_id: 1,
product: "Laptop"
}
```

```
Now we want:
```

```
"Show orders with user details"
```

```
This is where JOIN concept comes in.
```

```
==================================================
MONGODB JOIN CONCEPT
====================
```

```
MongoDB uses:
```

```
Aggregation Pipeline
```

```
Main stage for joining:
```

```
$lookup
```

```
==================================================
BASIC STRUCTURE OF LOOKUP
=========================
```

```
db.orders.aggregate([
```

```
{
$lookup: {
from: "users",
localField: "user_id",
foreignField: "_id",
as: "user_details"
}
}
])
```

```
==================================================
STEP BY STEP UNDERSTANDING
==========================
```

```
$lookup has 4 main parts:
```

```
---
```

```
1. from
```

```
---
```

```
from: "users"
```

```
Meaning:
```

```
Which collection we want to join.
```

```
Here:
```

```
users collection
```

```
---
```

```
2. localField
```

```
---
```

```
localField: "user_id"
```

```
Meaning:
```

```
Field in current collection (orders)
```

```
This is the matching key.
```

```
---
```

```
3. foreignField
```

```
---
```

```
foreignField: "_id"
```

```
Meaning:
```

```
Field in second collection (users)
```

```
We match:
```

```
orders.user_id = users._id
```

```
---
```

```
4. as
```

```
---
```

```
as: "user_details"
```

```
Meaning:
```

```
Where joined data will be stored.
```

```
Result will look like:
```

```
user_details: [ {...} ]
```

```
==================================================
OUTPUT RESULT
=============
```

```
After join:
```

```
{
_id: 101,
user_id: 1,
product: "Laptop",
user_details: [
{
_id: 1,
name: "Ali"
}
]
}
```

```
==================================================
TYPES OF JOINS IN MONGODB
=========================
```

`1. ONE TO ONE JOIN` 

`2. ONE TO MANY JOIN` 

`3. MANY TO MANY JOIN` 

```
We will understand all with examples.
```

```
==================================================
```

`1. ONE TO ONE JOIN ==================================================` 

```
Example:
```

```
users → profile
```

```
User has one profile only.
```

```
users:
```

```
{
_id: 1,
name: "Ali"
}
```

```
profiles:
```

```
{
```

```
user_id: 1,
age: 22
}
QUERY:
db.users.aggregate([
{
$lookup: {
from: "profiles",
localField: "_id",
foreignField: "user_id",
as: "profile"
}
}
])
```

```
OUTPUT:
```

```
{
_id: 1,
name: "Ali",
profile: [
{
user_id: 1,
age: 22
}
]
}
```

```
==================================================
2. ONE TO MANY JOIN
===================
```

```
Example:
```

```
users → orders
```

```
One user has many orders.
```

```
users:
```

```
{
_id: 1,
name: "Ali"
}
orders:
```

```
{
user_id: 1,
product: "Phone"
}
{
user_id: 1,
product: "Laptop"
}
QUERY:
db.users.aggregate([
```

```
{
$lookup: {
from: "orders",
localField: "_id",
foreignField: "user_id",
as: "orders"
}
}
])
OUTPUT:
{
_id: 1,
name: "Ali",
orders: [
{ product: "Phone" },
{ product: "Laptop" }
]
}
==================================================
3. MANY TO MANY JOIN
====================
```

```
Example:
students ↔ courses
students:
{
_id: 1,
name: "Ali"
}
courses:
{
_id: 10,
name: "Math"
}
We use third collection:
student_courses
student_courses:
{
student_id: 1,
course_id: 10
}
JOIN QUERY:
db.student_courses.aggregate([
```

```
{
$lookup: {
from: "students",
localField: "student_id",
```

```
foreignField: "_id",
as: "student"
}
},
{
$lookup: {
from: "courses",
localField: "course_id",
foreignField: "_id",
as: "course"
}
}
])
```

```
==================================================
FINAL RESULT
============
```

```
{
student_id: 1,
course_id: 10,
student: [{ name: "Ali" }],
course: [{ name: "Math" }]
}
```

```
==================================================
IMPORTANT REAL-WORLD POINT
==========================
```

```
MongoDB JOIN is NOT like SQL JOIN.
```

```
It is:
```

```
Aggregation-based JOIN
```

```
Meaning:
```

```
We process data step by step pipeline.
```

```
==================================================
WHEN TO USE LOOKUP?
===================
```

```
Use $lookup when:
```

- `✔ You need related data from another collection` 

- `✔ You want relational structure in NoSQL` 

- `✔ You are building dashboards` 

- `✔ You are showing combined API response` 

```
==================================================
WHEN NOT TO USE LOOKUP?
=======================
```

```
Avoid $lookup when:
```

- `✘ Data is already embedded` 

- `✘ High performance system (too many joins slow queries) ✘ Simple single collection queries` 

```
==================================================
REAL PROJECT EXAMPLE
```

```
====================
```

```
E-commerce App:
```

```
products
categories
orders
users
```

```
Example:
Product → Category JOIN
db.products.aggregate([
```

```
{
$lookup: {
from: "categories",
localField: "category_id",
foreignField: "_id",
as: "category"
}
}
```

## `])` 

```
Now every product shows category details.
```

```
