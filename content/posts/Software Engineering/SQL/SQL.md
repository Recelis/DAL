---
title: "SQL"
date: 2024-05-14T06:08:57+10:00
draft: true
---

# SQL

[docs](https://www.khanacademy.org/computing/computer-programming/sql)

Structured Query Language is used to create tables, update data and return data back to us.

## CREATE TABLE

Use the SQL command `CREATE TABLE` to create a table with a name.

The arguments are used to specify the columns on the table. This is what your `Database Schema` is defined to be.

For any table, you'll need to specify a `PRIMARY KEY`.

```SQL
    CREATE TABLE groceries (id INTEGER PRIMARY KEY, name TEXT, quantity INTEGER);
```

## INSERT INTO

You can now add data to your table by using the `INSERT` command. This data will be given row by row.

```SQL
    INSERT INTO groceries VALUES (1, "Bananas", 4);
```

The value of the `PRIMARY KEY` must be `unique` for each row in the table.

## SELECT

You can get data out by using the `SELECT` command.

```SQL
SELECT * FROM groceries;
```

The first value is the column that we are interested in. So if we only wanted a list of names from the groceries table:

```SQL
SELECT name FROM groceries;
```

### ORDER BY

You can order your results, if the order matters to your query, by using `ORDER BY` in your `SELECT` query.

```SQL
SELECT * FROM groceries ORDER BY aisle
```

### WHERE

You can do a filter using the `WHERE` keyword.

```SQL
SELECT * FROM groceries WHERE aisle > 5
```
