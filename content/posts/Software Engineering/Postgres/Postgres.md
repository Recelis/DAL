---
title: "Postgres"
date: 2024-10-23T12:48:09.292Z
draft: true
---

# Postgres

## Creating a db
```sql
    createdb dbName
```

## Deleting a db
```sql
    dropdb dbName
```

## Accessing a Database
To access the database from the CLI, you can use `psql`.

```sql
psql dbName
```

You can also use SQL Editing tools like PgAdmin. In MacOS, you can use the `postgres`
software to create, access, and delete dbs.

## Common Psql commands
[docs](https://stackoverflow.com/a/47185648)
Now in Psql you could run commands such as:

    \? list all the commands
    \l list databases
    \conninfo display information about current connection
    \c [DBNAME] connect to new database, e.g., \c template1
    \dt list tables of the public schema
    \dt <schema-name>.* list tables of certain schema, e.g., \dt public.*
    \dt *.* list tables of all schemas
    Then you can run SQL statements, e.g., SELECT * FROM my_table;(Note: a statement must be terminated with semicolon ;)
    \q quit psql

## Views

[docs](https://www.postgresql.org/docs/current/tutorial-views.html)

A view is a way to generate a query that is reuseable that you don't want to define over and over again. In the example below, it gets the weather with city name, but it does this by matching the city to the city which has the name of the weather.city.

```SQL
CREATE VIEW myview AS
SELECT name, temp_lo, temp_hi, prcp, date, location
FROM weather, cities
WHERE city = name;

SELECT * FROM myview;
```

### Transactions

[docs](https://www.postgresql.org/docs/current/tutorial-transactions.html)

You can group queries together so that they will behave in an atomic way. The way to do this is by wrap your queries with a BEGIN and a COMMIT statement.

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100.00
    WHERE name = 'Alice';
-- etc etc
COMMIT;
```

All sql statements are by default treated as transactional.

You can use `SAVEPOINT` and `ROLLBACK TO` to remove actions and then skip to the place after you rollbacked from.

```SQL
BEGIN;
UPDATE accounts SET balance = balance - 100.00
    WHERE name = 'Alice';
SAVEPOINT my_savepoint;
UPDATE accounts SET balance = balance + 100.00
    WHERE name = 'Bob';
-- oops ... forget that and use Wally's account
ROLLBACK TO my_savepoint;
UPDATE accounts SET balance = balance + 100.00
    WHERE name = 'Wally';
COMMIT;
```

In this example, you set the savepoint, and then rollback to the savepoint, which removes the update to Bob's account. Then continue forward to Wally's account.

Row Security Policies

[docs](https://www.postgresql.org/docs/current/ddl-rowsecurity.html)
This is a way to control access to tables. You can define policies that limit what rows can be accessed based on the user. If a user has access to the the table, then they can access all rows within that table.

Once row security is enabled on a table, then all subsequent queries on that table will be affected by the policy. If there are no policies defined, then default-deny policy is used so no one will be able to access the table.

I am currently using row security policies as part of Supabase's auth system within WishingMay.
