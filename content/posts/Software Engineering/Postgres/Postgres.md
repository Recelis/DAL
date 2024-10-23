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
