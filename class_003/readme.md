MONGODB JOINS (LOOKUP) - BEGINNER FRIENDLY LECTURE 

WHAT IS A JOIN? 

A Join is used to combine data from multiple collections. 

DOES MONGODB SUPPORT JOINS? 

Yes. MongoDB uses the $lookup operator instead of JOIN. 

## STEP 1: CREATE DATABASE 

use school 

STEP 2: CREATE STUDENTS COLLECTION 

db.students.insertMany([ {studentId:1,name:"Ali",city:"Karachi"}, {studentId:2,name:"Ahmed",city:"Lahore"}, {studentId:3,name:"Sara",city:"Islamabad"} ]) 

STEP 3: CREATE COURSES COLLECTION db.courses.insertMany([ {studentId:1,course:"MERN"}, {studentId:2,course:"Flutter"}, {studentId:3,course:"Python"} ]) 

FIRST LOOKUP QUERY 

db.students.aggregate([ { $lookup:{ from:"courses", localField:"studentId", foreignField:"studentId", as:"courseDetails" } } ]) 

LOOKUP PROPERTIES 

from: 

Collection from which data will come. 

localField: 

Field from current collection. 

foreignField: Field from second collection. 

as: 

Name of the output field. LOOKUP FORMULA 

db.collection.aggregate([ { 

$lookup:{ from:"secondCollection", localField:"field1", foreignField:"field2", as:"outputField" } } ]) 

