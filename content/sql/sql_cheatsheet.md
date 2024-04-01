---
Title: SQL Cheatsheet
Date: 3/28/2024
Tags: sql
slug: sql-cheatsheet
Summary: A Cheatsheet for using SQL
---

## Commands

- `CREATE TABLE <table_name> (<col_name> <col_type>, ...);`
- `INSERT INTO <table_name> (col_name, ...) VALUES (col_value);`
    - If supplying a value for every column, `(col_name, ...)` can be omitted
- `SELECT <col_name>, ... FROM <table_name>;`
- `ALTER TABLE <table_name> ...`
    - `... RENAME TO <new_table_name>;`
    - `... RENAME COLUMN <col_name> TO <new_col_name>;`
    - `... ADD COLUMN <col_name> <col_type>;`
    - `... DROP COLUMN <col_name>;`
- `DELETE FROM <table_name>;` :insert sirens:
- `UPDATE <table_name> SET <col_name> = <new_val>, ...;`
 
## Clauses

- `WHERE`: for filtering
    - `SELECT name FROM users WHERE age > 18;`
    - Accepts `=`, `<`, `<=`, `>`, `>=`, `IS NULL`, `IS NOT NULL`
    - Logical operators `AND` and `OR` can be used to combine filters
- `IN`: for filtering from a list
    - `SELECT * FROM users WHERE country_code IN ("CA", "US", "MX");`
- `LIKE`: for very simple string matching
    - `%` stands for 0 or more characters
    - `_` stands for 1 character
    - `SELECT name FROM products WHERE name LIKE "%banana%";` (name contains "banana")
- `LIMIT`: puts a cap on the number of records returned
    - `SELECT * FROM users WHERE country_code = "US" LIMIT 50;`
- `AS`: for single time column renaming
    - `SELECT employee_id AS id FROM employees;`
    - Useful when calculating new values in a query
- `IIF`: ternary function
    - `SELECT IIF(quantity < 10, "Low Stock", "In Stock") AS stock FROM inventory;`
- `BETWEEN`: for checking ranges
    - `SELECT * FROM users WHERE age BETWEEN 18 AND 65;`
- `DISTINCT`: for removing duplicates in results
    - `SELECT DISTINCT region FROM users;`
- `ORDER BY`: for sorting records
    - `SELECT * FROM users ORDER BY age;`
    - By default sorts by `ASC`, but can also specify `DESC`
    - Must come **before** `LIMIT`
- `GROUP BY`: combines rows into records with matching values
    - `SELECT user_id, sum(amount) FROM transactions GROUP BY user_id;`
- `HAVING`: `WHERE` but for groups instead of individual records
    - `SELECT sum(amount) AS balance FROM transactions GROUP BY user_id HAVING balance > 20;`
 
## Constraints

- `NOT NULL`
    - `CREATE TABLE users(id INTEGER PRIMARY KEY, name TEXT NOT NULL);`
- `PRIMARY KEY` (implies `NOT NULL`)
- `FOREIGN KEY` ties a column to values from another table
    - ```
      CREATE TABLE users(id PRIMARY KEY, country_code TEXT,
      FOREIGN KEY (country_code) REFERENCES countries(code)
      ```
 
## SQLite Data Types

- `NULL`
- `INTEGER`
- `REAL` - as in numbers. AKA float
- `TEXT` - aka strings
- `BLOB` - *B*inary *L*arge *Ob*ject
- `BOOLEAN` - Technically represented as 0/1, like C does

## Aggregators

- `count`: returns an integer count of the number of results that were returned, instead of the actual results
    - `SELECT count(*) FROM users;`
- `sum`: returns an integer sum of the selected records
    - `SELECT sum(amount) FROM transactions WHERE user_id=2;`
- `max`: returns the single largest value from the selected records
    - `SELECT max(amount) FROM transactions;`
- `min`: returns the single smallest value from the selected records
    - `SELECT min(amount) FROM transactions;`
- `avg`: returns the average value from the selected records
    - `SELECT avg(age) from users;`
- `round(<value>, <precision>)`: round a value, where `<precision>` is the number of digits past the decimal point
    - `SELECT round(avg(age)) from users;`

## Subqueries

Insert queries into other queries by surrounding them with parentheses.
```sql
SELECT id, song_name, artist_id
FROM songs
WHERE artist_id IN (
    SELECT id
    FROM artists
    WHERE artist_name LIKE 'Rick%'
);
```

## Joins

- Inner Join: Match each record from the first table with a record from the second table, but only the ones that have a match. This is also the default join.
    - `SELECT * FROM users INNER JOIN countries ON users.country_code = countries.code;`
- Left Join: Match each record from the first table with a record from the second table, and return every record from the first table.
