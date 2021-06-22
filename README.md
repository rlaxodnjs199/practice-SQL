# Practice SQL
## 1. Returning Strings

:scroll: Problem

Write a select statement that takes name from person table and return "Hello, <name> how are you doing today?" results in a column named greeting

:mag_right: Example

```
name = "John" -> greeting = "Hello, John how are you doing today?"
```  

:rocket: Solution
  
```sql
SELECT format('Hello, %s how are you doing today?', name)
AS greeting
FROM person;
```
  
---

**[⬆ Back to Top](#practice-sql)**

## 2. On the Canadian Border

:scroll: Problem

You are a border guard sitting on the Canadian border. You were given a list of travelers who have arrived at your gate today. You know that American, Mexican, and Canadian citizens don't need visas, so they can just continue their trips. You don't need to check their passports for visas! You only need to check the passports of citizens of all other countries!

Select names, and countries of origin of all the travelers, excluding anyone from Canada, Mexico, or The US.

travelers table schema

name
country
NOTE: The United States is written as 'USA' in the table.

:rocket: Solution
  
```sql
SELECT name, country
FROM travelers
WHERE lower(country) NOT IN ('usa', 'canada', 'mexico')
```
  
---

**[⬆ Back to Top](#practice-sql)**

## 3. Register for the Party

:scroll: Problem

You received an invitation to an amazing party. Now you need to write an insert statement to add yourself to the table of participants.

participants table schema

name (string)
age (integer)
attending (boolean)
NOTES:

Since alcohol will be served, you can only attend if you are 21 or older
You can't attend if the attending column returns anything but true

:rocket: Solution
  
```sql
INSERT INTO participants (name, age, attending) VALUES ('twkim', 31, true);
--INSERT INTO participants VALUES ('twkim', 31, true); -> Can omit columns info when inserting all values for all columns in correct order
SELECT * FROM participants
```
  
---

**[⬆ Back to Top](#practice-sql)**
  
## 3. Collect Tuition

:scroll: Problem

You are working for a local school, and you are responsible for collecting tuition from students. You have a list of all students, some of them have already paid tution and some haven't. Write a select statement to get a list of all students who haven't paid their tuition yet. The list should include all the data available about these students.

students table schema

name (string)
age (integer)
semester (integer)
mentor (string)
tuition_received (Boolean)

:rocket: Solution
  
```sql
SELECT *
FROM students
WHERE NOT tuition_received
--Or WHERE tuition_received = False
```
  
---

**[⬆ Back to Top](#practice-sql)**
