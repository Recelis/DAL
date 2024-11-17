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

### Aggregate Functions

You can use aggregate functions such as `SUM`, `MAX` etc.

This will come after your SELECT.

```SQL
SELECT aisle, SUM(quantity) FROM groceries
```

#### GROUP BY

Your aggregate function can be grouped by a column. Group By is always at the end of the aggregate query.

```SQL
SELECT aisle, SUM(quantity) FROM groceries GROUP BY aisle;
```

aisle here returns a column for the group by. You can return something else as this column but it will only return the first thing that the group by set.

## AUTOINCREMENT

You can specify a column to autoincrement instead of requiring the insert to create the value each time. This is especially common in the use of id.

```SQL
CREATE TABLE exercise_logs
    (id INTEGER PRIMARY KEY AUTOINCREMENT,
    type TEXT,
    minutes INTEGER,
    calories INTEGER,
    heart_rate INTEGER);


INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 30, 100, 110);
```

Putting the brackets after exercise_logs allows you to specify which columns to insert into for that row.

## AND Operator

This combines conditions for a SELECT query.

```SQL
SELECT * FROM exercise_logs WHERE calories > 50 AND minutes < 30;
```

## OR Operator

You can also use the OR that meet any of some conditions.

```SQL
SELECT * FROM exercise_logs WHERE calories > 50 OR minutes < 30;
```

AND operator takes precedent over OR operator but you can just use parenthesis.

## IN Operator

You can easily filter rows by values in a column.

```SQL
SELECT * FROM exercise_logs WHERE type IN ("biking", "hiking", "tree climbing", "rowing");
```

You can do the invert with a NOT operator.

```SQL
SELECT * FROM exercise_logs WHERE type NOT IN ("biking", "hiking", "tree climbing", "rowing");
```

## Subquery

You can use a subquery in an IN operator.

```SQL
SELECT * FROM exercise_logs WHERE type IN (SELECT type FROM dr_favourites);
```

## LIKE Operator

The LIKE operator allows you to do a fuzzy match against a column. So in the example below, it'll look for the word cardiovascular in the column reason. You need the two percentage signs on the start and end of the word as wildcard checks.

```SQL
SELECT * FROM exercise_logs WHERE type IN (
    SELECT type FROM drs_favorites WHERE reason LIKE "%cardiovascular%");
```

## Aggregate Queries

### SUM

You can sum up all the values in a table using the `SUM` operator.

```SQL
SELECT type, SUM(calories) AS total_calories FROM exercise_logs GROUP BY type;
```

Using the AS keyword, allows you to change the name of the column returned.

Where the table in the example is:
id type minutes calories heart_rate
1 biking 30 115 110
2 biking 10 45 105
3 dancing 15 200 120
4 dancing 15 165 120
5 tree climbing 30 70 90
6 tree climbing 25 72 80
7 rowing 30 70 90
8 hiking 60 80 85

### HAVING

Using the `HAVING` operator allows you to filter based on aggregate Grouped values.

```SQL
SELECT type, SUM(calories) AS total_calories FROM exercise_logs
    GROUP BY type
    HAVING total_calories > 150
    ;
```

type total_calories
biking 160
dancing 365

### COUNT

Count will be used with `GROUP BY` count the number of rows with a value under a column has appeared.

```SQL
SELECT type FROM exercise_logs GROUP BY type HAVING COUNT(*) >= 2;
```

type
biking
dancing
tree climbing

### CASE
A case statement is similar to a switch or if statement in other programming languages.

```sql
SELECT type, heart_rate,
    CASE
        WHEN heart_rate > 220 - 30 THEN "above max"
        WHEN heart_rate > ROUND(0.9 * (220 - 30)) THEN "above target"
        WHEN heart_rate > ROUND(0.5 * (220 - 30)) THEN "within target"
        ELSE "below target"
    END as "hr_zone"
FROM exercise_logs
```

This will do queries and group them into different conditions to then save as another "column".

#### ROUND
The round operator rounds up or down the decimals in a number.

(Current lesson Challenge: Gradebook)