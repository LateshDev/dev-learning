# MySQL, Tables, Relationships and Joins

# 1. What I Learned Today

Earlier, I thought data could simply be saved in files such as JSON, text files, or spreadsheets. But when the amount of information increases, managing files becomes difficult.

Today I learned how MySQL provides a proper way to store information, create tables, connect related data, and retrieve information using SQL queries.

# The main topics covered were:

- Databases and DBMS
- MySQL and RDBMS
- Tables, rows and columns
- Primary keys
- Foreign keys
- Indexes
- SQL commands
- Joins
- Data types
- SQL vs NoSQL
- ORM and Sequelize
- Basic database security concepts

---

# 2. What is Data?

Data means information or facts that can be stored and used later.

Examples

- Student name
- Roll number
- Email
- Mobile number
- Course
- Marks
- Salary

For example:

Name: Aman
Course: B.Tech
Semester: 3
Marks: 78

---

# 3. What is a Database?

A database is a structured place where information can be stored and managed.

Instead of keeping hundreds of separate files, we can organize information into tables and retrieve it whenever required.

For example, a college database could contain:

- Students
- Faculty
- Subjects
- Attendance
- Examination records

A database makes operations such as searching, inserting, modifying and deleting records much easier.

---

# 4. File Storage vs Database

Suppose a college stores all student information inside one large text file.

As the number of students grows, several problems can occur:

- Searching for a particular student can become slow.
- Multiple users may try to modify the same file.
- Duplicate information can easily be entered.
- There may be no proper rules for valid data.
- A failure during writing can damage the file.
- Maintaining relationships between different types of information becomes difficult.

A database is designed to solve these problems.

It provides:

- Better data organization
- Faster searching
- Data validation
- Relationship management
- Multi-user support
- Better consistency

So, a simple file is suitable for small and simple data, while a database is much more useful when applications become larger.

---

# 5. DBMS

DBMS means Database Management System.

It is software that allows us to create and manage databases.

A DBMS can perform operations such as:

- Creating databases
- Storing information
- Searching records
- Updating records
- Removing records
- Controlling access
- Managing data consistency

Examples include:

- MySQL
- PostgreSQL
- Oracle Database
- Microsoft SQL Server

---

# 6. RDBMS

RDBMS means Relational Database Management System.

It stores information in tables made up of rows and columns.

The important feature is that different tables can have relationships with each other.

For example:

Students

student_id| student_name
101| Aman
102| Riya

Subjects
```
subject_id| student_id| subject
         1| 101       | Java
         2| 102       | Python
```
Here, "student_id" connects the two tables.

MySQL is an example of an RDBMS.

---

# 7. MySQL Server and MySQL Workbench

I installed two important components:

MySQL Server

This is the actual database system.

It stores the data and processes SQL commands.

MySQL Workbench

Workbench provides a graphical interface through which I can:

- Write SQL queries
- Create tables
- View records
- Manage databases
- Run commands

In simple terms:
```
MySQL Workbench → sends commands → MySQL Server
                                      ↓
                                   Database
```
---

# 8. Database Structure

The basic hierarchy can be understood like this:
```
MySQL Server
     |
     └── Database
           |
           ├── Table
           │     ├── Row
           │     └── Row
           |
           └── Table
                 ├── Row
                 └── Row
```
A server can contain multiple databases.

A database can contain multiple tables.

A table contains rows, and each row represents one record.

---

# 9. Creating a Database

For practice, I created a database called "college_portal".

CREATE DATABASE college_portal;

USE college_portal;

"CREATE DATABASE" creates the database.

"USE" selects the database in which the upcoming queries will run.

---
# 10 Creating a Student Table

CREATE TABLE students (
    student_id INT AUTO_INCREMENT PRIMARY KEY,
    full_name VARCHAR(80) NOT NULL,
    email VARCHAR(120) NOT NULL UNIQUE
);
``
Explanation

                   Code| Purpose
       "student_id INT"| Stores a numerical ID
       "AUTO_INCREMENT"| Generates IDs automatically
          "PRIMARY KEY"| Gives every record a unique identity
"full_name VARCHAR(80)"| Stores text up to 80 characters
             "NOT NULL"| The field cannot be empty
               "UNIQUE"| Prevents duplicate email addresses

Because "student_id" is auto-generated, I don't need to manually provide it every time.

---

# 11. Adding Records

I can insert student information using:

INSERT INTO students (full_name, email)
VALUES ('Neeraj Sharma', 'neeraj@college.com');

Another example:

INSERT INTO students (full_name, email)
VALUES ('Priya Mehta', 'priya@college.com');

The database automatically generates the IDs.

---

# 12. Reading Data

To display all students:

SELECT * FROM students;

To find one particular student:

SELECT * FROM students
WHERE student_id = 2;

The "WHERE" condition reduces the result to records that satisfy the given condition.

