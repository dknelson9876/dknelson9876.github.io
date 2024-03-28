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

- Where: for filtering
    - `SELECT name FROM users WHERE age > 18;`
    - Accepts `=`, `<=`, `>=`, `IS NULL`, `IS NOT NULL`
 
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

## Other Commands(?)

- count: returns an integer count of the number of results that were returned, instead of the actual results
    - `SELECT count(*) FROM users;`
