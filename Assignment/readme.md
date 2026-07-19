# MongoDB Assignment
---
## Instructions

- Use the `mongosh` shell or MongoDB Compass to attempt every task.
- Write your own queries first before checking any notes — that's how the concepts actually stick.
- Every task below is a real-world scenario. Read it carefully, figure out what data operation it needs, and then write the matching query.
- Submit your final answers as a single `.txt` file with each query numbered to match the task number.

---

## Setup — Run This First

Create a fresh collection and load this dataset before starting the assignment.

```javascript
use companyDB

db.createCollection("employees")

db.employees.insertMany([
  { name: "Bilal",  age: 24, department: "IT",      city: "Karachi",    salary: 45000 },
  { name: "Ayesha", age: 29, department: "HR",       city: "Lahore",     salary: 60000 },
  { name: "Hamza",  age: 33, department: "Finance",  city: "Karachi",    salary: 75000 },
  { name: "Sara",   age: 21, department: "IT",       city: "Islamabad",  salary: 40000 },
  { name: "Usman",  age: 38, department: "HR",       city: "Karachi",    salary: 55000 },
  { name: "Zoya",   age: 27, department: "Finance",  city: "Peshawar",   salary: 68000 },
  { name: "Danish", age: 45, department: "IT",       city: "Lahore",     salary: 90000 },
  { name: "Mahnoor",age: 19, department: "HR",       city: "Islamabad",  salary: 35000 }
])
```

---

## Section A — Session 1 Scenarios (CRUD Operations)

**Task 1**
The HR manager wants to see the full record of the employee named "Hamza" before making a decision about his promotion. Write a query to fetch just that one record.

**Task 2**
The company wants to onboard three new employees at once instead of adding them one by one. Write a query to insert three new employee documents of your choice, following the same structure as the dataset above.

**Task 3**
Bilal recently got a raise. His salary needs to be updated to `50000`. Write a query to update only his record.

**Task 4**
Due to a city-wide policy, every employee based in Karachi needs a new field added to their record called `allowance`, set to `5000`. Write a query that updates all matching records at once.

**Task 5**
It's appraisal season, and every employee's salary needs to increase by `2000` company-wide. Write a query using the correct operator to increase the existing salary values, rather than replacing them.

**Task 6**
Mahnoor has resigned from the company. Write a query to remove only her record from the collection.

**Task 7**
The Finance department has been shut down, and all employees in that department have already been moved to other departments in another system. Write a query to delete every employee record where the department is "Finance".

**Task 8**
Management wants a report showing only employee names and their departments — not their age, city, or salary. Write a query using projection to return only these two fields (and make sure `_id` doesn't show up either).

**Task 9**
The finance team wants to know exactly how many employees currently exist in the company. Write a query to return that count.

---

## Section B — Session 2 Scenarios (Logical Operators)

**Task 10**
The company is planning a bonus for senior, high-earning staff. Find every employee who is older than 30 **and** earns more than 60000.

**Task 11**
The transport team is arranging pickup vans and needs the names of everyone based in Karachi or Lahore, since those are the two covered cities. Find all employees who live in Karachi **or** Lahore.

**Task 12**
An internal audit needs to review every department except HR and IT, since those two have already been cleared. Find all employees who are **not** working in HR or IT.

**Task 13**
The company is hosting an event only for employees who are not considered "junior" — meaning their age should not be less than 25. Find all employees who are **not** younger than 25.

**Task 14**
A relocation bonus is being offered only to employees who are either from Islamabad, or who are under the age of 25 — since both groups are considered priority cases. Find every employee who matches either of these two conditions.

**Task 15**
The company wants to flag anyone who does not belong to the IT department **and** does not earn more than 50000, since both these employees need extra training and a salary review. Find employees who satisfy both of these conditions using the correct operator.

**Task 16**
For an upcoming inclusion review, the company wants to exclude anyone from Karachi and anyone earning below 40000 from the review list entirely — meaning neither condition should apply to the employees selected. Find employees who match neither of these two conditions.

---
## Submission Checklist

Before submitting, make sure you have:

- One working query for every task above (17 in total, including the bonus)
- Correct use of `$and`, `$or`, `$not`, and `$nor` where the scenario calls for it
- Correct use of `insertOne`, `insertMany`, `updateOne`, `updateMany`, `deleteOne`, `deleteMany`, `find`, `findOne`, and `countDocuments` where relevant
- Comments above each query indicating which task number it answers
