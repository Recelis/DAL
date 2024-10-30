---
title: "Postgres"
date: 2024-10-23T12:48:09.292Z
draft: true
---

# Postgres

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