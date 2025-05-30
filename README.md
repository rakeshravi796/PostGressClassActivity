# PostGressClassActivity
```
postgres=# CREATE TABLE students (
postgres(#
postgres(#      student_id INTEGER,
postgres(#      first_name VARCHAR(50),
postgres(#      last_name VARCHAR(50),
postgres(#      email VARCHAR(100),
postgres(#      date_of_birth DATE
postgres(# );
CREATE TABLE
postgres=# SELECT * from students;
 student_id | first_name | last_name | email | date_of_birth
------------+------------+-----------+-------+---------------
(0 rows)


postgres=# ALTER TABLE students ADD COLUMN enrollment_date DATE;
ALTER TABLE
postgres=# ALTER TABLE students ADD COLUMN enrollment_dates DATE;
ALTER TABLE
postgres=# ALTER TABLE students DELETE COLUMN enrollment_dates;
ERROR:  syntax error at or near "DELETE"
LINE 1: ALTER TABLE students DELETE COLUMN enrollment_dates;
                             ^
postgres=# ALTER TABLE students DROP COLUMN enrollment_dates;
ALTER TABLE
postgres=# SELECT * from students;
 student_id | first_name | last_name | email | date_of_birth | enrollment_date
------------+------------+-----------+-------+---------------+-----------------
(0 rows)


postgres=# \d
          List of relations
 Schema |   Name   | Type  |  Owner
--------+----------+-------+----------
 public | students | table | postgres
(1 row)


postgres=# \d "minfy-training"
Did not find any relation named ""minfy-training"".
postgres=# \d "minfy-training";
Did not find any relation named ""minfy-training"".
postgres=#
postgres=#
postgres=#
postgres=#
postgres=#
postgres=#
postgres=#
postgres=#
postgres=#
postgres=#
postgres=#
postgres=#
postgres=#
postgres=# ALTER TABLE students DROP COLUMN enrollment_date;
ALTER TABLE
postgres=# ALTER TABLE RENAME COLUMN date_of_birth TO dob;
ERROR:  syntax error at or near "COLUMN"
LINE 1: ALTER TABLE RENAME COLUMN date_of_birth TO dob;
                           ^
postgres=# ALTER TABLE students RENAME COLUMN date_of_birth TO dob;
ALTER TABLE
postgres=# ALTER TABLE students ADD CONSTRAINT unique_email UNIQUE(email);
ALTER TABLE
postgres=# ALTER TABLE students ADD COLUMN phone_number VARCHAR(20);
ALTER TABLE
postgres=# DESCRIBE students;
ERROR:  syntax error at or near "DESCRIBE"
LINE 1: DESCRIBE students;
        ^
postgres=# \d students;
                        Table "public.students"
    Column    |          Type          | Collation | Nullable | Default
--------------+------------------------+-----------+----------+---------
 student_id   | integer                |           |          |
 first_name   | character varying(50)  |           |          |
 last_name    | character varying(50)  |           |          |
 email        | character varying(100) |           |          |
 dob          | date                   |           |          |
 phone_number | character varying(20)  |           |          |
Indexes:
    "unique_email" UNIQUE CONSTRAINT, btree (email)


postgres=# ALTER TABLE students DROP COLUMN phone_number;
ALTER TABLE
postgres=# \d students;
                       Table "public.students"
   Column   |          Type          | Collation | Nullable | Default
------------+------------------------+-----------+----------+---------
 student_id | integer                |           |          |
 first_name | character varying(50)  |           |          |
 last_name  | character varying(50)  |           |          |
 email      | character varying(100) |           |          |
 dob        | date                   |           |          |
Indexes:
    "unique_email" UNIQUE CONSTRAINT, btree (email)


postgres=# INSERT INTO students (student_id, first_name, last_name, email, dob)
postgres-# VALUES (1, 'Alice', 'Smith', 'alice.smith@example.com', '2003-05-15');
INSERT 0 1
postgres=# INSERT INTO students (student_id, first_name, last_name, email, dob)
postgres-# VALUES (2, 'Bob', 'Johnson', 'bob.johnson@example.com', '2002-08-22'),
postgres-#        (3, 'Charlie', 'Brown', 'charlie.brown@example.com', '2003-01-10');
INSERT 0 2
postgres=#
postgres=# INSERT INTO students VALUES (4 , 'rakesh' , 'r' , a@b.com , '2003-08-29');
ERROR:  column "a" does not exist
LINE 1: INSERT INTO students VALUES (4 , 'rakesh' , 'r' , a@b.com , ...
                                                          ^
postgres=# INSERT INTO students VALUES (4 , 'rakesh' , 'r' ,' a@b.com' , '2003-08-29');
INSERT 0 1
postgres=# INSERT INTO students VALUES (5 , 'acd' , 'b' ,' a@b.com' , '2003-08-19');
ERROR:  duplicate key value violates unique constraint "unique_email"
DETAIL:  Key (email)=( a@b.com) already exists.
postgres=# INSERT INTO students VALUES (5 , 'acd' , 'b' ,' ca@b.com' , '2003-08-19');
INSERT 0 1
postgres=# SELECT * from students;
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          1 | Alice      | Smith     | alice.smith@example.com   | 2003-05-15
          2 | Bob        | Johnson   | bob.johnson@example.com   | 2002-08-22
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
          4 | rakesh     | r         |  a@b.com                  | 2003-08-29
          5 | acd        | b         |  ca@b.com                 | 2003-08-19
(5 rows)


postgres=# SELECT * FROM students WHERE student_id = 2;
 student_id | first_name | last_name |          email          |    dob
------------+------------+-----------+-------------------------+------------
          2 | Bob        | Johnson   | bob.johnson@example.com | 2002-08-22
(1 row)


postgres=# SELECT * FROM students WHERE dob < '2003-01-01';;
 student_id | first_name | last_name |          email          |    dob
------------+------------+-----------+-------------------------+------------
          2 | Bob        | Johnson   | bob.johnson@example.com | 2002-08-22
(1 row)


postgres=# SELECT * FROM students WHERE first_name LIKE 'B%' OR 'C%';
ERROR:  invalid input syntax for type boolean: "C%"
LINE 1: SELECT * FROM students WHERE first_name LIKE 'B%' OR 'C%';
                                                             ^
postgres=# SELECT * FROM students WHERE first_name LIKE 'B%' OR first_name LIKE 'C%';
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          2 | Bob        | Johnson   | bob.johnson@example.com   | 2002-08-22
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
(2 rows)


postgres=# SELECT * FROM students WHERE email LIKE '%example.com';
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          1 | Alice      | Smith     | alice.smith@example.com   | 2003-05-15
          2 | Bob        | Johnson   | bob.johnson@example.com   | 2002-08-22
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
(3 rows)


postgres=# SELECT * FROM students WHERE email IS NULL;
 student_id | first_name | last_name | email | dob
------------+------------+-----------+-------+-----
(0 rows)


postgres=# UPDATE students dob = '2003-06-27' WHERE student_id = 1;
ERROR:  syntax error at or near "="
LINE 1: UPDATE students dob = '2003-06-27' WHERE student_id = 1;
                            ^
postgres=# UPDATE students SET  dob = '2003-06-27' WHERE student_id = 1;
UPDATE 1
postgres=# Microsoft Windows [Version 10.0.19045.5854]
(c) Microsoft Corporation. All rights reserved.

C:\Program Files\PostgreSQL\17\pgAdmin 4\runtime>"
C:\Program Files\PostgreSQL\17\pgAdmin 4\runtime\p
sql.exe" "host=localhost port=5432 dbname=postgres
 user=postgres sslmode=prefer connect_timeout=10"
2>>&1
psql (17.5)
WARNING: Console code page (437) differs from Wind
ows code page (1252)
         8-bit characters might not work correctly
. See psql reference
         page "Notes for Windows users" for detail
s.
Type "help" for help.

postgres=# SELECT * from students order by last_na
me asc
postgres-# SELECT * from students order by last_na
me asc;
ERROR:  syntax error at or near "SELECT"
LINE 2: SELECT * from students order by last_name
asc;
        ^
postgres=# SELECT * from students order by last_na
me asc;
 student_id | first_name | last_name |           e
mail           |    dob
------------+------------+-----------+------------
---------------+------------
          5 | acd        | b         |  ca@b.com
               | 2003-08-19
          3 | Charlie    | Brown     | charlie.bro
wn@example.com | 2003-01-10
          2 | Bob        | Johnson   | bob.johnson
@example.com   | 2002-08-22
          4 | rakesh     | r         |  a@b.com
               | 2003-08-29
          1 | Alice      | Smith     | alice.smith
@example.com   | 2003-06-27
(5 rows)


postgres=# SELECT * from students order by last_na
me desc;
 student_id | first_name | last_name |           e
mail           |    dob
------------+------------+-----------+------------
---------------+------------
          1 | Alice      | Smith     | alice.smith
@example.com   | 2003-06-27
          4 | rakesh     | r         |  a@b.com
               | 2003-08-29
          2 | Bob        | Johnson   | bob.johnson
@example.com   | 2002-08-22
          3 | Charlie    | Brown     | charlie.bro
wn@example.com | 2003-01-10
          5 | acd        | b         |  ca@b.com
               | 2003-08-19
(5 rows)


postgres=# SELECT * from students order by last_na
me desc , first_name Asc;
 student_id | first_name | last_name |           e
mail           |    dob
------------+------------+-----------+------------
---------------+------------
          1 | Alice      | Smith     | alice.smith
@example.com   | 2003-06-27
          4 | rakesh     | r         |  a@b.com
               | 2003-08-29
          2 | Bob        | Johnson   | bob.johnson
@example.com   | 2002-08-22
          3 | Charlie    | Brown     | charlie.bro
wn@example.com | 2003-01-10
          5 | acd        | b         |  ca@b.com
               | 2003-08-19
(5 rows)


postgres=# SELECT * from students order by dob asc
 LIMIT(2);
 student_id | first_name | last_name |           e
mail           |    dob
------------+------------+-----------+------------
---------------+------------
          2 | Bob        | Johnson   | bob.johnson
@example.com   | 2002-08-22
          3 | Charlie    | Brown     | charlie.bro
wn@example.com | 2003-01-10
(2 rows)


postgres=# SELECT DISTINCT YEAR(dob) AS Unique Yea
rs FROm students;
ERROR:  syntax error at or near "Years"
LINE 1: SELECT DISTINCT YEAR(dob) AS Unique Years
FROm students;
                                            ^
postgres=# SELECT DISTINCT YEAR(dob) AS UniqueYear
s FROm students;
ERROR:  function year(date) does not exist
LINE 1: SELECT DISTINCT YEAR(dob) AS UniqueYears F
ROm students;
                        ^
HINT:  No function matches the given name and argu
ment types. You might need to add explicit type ca
sts.
postgres=# INSERT INTO students VALUES(1 , "na" ,
"b")
postgres-# ;
ERROR:  column "na" does not exist
LINE 1: INSERT INTO students VALUES(1 , "na" , "b"
)
                                        ^
postgres=# INSERT INTO students VALUES(1 , "na" ,
"b" , ,)
postgres-# ;
ERROR:  syntax error at or near ","
LINE 1: INSERT INTO students VALUES(1 , "na" , "b"
 , ,)

   ^
postgres=# INSERT INTO students VALUES(1 , "na" ,
"b" , ,);
ERROR:  syntax error at or near ","
LINE 1: INSERT INTO students VALUES(1 , "na" , "b"
 , ,);

   ^
postgres=# INSERT INTO students (student_id, first
_name, last_name, email, dob)
postgres-# VALUES (1, 'Alice', 'Smith', 'alice.smi
th@example.com', '2003-05-15');
ERROR:  duplicate key value violates unique constr
aint "unique_email"
DETAIL:  Key (email)=(alice.smith@example.com) alr
eady exists.
postgres=# INSERT INTO students (student_id, first
_name, last_name, email, dob)
postgres-# VALUES (1, 'Alice', 'Smith', 'alice.smi
th@example.com', '2003-05-15');
ERROR:  duplicate key value violates unique constr
aint "unique_email"
DETAIL:  Key (email)=(alice.smith@example.com) alr
eady exists.
postgres=# INSERT INTO students (student_id, first
_name, last_name, email, dob)
postgres-# INSERT INTO students (student_id, first
_name, last_name, email, dob)
postgres-# VALUES (1, 'Alice', 'Smith', 'alice.smi
th@example.com', '2003-05-15')
postgres-# ;
ERROR:  syntax error at or near "INSERT"
LINE 2: INSERT INTO students (student_id, first_na
me, last_name, ema...
        ^
postgres=# SELECT COUNT(first_name) FROM students;

 count
-------
     5
(1 row)


postgres=# SELECT min(dob) from students;
    min
------------
 2002-08-22
(1 row)


postgres=# SELECT COUNT(DISTINCT(last_name) FROM s
tudents;
postgres(# ;
postgres(# )
postgres-# ;
ERROR:  syntax error at or near "FROM"
LINE 1: SELECT COUNT(DISTINCT(last_name) FROM stud
ents;
                                         ^
postgres=# SELECT COUNT(DISTINCT(last_name)) FROM
students;
 count
-------
     5
(1 row)


postgres=# SELECT * from students;
 student_id | first_name | last_name |           e
mail           |    dob
------------+------------+-----------+------------
---------------+------------
          2 | Bob        | Johnson   | bob.johnson
@example.com   | 2002-08-22
          3 | Charlie    | Brown     | charlie.bro
wn@example.com | 2003-01-10
          4 | rakesh     | r         |  a@b.com
               | 2003-08-29
          5 | acd        | b         |  ca@b.com
               | 2003-08-19
          1 | Alice      | Smith     | alice.smith
@example.com   | 2003-06-27
(5 rows)


postgres=# SELECT date_part('YEAR,dob) AS year , C
OUNT(*)
postgres'# FROM students
postgres'# GROUP BY date_part('YEAR
postgres(# ;
postgres(# );
ERROR:  syntax error at or near "YEAR"
LINE 3: GROUP BY date_part('YEAR
                            ^
postgres=# SELECT date_part('YEAR',dob) AS year ,
COUNT(*)
postgres-# FROM students
postgres-# GROUP BY date_part('YEAR' , dob);
 year | count
------+-------
 2002 |     1
 2003 |     4
(2 rows)


postgres=# SELECT name , COUNT(*)
postgres-# FROM students
postgres-# GROUP BYdents;
ERROR:  syntax error at or near "BYdents"
LINE 3: GROUP BYdents;
              ^
postgres=# SELECT first_name , COUNT(*)
postgres-# FROM students
postgres-# GROUP BY first_name;
 first_name | count
------------+-------
 acd        |     1
 Alice      |     1
 Bob        |     1
 Charlie    |     1
 rakesh     |     1
(5 rows)


postgres=# DROP TABLE IF EXISTS students
postgres-# ;
DROP TABLE
postgres=# CREATE TABLE students(
postgres(# student_id SERIAL PRIMARY KEY,
postgres(# first_name VARCHAR(50) NOT NULL,
postgres(# last_name VARCHAR(50) NOT NULL,
postgres(# email VARCHAR(100) UNIQUE,
postgres(# dob DATE,
postgres(# enrollment_status VARCHAR(50) CHECK (en
rollment_status IN ('enrolled' , 'graduated' , 'dr
opped' , 'pending')));
CREATE TABLE
postgres=# INSERT INTO students (first_name, last_
name, email, dob, enrollment_status)
postgres-# VALUES ('Alice', 'Smith', 'alice.smith@
example.com', '2003-05-15', 'enrolled');
INSERT 0 1
postgres=# INSERT INTO students (first_name, last_
name, email, dob, enrollment_status)
postgres-# VALUES ('Robert', 'Johnson', 'robert.j@
example.com', '2002-08-22', 'enrolled');
INSERT 0 1
postgres=# INSERT INTO students (first_name, last_
name, email, dob, enrollment_status)
postgres-# VALUES ('Charlie', 'Brown', 'charlie.br
own@example.com', '2003-01-10', 'pending');
INSERT 0 1
postgres=# SELECT * from students;
 student_id | first_name | last_name |           e
mail           |    dob     | enrollment_status
------------+------------+-----------+------------
---------------+------------+-------------------
          1 | Alice      | Smith     | alice.smith
@example.com   | 2003-05-15 | enrolled
          2 | Robert     | Johnson   | robert.j@ex
ample.com      | 2002-08-22 | enrolled
          3 | Charlie    | Brown     | charlie.bro
wn@example.com | 2003-01-10 | pending
(3 rows)


postgres=# INSERT INTO students (first_name, last_
name,dob, enrollment_status) VALUES ('Charlie', 'B
rown', '2003-01-10', 'pending');
INSERT 0 1
postgres=# INSERT INTO students (first_name, last_
name, email, dob, enrollment_status)
postgres-# VALUES ('Charlie', 'Brown', 'charlie.br
own@example.com', '2003-01-10', 'pending');
ERROR:  duplicate key value violates unique constr
aint "students_email_key"
DETAIL:  Key (email)=(charlie.brown@example.com) a
lready exists.
postgres=# INSERT INTO students (last_name, email,
 dob, enrollment_status) VALUES ('Brown', 'charlie
.brown@example.com', '2003-01-10', 'pending');
ERROR:  null value in column "first_name" of relat
ion "students" violates not-null constraint
DETAIL:  Failing row contains (6, null, Brown, cha
rlie.brown@example.com, 2003-01-10, pending).
postgres=#
postgres=#
postgres=#
postgres=# CREATE TABLE courses(
postgres(#
postgres(# course_id SERIAL PRIMARY KEY,
postgres(# course_name VARCHAR(100) NOT NULL UNIQU
E,
postgres(# credits INTEGER CHECK (credits>0 AND cr
edits<10));
CREATE TABLE
postgres=# SELECT * FROM courses;
 course_id | course_name | credits
-----------+-------------+---------
(0 rows)


postgres=# INSERT INTO courses (course_name, credi
ts) VALUES ('Introduction to SQL', 3);
INSERT 0 1
postgres=# INSERT INTO courses (course_name, credi
ts) VALUES ('Database Design', 4);
INSERT 0 1
postgres=# INSERT INTO courses (course_name, credi
ts) VALUES ('Web Development', 3);
INSERT 0 1
postgres=# INSERT INTO courses (course_name, credi
ts) VALUES ('Data Structures', 4);
INSERT 0 1
postgres=# CREATE TABLE enrollments (
postgres(#     enrollment_id SERIAL PRIMARY KEY,
postgres(#     student_id INTEGER NOT NULL,
postgres(#     course_id INTEGER NOT NULL,
postgres(#     enrollment_date DATE DEFAULT CURREN
T_DATE, -- Sets default if not specified
postgres(#     grade CHAR(1) CHECK (grade IN ('A',
 'B', 'C', 'D', 'F', 'W', NULL)), -- W for withdra
w, NULL if not graded
postgres(#
postgres(#     -- Foreign Key constraint for stude
nt_id
postgres(#     CONSTRAINT fk_student
postgres(#         FOREIGN KEY (student_id)
postgres(#         REFERENCES students(student_id)

postgres(#         ON DELETE CASCADE, -- If a stud
ent is deleted, their enrollments are also deleted
.
postgres(#                            -- Other opt
ions: ON DELETE RESTRICT, ON DELETE SET NULL, ON D
ELETE SET DEFAULT
postgres(#
postgres(#     -- Foreign Key constraint for cours
e_id
postgres(#     CONSTRAINT fk_course
postgres(#         FOREIGN KEY (course_id)
postgres(#         REFERENCES courses(course_id)
postgres(#         ON DELETE RESTRICT, -- (Default
 if not specified) Prevent deleting a course if st
udents are enrolled.
postgres(#
postgres(#     -- Ensure a student cannot enroll i
n the same course multiple times (if semester isn'
t a factor)
postgres(#     UNIQUE (student_id, course_id)
postgres(# );
CREATE TABLE
postgres=# INSERT INTO enrollments (student_id, co
urse_id, grade) VALUES (999, 1, 'A');
ERROR:  insert or update on table "enrollments" vi
olates foreign key constraint "fk_student"
DETAIL:  Key (student_id)=(999) is not present in
table "students".
postgres=# INSERT INTO enrollments (student_id, co
urse_id, grade) VALUES (1, 800, 'A');
ERROR:  insert or update on table "enrollments" vi
olates foreign key constraint "fk_course"
DETAIL:  Key (course_id)=(800) is not present in t
able "courses".
postgres=# INSERT INTO enrollments (student_id, co
urse_id, grade) VALUES (1, 1, 'A');
INSERT 0 1
postgres=# INSERT INTO enrollments (student_id, co
urse_id) VALUES (1, 2);
INSERT 0 1
postgres=# INSERT INTO enrollments (student_id, co
urse_id, grade) VALUES (2, 1, 'B');
INSERT 0 1
postgres=# SELECT * FROM students WHERE student_id
 = 1;SELECT * FROM enrollments WHERE student_id =
1;DELETE FROM students WHERE student_id = 1; -- Al
ice
 student_id | first_name | last_name |          em
ail          |    dob     | enrollment_status
------------+------------+-----------+------------
-------------+------------+-------------------
          1 | Alice      | Smith     | alice.smith
@example.com | 2003-05-15 | enrolled
(1 row)


 enrollment_id | student_id | course_id | enrollme
nt_date | grade
---------------+------------+-----------+---------
--------+-------
             3 |          1 |         1 | 2025-05-
30      | A
             4 |          1 |         2 | 2025-05-
30      |
(2 rows)


DELETE 1
postgres=# SELECT * FROM students WHERE student_id
 = 1; -- Should be gone
 student_id | first_name | last_name | email | dob
 | enrollment_status
------------+------------+-----------+-------+----
-+-------------------
(0 rows)


postgres=# SELECT * FROM enrollments WHERE student
_id = 1; -- Should also be gone (due to ON
 enrollment_id | student_id | course_id | enrollme
nt_date | grade
---------------+------------+-----------+---------
--------+-------
(0 rows)


postgres=#
```

