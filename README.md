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

## 4. Best-Selling Books

:scroll: Problem

You work at a book store. It's the end of the month, and you need to find out the 5 bestselling books at your store. Use a select statement to list names, authors, and number of copies sold of the 5 books which were sold most.

books table schema

name
author
copies_sold

:rocket: Solution
  
```sql
SELECT * FROM books ORDER BY copies_sold DESC LIMIT 5;
```
  
---

**[⬆ Back to Top](#practice-sql)**

## 5. Countries Capitals for Trivia Night

:scroll: Problem

Your friends told you that if you keep coding on your computer, you are going to hurt your eyes. They suggested that you go with them to trivia night at the local club.

Once you arrive at the club, you realize the true motive behind your friends' invitation. They know that you are a computer nerd, and they want you to query the countries table and get the answers to the trivia questions.

Schema of the countries table:

country (String)
capital (String)
continent (String)
The first question: from the African countries that start with the character E, get the names of their capitals ordered alphabetically.

You should only give the names of the capitals. Any additional information is just noise
If you get more than 3, you will be kicked out, for being too smart
Also, this database is crowd-sourced, so sometimes Africa is written Africa and in other times Afrika.

:rocket: Solution
  
```sql
SELECT capital 
FROM countries 
WHERE continent IN ('Africa', 'Afrika') 
  AND country LIKE 'E%' 
ORDER BY capital
LIMIT 3;
```
  
---

**[⬆ Back to Top](#practice-sql)**

## 6. Even or Odd

:scroll: Problem

You will be given a table, numbers, with one column number.

Return a table with a column is_even containing "Even" or "Odd" depending on number column values.

numbers table schema
number INT
output table schema
is_even STRING

:rocket: Solution
  
```sql
SELECT
  CASE
    WHEN number % 2 = 0 THEN 'Even'
    ELSE 'Odd'
  END AS is_even
FROM numbers;
```
  
---

**[⬆ Back to Top](#practice-sql)**
  
## 7. Create a FUNCTION

:scroll: Problem

For this challenge you need to create a basic Increment function which Increments on the age field of the peoples table.

The function should be called increment, it needs to take 1 integer and increment that number by 1.

:rocket: Solution
  
```sql
CREATE FUNCTION increment(age integer) RETURNS integer AS $$
BEGIN
  RETURN age + 1;
END;
$$ LANGUAGE plpgsql
```
  
---

**[⬆ Back to Top](#practice-sql)**

## 8. Simple VIEW

:scroll: Problem

For this challenge you need to create a VIEW. This VIEW is used by a sales store to give out vouches to members who have spent over $1000 in departments that have brought in more than $10000 total ordered by the members id. The VIEW must be called members_approved_for_voucher then you must create a SELECT query using the view.

![image](https://user-images.githubusercontent.com/26533193/123575152-c5e33480-d796-11eb-89e4-d3db074a32ed.png)

:rocket: Solution
  
```sql
CREATE VIEW members_approved_for_voucher AS
SELECT m.id, m.name, m.email, SUM(p.price) AS "total_spending"
FROM members m
INNER JOIN sales s ON s.member_id = m.id
INNER JOIN products p ON p.id = s.product_id
WHERE s.department_id IN (
  SELECT s2.department_id
  FROM sales s2
  INNER JOIN products p2 ON p2.id = s2.product_id
  GROUP BY s2.department_id
  HAVING SUM(p2.price) > 10000
)
GROUP BY m.id
HAVING SUM(p.price) > 1000
ORDER BY m.id;

SELECT * FROM members_approved_for_voucher;
```
  
---

**[⬆ Back to Top](#practice-sql)**
