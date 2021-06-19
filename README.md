# Practice SQL
## 1. Returning Strings

Write a select statement that takes name from person table and return "Hello, <name> how are you doing today?" results in a column named greeting

<details><summary>Solution</summary>
```sql
SELECT format('Hello, %s how are you doing today?', name)
AS greeting
FROM person;
```
</details>