---

# 13. Primary Key

A primary key uniquely identifies a record inside a table.

For example:

student_id
101
102
103

Every ID must be different.

A primary key is useful because the database can quickly identify a specific row.

---

# 14. Foreign Key

A foreign key is used to create a connection between two tables.

For example, I created another table for courses:
``` 
CREATE TABLE courses (
    course_id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT NOT NULL,
    course_name VARCHAR(100) NOT NULL,
    FOREIGN KEY (student_id)
        REFERENCES students(student_id)
);
```
Here:

courses.student_id
       ↓
students.student_id

The "student_id" inside "courses" refers to an existing student.

This prevents us from accidentally assigning a course to a student ID that does not exist.

---

# 15. Why Foreign Keys are Useful

Without a foreign key, an application could accidentally store something like:

student_id = 9999

even when student 9999 does not exist.

A foreign key allows MySQL to enforce the relationship between the tables.

This improves data consistency.

---

# 16. Indexing

An index can be compared with the index page of a textbook.

Suppose a book contains 500 pages and I want to find a particular topic.

Instead of checking every page, I can use the index to locate the required page quickly.

Database indexes work in a similar way.

Without a useful index, MySQL may need to examine many rows to locate matching data.

With an index, the database can locate records much more efficiently.

A primary key normally has an index associated with it.

We can also create an index on another frequently searched column:

CREATE INDEX idx_student_email
ON students(email);

Now searches involving the email column can benefit from the index.

---

# 17. Disadvantage of Indexes

Indexes are useful, but adding indexes to every column is not a good idea.

Why?

Whenever a record is inserted, updated, or deleted, the related indexes may also need to be updated.

Therefore:
```
More indexes
     ↓
Faster certain searches
     +
More work during data modification
```
Indexes should mainly be added where they provide a real performance benefit.

---

# 18. Joining Tables

Sometimes the information we need is divided between multiple tables.

For example:

students

student_id| full_name
         1| Neeraj Sharma
         2| Priya Mehta

courses
```
course_id| student_id| course_name
        1|          1| Java
        2|          2| Python
```
If I want the student's name and course together, I can use a JOIN.
```
SELECT students.full_name, courses.course_name
FROM courses
INNER JOIN students
ON courses.student_id = students.student_id;
```
The result can look like:
```
    full_name| course_name
Neeraj Sharma| Java
``Priya Mehta| Python
```
The JOIN matches the foreign key with the primary key.

---

# 19. Types of JOIN

# INNER JOIN

Returns only records that have a matching record in both tables.
```
SELECT students.full_name, courses.course_name
FROM students
INNER JOIN courses
ON students.student_id = courses.student_id;
```
If a student has no course, that student will not appear in this result.

---

# LEFT JOIN

Returns every record from the left table.
```
SELECT students.full_name, courses.course_name
FROM students
LEFT JOIN courses
ON students.student_id = courses.student_id;
```
If a student has no course, the course column will contain "NULL".

Useful when I want:

«All students, including students who have not selected a course.»

---

# RIGHT JOIN

It keeps all records from the right-side table.
```
SELECT students.full_name, courses.course_name
FROM students
RIGHT JOIN courses
ON students.student_id = courses.student_id;
```
It is basically the reverse idea of a LEFT JOIN.

---

# FULL OUTER JOIN

A FULL OUTER JOIN returns matching records plus unmatched records from both tables.

MySQL does not provide "FULL OUTER JOIN" directly in the same way some other database systems do.

It can be achieved using a combination such as "LEFT JOIN", "RIGHT JOIN", and "UNION".

---

# 20. Easy JOIN Memory Trick

INNER → only matching records

LEFT  → keep everything from left table

RIGHT → keep everything from right table

FULL  → keep everything from both sides

---

# 21. SQL

SQL stands for Structured Query Language.

It is used to communicate with relational databases.

Using SQL, we can:

- Create databases
- Create tables
- Add records
- Modify records
- Remove records
- Search records
- Join multiple tables

---

# 22. SQL Command Categories

DDL – Data Definition Language

Used to create or modify database structures.

Examples:

CREATE
ALTER
DROP
TRUNCATE

---

DML – Data Manipulation Language

Used for changing records.

Examples:

INSERT
UPDATE
DELETE

---

DQL – Data Query Language

Used to retrieve information.

Main command:

SELECT

---

DCL – Data Control Language

Used for database permissions.

Examples:

GRANT
REVOKE

---

TCL – Transaction Control Language

Used for controlling transactions.

Examples:

COMMIT
ROLLBACK
SAVEPOINT

---

# 23. MySQL Data Types

Choosing the correct data type is important because it affects storage and database performance.

# VARCHAR

Used for variable-length text.

username VARCHAR(60)

Useful for:

- Names
- Emails
- City names
- Addresses

# TEXT

