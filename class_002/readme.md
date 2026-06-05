```
MONGODB CRUD OPERATIONS - PRACTICAL LECTURE
```

```
---
```

```
## TODAY'S TOPICS
```

`1. findOne()` 

`2. updateOne()` 

`3. updateMany()` 

`4. deleteOne()` 

`5. deleteMany()` 

```
---
```

```
## STEP 1: CREATE DATABASE
```

```
use school
```

```
---
```

```
## STEP 2: CREATE COLLECTION
```

```
db.createCollection("students")
```

```
---
```

```
## STEP 3: INSERT SAMPLE DATA
```

```
db.students.insertMany([
{
name:"Ali",
age:20,
city:"Karachi",
course:"MERN"
},
{
name:"Ahmed",
age:22,
city:"Lahore",
course:"Flutter"
},
{
name:"Sara",
age:19,
city:"Islamabad",
course:"Python"
},
{
name:"Hina",
age:25,
city:"Karachi",
course:"MERN"
},
{
name:"Usman",
age:27,
city:"Karachi",
course:"Flutter"
}
])
```

```
---
```

```
## DISPLAY ALL RECORDS
```

```
db.students.find()
```

```
---
```

```
## FINDONE()
```

```
Returns only one matching document.
Example 1
```

```
db.students.findOne({name:"Ali"})
```

```
Example 2
```

```
db.students.findOne({city:"Karachi"})
```

```
Example 3
```

```
db.students.findOne({course:"Flutter"})
```

```
Example 4
```

```
db.students.findOne({age:25})
```

```
---
```

```
## PRACTICE TASKS
```

```
Find:
```

`1. Sara` 

`2. Ahmed` 

`3. Student from Lahore` 

`4. Student whose age is 27` 

```
---
```

## `## UPDATEONE()` 

```
Updates only one document.
```

```
Syntax
```

```
db.students.updateOne(
{condition},
{$set:{field}}
)
```

```
---
```

## `## EXAMPLE 1` 

```
Change Ali's age to 21
```

```
db.students.updateOne(
{name:"Ali"},
{$set:{age:21}}
)
```

```
Check Result
```

```
db.students.find()
```

```
---
```

## `## EXAMPLE 2` 

```
Change Ahmed's city to Karachi
```

```
db.students.updateOne(
{name:"Ahmed"},
{$set:{city:"Karachi"}}
)
---
```

```
## EXAMPLE 3
Change Sara's course to AI
db.students.updateOne(
{name:"Sara"},
{$set:{course:"AI"}}
)
---
## EXAMPLE 4
Add new field phone
```

```
db.students.updateOne(
{name:"Ali"},
{$set:{phone:"03001234567"}}
)
Before
{
name:"Ali",
age:21
}
After
{
name:"Ali",
age:21,
phone:"03001234567"
}
---
```

```
## EXAMPLE 5
Add status field
```

```
db.students.updateOne(
{name:"Usman"},
{$set:{status:"Active"}}
)
```

```
---
```

```
## PRACTICE TASKS
```

```
1. Change Hina's city to Lahore
```

`2. Change Usman's age to 30` 

`3. Add Email field to Ahmed` 

`4. Add Semester field to Sara` 

```
---
```

```
## UPDATEMANY()
```

```
Updates multiple documents.
```

```
Syntax
```

```
db.students.updateMany(
{condition},
{$set:{field}}
)
```

```
---
```

## `## EXAMPLE 1` 

```
Update all Karachi students
```

```
db.students.updateMany(
{city:"Karachi"},
{$set:{country:"Pakistan"}}
)
```

```
---
```

## `## EXAMPLE 2` 

```
Update all Flutter students
```

```
db.students.updateMany(
{course:"Flutter"},
{$set:{trainer:"Mujtaba"}}
)
```

```
---
```

## `## EXAMPLE 3` 

```
Add status field to everyone
db.students.updateMany(
{},
{$set:{status:"Active"}}
)
---
```

```
## EXAMPLE 4
```

```
Add institute field to all records
db.students.updateMany(
{},
{$set:{institute:"ABC Institute"}}
)
```

```
---
```

```
## $inc OPERATOR
```

```
Used to increase numeric values.
```

```
---
```

```
## EXAMPLE 1
```

```
Increase Ali age by 1
db.students.updateOne(
{name:"Ali"},
{$inc:{age:1}}
)
---
```

```
## EXAMPLE 2
Increase all ages by 2
db.students.updateMany(
{},
{$inc:{age:2}}
)
---
```

```
## EXAMPLE 3
Increase Karachi students age by 5
```

```
db.students.updateMany(
{city:"Karachi"},
{$inc:{age:5}}
)
---
```

```
## PRACTICE TASKS
```

```
1. Increase Ahmed age by 3
2. Increase all ages by 1
```

`3. Increase Lahore students age by 2` 

```
---
```

```
## DELETEONE()
```

```
Deletes only one document.
```

```
Syntax
```

```
db.students.deleteOne(
{condition}
)
---
```

```
## EXAMPLE 1
```

```
Delete Sara
```

```
db.students.deleteOne(
{name:"Sara"}
```

```
)
```

```
---
```

```
## EXAMPLE 2
```

```
Delete Ahmed
```

```
db.students.deleteOne(
{name:"Ahmed"}
)
---
```

## `## EXAMPLE 3` 

```
Delete first Karachi student
```

```
db.students.deleteOne(
{city:"Karachi"}
)
```

```
---
```

```
## CHECK DATA
```

```
db.students.find()
```

```
---
```

```
## PRACTICE TASKS
```

`1. Delete Hina` 

`2. Delete Ali` 

`3. Delete one Lahore student` 

```
---
```

## `## DELETEMANY()` 

```
Deletes multiple documents.
```

```
Syntax
```

```
db.students.deleteMany(
{condition}
)
```

```
---
```

## `## EXAMPLE 1` 

```
Delete all Karachi students
```

```
db.students.deleteMany(
{city:"Karachi"}
)
```

```
---
```

## `## EXAMPLE 2` 

```
Delete all Flutter students
```

```
db.students.deleteMany(
{course:"Flutter"}
)
---
```

```
## EXAMPLE 3
```

```
Delete all students older than 25
db.students.deleteMany(
{age:{$gt:25}}
)
---
```

```
## EXAMPLE 4
Delete all students younger than 20
db.students.deleteMany(
{age:{$lt:20}}
)
---
## EXAMPLE 5
Delete all students from Lahore
db.students.deleteMany(
{city:"Lahore"}
)
---
```

```
## DELETE ALL DOCUMENTS
db.students.deleteMany({})
WARNING:
```

```
This command removes ALL documents from the collection.
```

```
---
## FINAL PRACTICAL
Insert Data
db.students.insertMany([
{name:"Ahsan",age:20,city:"Karachi"},
{name:"Bilal",age:22,city:"Lahore"},
{name:"Hamza",age:25,city:"Karachi"},
{name:"Saad",age:28,city:"Islamabad"},
{name:"Zara",age:19,city:"Karachi"}
])
Task 1
Find Zara
db.students.findOne({name:"Zara"})
```

```
Task 2
```

```
Change Zara city to Lahore
db.students.updateOne(
{name:"Zara"},
{$set:{city:"Lahore"}}
)
Task 3
Add status Active to all students
db.students.updateMany(
{},
{$set:{status:"Active"}}
)
Task 4
Increase everyone's age by 1
db.students.updateMany(
{},
{$inc:{age:1}}
)
Task 5
Delete all students from Karachi
db.students.deleteMany(
{city:"Karachi"}
)
Task 6
Display final data
db.students.find()
```

```
---
```

