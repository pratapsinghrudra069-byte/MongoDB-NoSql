# MongoDB-NoSql
# 📊 MongoDB Student Management System

A comprehensive **NoSQL project using MongoDB** that demonstrates real-world database design, querying, and data manipulation for an academic environment. This project models students, faculty, courses, enrollments, and activities, and showcases advanced MongoDB operations.

---

## 🚀 Project Overview

This project simulates a **college database system** using MongoDB. It includes multiple collections and demonstrates how to:

* Design NoSQL schemas
* Perform complex queries
* Use aggregation pipelines
* Implement joins using `$lookup`
* Handle updates, deletes, and upserts

---

## 🗂️ Database Structure

### Collections Used:

* **students**
* **faculty**
* **courses**
* **enrollments**
* **activities**

---

## ⚙️ Setup Instructions

### 1. Install MongoDB & Compass

* Install MongoDB locally
* Open MongoDB Compass

### 2. Connect to Database

```bash id="conn1"
mongodb://localhost:27017
```

### 3. Create Database

* Database Name: `Mongo Assignment`
* Collection Name: `students`

### 4. Insert Sample Data

* Use **Insert Document**
* Add JSON records

---

## 🧠 Features & Queries Implemented

---

### 🔍 1. Complex Filters & Projections

**👉 List students with >85% attendance and skills in MongoDB & Python**

```javascript id="q1"
db.students.find(
  {
    attendance: { $gt: 85 },
    skills: { $all: ["MongoDB", "Python"] }
  },
  {
    name: 1,
    department: 1,
    _id: 0
  }
)
```

---

### 🔗 2. Joins ($lookup)

**👉 Show each student’s name with enrolled course titles**

```javascript id="q2"
db.enrollments.aggregate([
  {
    $lookup: {
      from: "students",
      localField: "student_id",
      foreignField: "_id",
      as: "student"
    }
  },
  { $unwind: "$student" },
  {
    $lookup: {
      from: "courses",
      localField: "course_id",
      foreignField: "_id",
      as: "course"
    }
  },
  { $unwind: "$course" },
  {
    $project: {
      _id: 0,
      student_name: "$student.name",
      course_title: "$course.title"
    }
  }
])
```

---

### 📈 3. Aggregation (Grouping)

**👉 Course title, number of students, and average marks**

```javascript id="q3"
db.enrollments.aggregate([
  {
    $group: {
      _id: "$course_id",
      total_students: { $sum: 1 },
      avg_marks: { $avg: "$marks" }
    }
  },
  {
    $lookup: {
      from: "courses",
      localField: "_id",
      foreignField: "_id",
      as: "course"
    }
  },
  { $unwind: "$course" },
  {
    $project: {
      _id: 0,
      course_title: "$course.title",
      total_students: 1,
      avg_marks: 1
    }
  }
])
```

---

### 🔄 4. Update Operation

**👉 Update attendance to 100% for students who participated in Hackathon**

```javascript id="q4"
db.students.updateMany(
  { activities: "Hackathon" },
  { $set: { attendance: 100 } }
)
```

---

## 📌 Key MongoDB Concepts Used

* `$match`
* `$project`
* `$group`
* `$lookup`
* `$set`
* `$avg`
* `$sum`
* `$all`

---

## 🛠️ Tools Used

* MongoDB
* MongoDB Compass
* JSON

---

## 📷 Sample Use Cases

* Academic performance tracking
* Faculty workload analysis
* Activity participation insights
* Course enrollment analytics

---

## 🙌 Author

**Sumit Singh**
Class: MCADS11
Roll No: 1250259060

---

## ⭐ Contribute / Improve

Feel free to:

* Add more features
* Improve schema
* Build frontend

---

## 📜 License

This project is for educational purposes.

---