Used when the text can be much longer.

about_me TEXT

Useful for:

- Descriptions
- Articles
- Comments

# BOOLEAN

Used for true/false values.

is_verified BOOLEAN

MySQL internally represents BOOLEAN values using a numeric type.

# TINYINT

Used for small integer values.

age TINYINT

# ENUM

Used when only selected values should be allowed.

status ENUM('Active', 'Inactive', 'Pending')

This can help maintain consistent values.

---

# 24. SQL vs NoSQL
```
                                  SQL| NoSQL
     Data is usually stored in tables| Data can use documents, key-value structures, graphs, etc.
      Generally uses a defined schema| Usually provides more flexible structures
                Uses SQL for querying| Query method depends on the database
Relationships and JOINs are important| Applications often design data to reduce joins
  Good for structured relational data| Useful for flexible or rapidly changing data
                    MySQL, PostgreSQL| MongoDB, Cassandra
```
Neither approach is automatically better for every project. The correct choice depends on the application's requirements.

---

# 25. ORM

ORM stands for Object Relational Mapping.

An ORM allows developers to interact with database tables using programming-language objects and methods.

For example, using Sequelize:

Student.findAll();

This can represent an operation similar to:

SELECT * FROM students;

The ORM generates SQL behind the scenes.

---

# 26. Why SQL is Still Important

Even when an ORM such as Sequelize is used, understanding SQL is necessary.

SQL knowledge helps developers:

- Understand generated queries
- Find database errors
- Improve slow queries
- Create complicated JOINs
- Write custom queries
- Understand database performance
- Perform better in technical interviews

So an ORM does not replace SQL knowledge.

It is better to understand both.

---

# 27. Basic Security Lessons

A backend developer should never assume that information coming from the browser is trustworthy.

Anything running in the browser can potentially be inspected or modified by the user.

Therefore:
```
Browser
   ↓
Request
   ↓
Server-side validation
   ↓
Database
```
The server should verify important information before storing or processing it.

Sensitive information such as passwords, private keys, and database credentials should never be exposed in frontend JavaScript.

---

# 28. Handling Repeated Requests

A public API can receive requests from automated programs instead of only normal users.

For example, an unprotected endpoint could receive thousands of requests very quickly.

A backend can reduce this problem using:

- Authentication
- Rate limiting
- Input validation
- Duplicate checks
- CAPTCHA where appropriate
- Request monitoring

The important idea is:

«A backend should be designed assuming that users and automated programs may send unexpected requests.»

---

# 29. Ethical Hacking

An ethical hacker uses security knowledge with authorization to identify weaknesses and help fix them.

The major difference is permission and purpose.
```
Ethical Security Researcher| Malicious Attacker
   Works with authorization| Acts without permission
    Reports vulnerabilities| Exploits vulnerabilities
     Helps improve security| Attempts to cause harm or gain
   Follows legal boundaries| Can face legal consequences
```
The same technical knowledge can be used in very different ways.

---

# 30. What the Browser Can Reveal

The frontend consists mainly of:

- HTML
- CSS
- JavaScript
- Browser storage

Users can inspect the code delivered to their browser.

Therefore, frontend code should not contain:

- Database passwords
- Private API keys
- Secret credentials
- Server-only authentication decisions

A useful rule is:

«If the browser receives it, assume the user can inspect it.»

Important security decisions must be made on the server.

---

# 31. Final Learning

Today's class helped me understand why databases are important for backend development.

The main concepts I learned are:

1. Data is information that can be stored and processed.
2. A database organizes large amounts of information.
3. DBMS software manages databases.
4. RDBMS stores related information using tables.
5. Primary keys uniquely identify records.
6. Foreign keys create relationships between tables.
7. Indexes can make frequently used searches faster.
8. SQL is used to communicate with relational databases.
9. JOINs allow information from related tables to be retrieved together.
10. Data types define what kind of information a column can store.
11. SQL and NoSQL are suitable for different types of applications.
12. ORMs make database interaction easier but do not eliminate the need to understand SQL.
13. Backend systems should validate and protect information on the server.
14. Security should be considered while designing APIs and databases.

#Next Step

My next practical step is to replace JSON-based storage in my resume API with MySQL.

The project can be structured using separate tables, for example:

resume_db
   |
   ├── users
   |
   ├── resumes
   |
   └── skills

The tables can then be connected using foreign keys, and JOIN queries can be used to retrieve complete resume information.

This will give me practical experience with database design, relationships, indexing and SQL while also making my backend project more realistic.

# 👨‍💻 Author
Latesh Padaliya

🎓 B.Tech Computer Science Engineering Student

🌱 Aspiring Full Stack Developer

GitHub: https://github.com/LateshDev

LinkedIn: https://www.linkedin.com/in/latesh-padaliya

⭐ Support
If you like this project, consider giving it a ⭐ on GitHub
