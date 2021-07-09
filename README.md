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
  
## 9. GROCERY STORE: Support Local Products

:scroll: Problem
You are the owner of the Grocery Store. All your products are in the database, that you have created after CodeWars SQL excercises!:)

You care about local market, and want to check how many products come from United States of America or Canada.

Please use SELECT statement and IN to filter out other origins.

In the results show how many products are from United States of America and Canada respectively.

Order by number of products, descending.

products table schema
id
name
price
stock
producer
country
  
results table schema
products
country

:rocket: Solution
  
```sql
CREATE count(*) AS products, country
FROM products
WHERE country IN ('United States of America', 'Canada')
GROUP BY country
ORDER BY count(*) DESC;
```
  
---

**[⬆ Back to Top](#practice-sql)**

## 10. SQL Basics: Simple FULL TEXT SEARCH

:scroll: Problem
  
For this challenge you need to create a simple SELECT statement. Your task is to create a query and do a FULL TEXT SEARCH. You must search the product table on the field name for the word Awesome and return each row with the given word. Your query MUST contain to_tsvector and to_tsquery PostgreSQL functions.

:rocket: Solution
  
```sql
SELECT * FROM products
WHERE name ~ 'Awesome'
```
  
---

**[⬆ Back to Top](#practice-sql)**
  
## 11. SQL Statistics: MIN, MEDIAN, MAX

:scroll: Problem
  
For this challenge you need to create a simple SELECT statement. Your task is to calculate the MIN, MEDIAN and MAX scores of the students from the results table.

:rocket: Solution
  
```sql
SELECT
  MIN(score) AS min,
  percentile_cont(0.5) WITHIN GROUP (ORDER BY score) AS median, 
  MAX(score)
FROM result;
```
  
---

**[⬆ Back to Top](#practice-sql)**

## 12. Relational division: Find all movies two actors cast in together

:scroll: Problem
  
Given film_actor and film tables from the DVD Rental sample database find all movies both Sidney Crowe (actor_id = 105) and Salma Nolte (actor_id = 122) cast in together and order the result set alphabetically.

```
film schema
 Column     | Type                        | Modifiers
------------+-----------------------------+----------
title       | character varying(255)      | not null
film_id     | smallint                    | not null

film_actor schema
 Column     | Type                        | Modifiers
------------+-----------------------------+----------
actor_id    | smallint                    | not null
film_id     | smallint                    | not null
last_update | timestamp without time zone | not null 

actor schema
 Column     | Type                        | Modifiers
------------+-----------------------------+----------
actor_id    | integer                     | not null 
first_name  | character varying(45)       | not null
last_name   | character varying(45)       | not null
last_update | timestamp without time zone | not null 
The desired output:

title
-------------
Film Title 1
Film Title 2
...
```

:rocket: Solution
  
```sql
SELECT f.title
FROM film f
JOIN film_actor fa ON fa.film_id = f.film_id
WHERE fa.actor_id IN (105, 122)
GROUP BY f.film_id
HAVING COUNT(*) = 2
ORDER BY f.title
```

```sql
SELECT f.title
FROM film f
INNER JOIN film_actor fa1 ON fa1.film_id = f.film_id
INNER JOIN film_actor fa2 ON fa2.film_id = f.film_id
WHERE fa1.film_id = 105 AND fa2.film_id = 122
ORDER BY f.title
```
---

**[⬆ Back to Top](#practice-sql)**

## 13. Calculating Running Total

:scroll: Problem
  
Description
Given a posts table that contains a created_at timestamp column write a query that returns date (without time component), a number of posts for a given date and a running (cumulative) total number of posts up until a given date. The resulting set should be ordered chronologically by date.

Desired Output
The resulting set should look similar to the following:

```
date       | count | total
-----------+-------+-------
2017-01-26 |    20 |    20
2017-01-27 |    17 |    37
2017-01-28 |     7 |    44
2017-01-29 |     8 |    52
...
```
date - (DATE) date
count - (INT) number of posts for a date
total - (INT) a running (cumulative) number of posts up until a date

:rocket: Solution
  
```sql
SELECT 
  created_at::date AS date, 
  count(*), 
  SUM(COUNT(*)) OVER (ORDER BY created_at::date ROWS UNBOUNDED PRECEDING)::int AS total
FROM posts
GROUP BY date
ORDER BY date;
```
---

**[⬆ Back to Top](#practice-sql)**
  
## 14. Calculating Month-Over-Month Percentage Growth Rate

:scroll: Problem

Given a posts table that contains a created_at timestamp column write a query that returns a first date of the month, a number of posts created in a given month and a month-over-month growth rate.

The resulting set should be ordered chronologically by date.

Note:

percent growth rate can be negative
percent growth rate should be rounded to one digit after the decimal point and immediately followed by a percent symbol "%". See the desired output below for the reference.
Desired Output
The resulting set should look similar to the following:

```
date       | count | percent_growth
-----------+-------+---------------
2016-02-01 |   175 |           null
2016-03-01 |   338 |          93.1%
2016-04-01 |   345 |           2.1%
2016-05-01 |   295 |         -14.5%
2016-06-01 |   330 |          11.9%
...
```
date - (DATE) a first date of the month
count - (INT) a number of posts in a given month
percent_growth - (TEXT) a month-over-month growth rate expressed in percents
:rocket: Solution
  
```sql
WITH temp AS (
  SELECT
    to_char(created_at::date, 'YYYY-MM-01')::date AS date,
    COUNT(*) AS count
  FROM posts
  GROUP BY date
  ORDER BY date
)

SELECT 
  date,
  count,
  to_char((count - LAG(count) OVER ()) / LAG(count) OVER ()::float * 100, 'FM990.0%') AS percent_growth
FROM temp;
```
---

**[⬆ Back to Top](#practice-sql)**
