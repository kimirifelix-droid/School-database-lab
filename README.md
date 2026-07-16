Microsoft Windows [Version 10.0.26200.8737]
(c) Microsoft Corporation. All rights reserved.

C:\Program Files\PostgreSQL\18\pgAdmin 4\runtime>"C:\Program Files\PostgreSQL\18\pgAdmin 4\
runtime\psql.exe" "host=localhost port=5432 dbname=bootcamp user=postgres sslmode=prefer co
nnect_timeout=10" 2>>&1
psql (18.4)
WARNING: Console code page (437) differs from Windows code page (1252)
         8-bit characters might not work correctly. See psql reference
         page "Notes for Windows users" for details.
Type "help" for help.

bootcamp=# SELECT current_database();
 current_database
------------------
 bootcamp
(1 row)


bootcamp=# CREATE TABLE departments (
bootcamp(#     department_id SERIAL PRIMARY KEY,
bootcamp(#     department_name VARCHAR(100) UNIQUE NOT NULL
bootcamp(# );
CREATE TABLE
bootcamp=# SELECT * FROM departments;
 department_id | department_name
---------------+-----------------
(0 rows)


bootcamp=# CREATE TABLE courses (
bootcamp(#     course_id SERIAL PRIMARY KEY,
bootcamp(#     department_id INT NOT NULL,
bootcamp(#     course_title VARCHAR(100) UNIQUE NOT NULL,
bootcamp(#
bootcamp(#     FOREIGN KEY (department_id)
bootcamp(#     REFERENCES departments(department_id)
bootcamp(# );
CREATE TABLE
bootcamp=# CREATE TABLE students (
bootcamp(#     student_id SERIAL PRIMARY KEY,
bootcamp(#     student_name VARCHAR(100) NOT NULL,
bootcamp(#     email VARCHAR(100) UNIQUE
bootcamp(# );
CREATE TABLE
bootcamp=# CREATE TABLE enrollments (
bootcamp(#     enrollment_id SERIAL PRIMARY KEY,
bootcamp(#
bootcamp(#     student_id INT NOT NULL,
bootcamp(#
bootcamp(#     course_id INT NOT NULL,
bootcamp(#
bootcamp(#     grade DECIMAL(5,2),
bootcamp(#
bootcamp(#     FOREIGN KEY(student_id)
bootcamp(#         REFERENCES students(student_id),
bootcamp(#
bootcamp(#     FOREIGN KEY(course_id)
bootcamp(#         REFERENCES courses(course_id),
bootcamp(#
bootcamp(#     UNIQUE(student_id, course_id)
bootcamp(# );
CREATE TABLE
bootcamp=# SELECT table_name
bootcamp-# FROM information_schema.tables
bootcamp-# WHERE table_schema='public';
    table_name
-------------------
 school_schema.sql
 departments
 courses
 students
 enrollments
(5 rows)


bootcamp=# INSERT INTO departments (department_name)
bootcamp-#
bootcamp-# VALUES
bootcamp-#
bootcamp-# ('Computer Science'),
bootcamp-#
bootcamp-# ('Mathematics');
INSERT 0 2
bootcamp=# SELECT * FROM departments;
 department_id | department_name
---------------+------------------
             1 | Computer Science
             2 | Mathematics
(2 rows)


bootcamp=# INSERT INTO courses
bootcamp-#
bootcamp-# (department_id, course_title)
bootcamp-#
bootcamp-# VALUES
bootcamp-#
bootcamp-# (1,'Database Systems'),
bootcamp-#
bootcamp-# (1,'Algorithms'),
bootcamp-#
bootcamp-# (2,'Calculus');
INSERT 0 3
bootcamp=# SELECT * FROM courses;
 course_id | department_id |   course_title
-----------+---------------+------------------
         1 |             1 | Database Systems
         2 |             1 | Algorithms
         3 |             2 | Calculus
(3 rows)


bootcamp=# INSERT INTO students
bootcamp-#
bootcamp-# (student_name,email)
bootcamp-#
bootcamp-# VALUES
bootcamp-#
bootcamp-# ('Kofi Mensah','kofi@gmail.com'),
bootcamp-#
bootcamp-# ('Nia Kamau','nia@gmail.com'),
bootcamp-#
bootcamp-# ('Tariq Diallo','tariq@gmail.com');
INSERT 0 3
bootcamp=# SELECT * FROM students;
 student_id | student_name |      email
------------+--------------+-----------------
          1 | Kofi Mensah  | kofi@gmail.com
          2 | Nia Kamau    | nia@gmail.com
          3 | Tariq Diallo | tariq@gmail.com
(3 rows)


bootcamp=# INSERT INTO enrollments
bootcamp-#
bootcamp-# (student_id,course_id,grade)
bootcamp-#
bootcamp-# VALUES
bootcamp-#
bootcamp-# (1,1,91),
bootcamp-#
bootcamp-# (1,2,84),
bootcamp-#
bootcamp-# (1,3,88),
bootcamp-#
bootcamp-# (2,1,79),
bootcamp-#
bootcamp-# (2,2,92),
bootcamp-#
bootcamp-# (2,3,86),
bootcamp-#
bootcamp-# (3,1,87),
bootcamp-#
bootcamp-# (3,2,81),
bootcamp-#
bootcamp-# (3,3,94);
INSERT 0 9
bootcamp=# SELECT * FROM enrollments;
 enrollment_id | student_id | course_id | grade
---------------+------------+-----------+-------
             1 |          1 |         1 | 91.00
             2 |          1 |         2 | 84.00
             3 |          1 |         3 | 88.00
             4 |          2 |         1 | 79.00
             5 |          2 |         2 | 92.00
             6 |          2 |         3 | 86.00
             7 |          3 |         1 | 87.00
             8 |          3 |         2 | 81.00
             9 |          3 |         3 | 94.00
(9 rows)


bootcamp=# SELECT
bootcamp-#
bootcamp-# s.student_name,
bootcamp-#
bootcamp-# c.course_title,
bootcamp-#
bootcamp-# e.grade
bootcamp-#
bootcamp-# FROM students s
bootcamp-#
bootcamp-# JOIN enrollments e
bootcamp-#
bootcamp-# ON s.student_id=e.student_id
bootcamp-#
bootcamp-# JOIN courses c
bootcamp-#
bootcamp-# ON e.course_id=c.course_id
bootcamp-#
bootcamp-# ORDER BY s.student_name;
 student_name |   course_title   | grade
--------------+------------------+-------
 Kofi Mensah  | Database Systems | 91.00
 Kofi Mensah  | Algorithms       | 84.00
 Kofi Mensah  | Calculus         | 88.00
 Nia Kamau    | Database Systems | 79.00
 Nia Kamau    | Algorithms       | 92.00
 Nia Kamau    | Calculus         | 86.00
 Tariq Diallo | Database Systems | 87.00
 Tariq Diallo | Algorithms       | 81.00
 Tariq Diallo | Calculus         | 94.00
(9 rows)


bootcamp=# SELECT
bootcamp-#
bootcamp-# c.course_title,
bootcamp-#
bootcamp-# AVG(e.grade) AS average_grade,
bootcamp-#
bootcamp-# COUNT(e.student_id) AS enrolled_students
bootcamp-#
bootcamp-# FROM courses c
bootcamp-#
bootcamp-# JOIN enrollments e
bootcamp-#
bootcamp-# ON c.course_id=e.course_id
bootcamp-#
bootcamp-# GROUP BY c.course_title
bootcamp-#
bootcamp-# HAVING AVG(e.grade)>=80
bootcamp-#
bootcamp-# ORDER BY average_grade DESC;
   course_title   |    average_grade    | enrolled_students
------------------+---------------------+-------------------
 Calculus         | 89.3333333333333333 |                 3
 Database Systems | 85.6666666666666667 |                 3
 Algorithms       | 85.6666666666666667 |                 3
(3 rows)


bootcamp=# SELECT
bootcamp-#
bootcamp-# s.student_name,
bootcamp-#
bootcamp-# c.course_title,
bootcamp-#
bootcamp-# e.grade,
bootcamp-#
bootcamp-# RANK()
bootcamp-#
bootcamp-# OVER(
bootcamp(#
bootcamp(# PARTITION BY c.course_title
bootcamp(#
bootcamp(# ORDER BY e.grade DESC
bootcamp(#
bootcamp(# ) AS course_rank
bootcamp-#
bootcamp-# FROM enrollments e
bootcamp-#
bootcamp-# JOIN students s
bootcamp-#
bootcamp-# ON e.student_id=s.student_id
bootcamp-#
bootcamp-# JOIN courses c
bootcamp-#
bootcamp-# ON e.course_id=c.course_id
bootcamp-#
bootcamp-# ORDER BY c.course_title, course_rank;
 student_name |   course_title   | grade | course_rank
--------------+------------------+-------+-------------
 Nia Kamau    | Algorithms       | 92.00 |           1
 Kofi Mensah  | Algorithms       | 84.00 |           2
 Tariq Diallo | Algorithms       | 81.00 |           3
 Tariq Diallo | Calculus         | 94.00 |           1
 Kofi Mensah  | Calculus         | 88.00 |           2
 Nia Kamau    | Calculus         | 86.00 |           3
 Kofi Mensah  | Database Systems | 91.00 |           1
 Tariq Diallo | Database Systems | 87.00 |           2
 Nia Kamau    | Database Systems | 79.00 |           3
(9 rows)
# School-database-lab
