# Database Basics — SQL Fundamentals

## 1. What is a Database?

A **database** is an organized collection of data that can be stored, searched, updated, and managed efficiently.

For example, a university system may store information about:

- Students
- Courses
- Teachers
- Grades
- Enrollments

Instead of storing all this information in separate files, a database provides a structured way to manage it.

---

## 2. What is a Relational Database?

A **relational database** stores data in **tables**.

A table consists of:

- **Rows:** Individual records
- **Columns:** Properties of those records

Example:

### Students

| student_id | name | department |
|---:|---|---|
| 1 | Alice | Software Engineering |
| 2 | Bob | Mechanical Engineering |
| 3 | Carol | Software Engineering |

Here:

```text id="2ag4u8"
Table  → Students
Column → student_id, name, department
Row    → One student record
```

The important idea behind relational databases is that different tables can be connected through **relationships**.

---

## 3. What is SQL?

**SQL (Structured Query Language)** is a language used to communicate with relational databases.

SQL can be used to:

- Retrieve data
- Insert new data
- Update existing data
- Delete data
- Create and modify tables
- Combine data from multiple tables

For example:

```sql id="nm0o7z"
SELECT *
FROM students;
```

This query retrieves all records from the `students` table.

---

## 4. Primary Key

A **Primary Key (PK)** uniquely identifies each record in a table.

Example:

| student_id | name |
|---:|---|
| 1 | Alice |
| 2 | Bob |
| 3 | Alice |

Names can be repeated, but `student_id` is unique.

Therefore:

```text id="u5kz90"
student_id → Primary Key
```

A primary key:

- Must be unique
- Cannot be `NULL`
- Identifies one specific record

---

## 5. Foreign Key

A **Foreign Key (FK)** connects one table to another.

Example:

### Students

| student_id | name |
|---:|---|
| 1 | Alice |
| 2 | Bob |

### Enrollments

| enrollment_id | student_id | course_id |
|---:|---:|---:|
| 100 | 1 | 10 |
| 101 | 1 | 20 |
| 102 | 2 | 10 |

Here:

```text id="g1y9b0"
Enrollments.student_id
        ↓
Students.student_id
```

`student_id` in the `Enrollments` table is a foreign key referencing the `Students` table.

This allows tables to create relationships without storing the same information repeatedly.

---

## 6. Primary Key vs Foreign Key

| Primary Key | Foreign Key |
|---|---|
| Uniquely identifies a record | References another table |
| Must be unique | Can contain repeated values |
| Cannot be `NULL` | Can sometimes be `NULL` |
| Represents the record's identity | Creates relationships between tables |

Example structure:

```text id="ezjmw7"
STUDENTS
--------
student_id (PK)
name


ENROLLMENTS
-----------
enrollment_id (PK)
student_id    (FK)
course_id     (FK)
```

---

## 7. SELECT

`SELECT` is used to retrieve data from a database.

Get all students:

```sql id="1oummk"
SELECT *
FROM students;
```

Get only names:

```sql id="b7p8mr"
SELECT name
FROM students;
```

Get specific columns:

```sql id="hmq5v4"
SELECT name, department
FROM students;
```

Data can also be filtered using `WHERE`:

```sql id="o0r2pi"
SELECT *
FROM students
WHERE department = 'Software Engineering';
```

A useful way to remember it:

```text id="rs7jxg"
SELECT → What data?
FROM   → From which table?
WHERE  → Under what condition?
```

---

## 8. JOIN

`JOIN` combines related information from multiple tables.

For example:

### Students

| student_id | name |
|---:|---|
| 1 | Alice |
| 2 | Bob |

### Enrollments

| student_id | course_id |
|---:|---:|
| 1 | 10 |
| 2 | 20 |

We can combine them:

```sql id="fnf6km"
SELECT students.name, enrollments.course_id
FROM students
JOIN enrollments
ON students.student_id = enrollments.student_id;
```

Result:

| name | course_id |
|---|---:|
| Alice | 10 |
| Bob | 20 |

The basic idea is:

```text id="k43w6z"
Table A
   +
Table B
   ↓
Match related values
   ↓
Combined Result
```

---

## 9. GROUP BY

`GROUP BY` groups rows that have the same value.

Suppose we have:

| name | department |
|---|---|
| Alice | Software Engineering |
| Bob | Mechanical Engineering |
| Carol | Software Engineering |

We can count students in each department:

```sql id="vwus52"
SELECT department, COUNT(*)
FROM students
GROUP BY department;
```

Result:

| department | count |
|---|---:|
| Software Engineering | 2 |
| Mechanical Engineering | 1 |

`GROUP BY` is commonly used with aggregate functions:

| Function | Purpose |
|---|---|
| `COUNT()` | Counts records |
| `SUM()` | Calculates a total |
| `AVG()` | Calculates an average |
| `MIN()` | Finds the minimum |
| `MAX()` | Finds the maximum |

---

## 10. Index

An **index** is a data structure that helps the database find data more efficiently.

Think of the index at the end of a book.

Without an index, you may need to search through many pages to find a topic.

With an index:

```text id="4lft66"
Database → Page 120
Network  → Page 240
SQL      → Page 315
```

you can locate the information much faster.

The same idea applies to databases.

For example:

```sql id="t8zxnm"
SELECT *
FROM users
WHERE email = 'alice@example.com';
```

If the table contains millions of users, an index on `email` can make this search significantly more efficient.

An index can be created with:

```sql id="vudam3"
CREATE INDEX idx_users_email
ON users(email);
```

However, indexes also require additional storage and must be updated when data changes.

Therefore, adding an index to every column is usually not a good idea.

---

## 11. Simple Database Structure

A basic university database could look like this:

```text id="llq3i8"
STUDENTS
--------
student_id (PK)
name
department


COURSES
-------
course_id (PK)
course_name


ENROLLMENTS
-----------
enrollment_id (PK)
student_id (FK)
course_id (FK)
grade
```

The relationships are:

```text id="ttwvp1"
STUDENTS
    |
    | student_id
    ↓
ENROLLMENTS
    ↑
    | course_id
    |
COURSES
```

The `Enrollments` table connects students with courses.

---

## 12. Database and API Relationship

In a backend application, APIs and databases often work together.

For example:

```text id="m5mmgk"
Frontend
   ↓
GET /api/students/5
   ↓
Backend
   ↓
SQL Query
   ↓
Database
```

The backend could execute:

```sql id="9sfguc"
SELECT *
FROM students
WHERE student_id = 5;
```

The result could then be returned through the API as JSON:

```json id="bh3h5k"
{
  "id": 5,
  "name": "Alice",
  "department": "Software Engineering"
}
```

The complete flow becomes:

```text id="x5vz6o"
Frontend
   ↓
API
   ↓
Backend
   ↓
SQL
   ↓
Database
```

---

## Summary

```text id="u0u1ce"
Database
↓
Stores and manages data.

Relational Database
↓
Stores data in related tables.

Primary Key
↓
Uniquely identifies a record.

Foreign Key
↓
Creates relationships between tables.

SELECT
↓
Retrieves data.

WHERE
↓
Filters data.

JOIN
↓
Combines related data from multiple tables.

GROUP BY
↓
Groups records with common values.

Index
↓
Helps the database find data more efficiently.
```

Understanding relational databases and SQL provides an important foundation for **data management and backend development**.