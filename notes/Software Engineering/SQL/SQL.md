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
    INSERT INTO groceries VALUES (1, 'Bananas', 4);
```

The value of the `PRIMARY KEY` must be `unique` for each row in the table.

**Be very careful of Double Quotation marks vs Single Quotation Marks. Double Quotation marks are used for column or table name while strings use Single Quotation marks.**

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

You can add ASC|DESC to the end of the ORDER BY to sort in ascending or descending order.

```SQL
SELECT * FROM groceries ORDER BY aisle DESC;
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


INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ('biking', 30, 100, 110);
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
SELECT * FROM exercise_logs WHERE type IN ('biking', 'hiking', 'tree climbing', 'rowing');
```

You can do the invert with a NOT operator.

```SQL
SELECT * FROM exercise_logs WHERE type NOT IN ('biking', 'hiking', 'tree climbing', 'rowing');
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
    SELECT type FROM drs_favorites WHERE reason LIKE '%cardiovascular%');
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
        WHEN heart_rate > 220 - 30 THEN 'above max'
        WHEN heart_rate > ROUND(0.9 * (220 - 30)) THEN 'above target'
        WHEN heart_rate > ROUND(0.5 * (220 - 30)) THEN 'within target'
        ELSE 'below target'
    END as 'hr_zone'
FROM exercise_logs
```

This will do queries and group them into different conditions to then save as another 'column'.

#### ROUND

The round operator rounds up or down the decimals in a number.

<!-- Challenge: Gradebook https://www.khanacademy.org/computing/computer-programming/sql/more-advanced-sql-queries/pc/challenge-gradebook -->

1. Create a table with names, number_grade, and the percentage completed.

```sql
SELECT name, number_grade, ROUND(fraction_completed * 100) AS percent_completed from student_grades;
```

2. Create a table with letter_grades and the number of students who had reached that letter grade.

```sql
SELECT COUNT(*),
    CASE
        WHEN number_grade > 90 THEN 'A'
        WHEN number_grade <= 90 AND number_grade > 80 THEN 'B'
        WHEN number_grade <= 80 AND number_grade > 70 THEN 'C'
        ELSE 'F'
    END as 'letter_grade'
FROM student_grades
GROUP BY letter_grade;
```

<!-- Challenge: Project Data Dig https://www.khanacademy.org/computing/computer-programming/sql/more-advanced-sql-queries/pp/project-data-dig -->

What are average, max, and min values in the data?

```SQL
SELECT
    MAX(population) as Max,
    ROUND(AVG(population)) as Avg,
    MIN(population) as Min
from countries;
```

What about those numbers per category in the data (using HAVING)?

```SQL
SELECT name, population from countries
    GROUP BY name
    HAVING percent_one_year_change >= 2.1
    ORDER BY population;
```

What ways are there to group the data values that don’t exist yet (using CASE)?

```SQL
SELECT COUNT(*),
    CASE
        WHEN percent_one_year_change > 1 THEN 'Growing'
        WHEN percent_one_year_change = 1 THEN 'Maintaining'
        ELSE 'Shrinking'
    END as 'percentage_change'
FROM countries
GROUP BY percentage_change;
```

What interesting ways are there to filter the data (using AND/OR)?

```SQL
SELECT
    CASE
        WHEN percent_one_year_change > 1 AND fertility_rate > 2.1 THEN 'Growth with ABOVE THRESHOLD'
        WHEN percent_one_year_change > 1  AND fertility_rate = 2.1 THEN 'Growth with ON THRESHOLD'
        WHEN percent_one_year_change > 1  AND fertility_rate < 2.1 THEN 'Growth with BELOW THRESHOLD'
        WHEN percent_one_year_change = 1  AND fertility_rate > 2.1 THEN 'Maintaining with ABOVE THRESHOLD'
        WHEN percent_one_year_change = 1  AND fertility_rate = 2.1 THEN 'Maintaining with ON THRESHOLD'
        WHEN percent_one_year_change = 1  AND fertility_rate < 2.1 THEN 'Maintaining with BELOW THRESHOLD'
        WHEN percent_one_year_change < 1  AND fertility_rate > 2.1 THEN 'Shrinkage with ABOVE THRESHOLD'
        WHEN percent_one_year_change < 1  AND fertility_rate = 2.1 THEN 'Shrinkage with ON THRESHOLD'
        WHEN percent_one_year_change < 1  AND fertility_rate < 2.1 THEN 'Shrinkage with BELOW THRESHOLD'
        ELSE 'NA'
    END as 'threshold',
    COUNT(*) as 'Number of Countries'
FROM countries
GROUP BY threshold;
```

## JOIN

### CROSS JOIN

This is the simplest join. For each row in first table, create a new row for each row in the second table.

```SQL
SELECT * FROM student_grades, students;
```

e.g.

```SQL
-- Students Table --
INSERT INTO students (first_name, last_name, email, phone, birthdate)
    VALUES ('Peter', 'Rabbit', 'peter@rabbit.com', '555-6666', '2002-06-24');
INSERT INTO students (first_name, last_name, email, phone, birthdate)
    VALUES ('Alice', 'Wonderland', 'alice@wonderland.com', '555-4444', '2002-07-04');
-- Grades Table --
INSERT INTO student_grades (student_id, test, grade)
    VALUES (1, 'Nutrition', 95);
INSERT INTO student_grades (student_id, test, grade)
    VALUES (2, 'Nutrition', 92);
INSERT INTO student_grades (student_id, test, grade)
    VALUES (1, 'Chemistry', 85);
INSERT INTO student_grades (student_id, test, grade)
    VALUES (2, 'Chemistry', 95);
```

-- Result
| id | student_id | test | grade | id | first_name | last_name | email | phone | birthdate |
|----|------------|-----------|-------|----|------------|------------|----------------------|----------|------------|
| 1 | 1 | Nutrition | 95 | 1 | Peter | Rabbit | peter@rabbit.com | 555-6666 | 2002-06-24 |
| 1 | 1 | Nutrition | 95 | 2 | Alice | Wonderland | alice@wonderland.com | 555-4444 | 2002-07-04 |
| 2 | 2 | Nutrition | 92 | 1 | Peter | Rabbit | peter@rabbit.com | 555-6666 | 2002-06-24 |
| 2 | 2 | Nutrition | 92 | 2 | Alice | Wonderland | alice@wonderland.com | 555-4444 | 2002-07-04 |
| 3 | 1 | Chemistry | 85 | 1 | Peter | Rabbit | peter@rabbit.com | 555-6666 | 2002-06-24 |
| 3 | 1 | Chemistry | 85 | 2 | Alice | Wonderland | alice@wonderland.com | 555-4444 | 2002-07-04 |
| 4 | 2 | Chemistry | 95 | 1 | Peter | Rabbit | peter@rabbit.com | 555-6666 | 2002-06-24 |
| 4 | 2 | Chemistry | 95 | 2 | Alice | Wonderland | alice@wonderland.com | 555-4444 | 2002-07-04 |

### INNER JOIN

This is a join that does the cross join but limits it to the condition.

#### Implicit Inner Join

This type of inner join is not very efficient.

```SQL
SELECT * FROM student_grades, students
    WHERE student_grades.student_id = students.id;
```

#### Explicit Inner Join

This is a more efficient inner join which uses the JOIN keyword.

```SQL
SELECT first_name, last_name, email, test, grade FROM students
    JOIN student_grades
    ON students.id = student_grades.student_id;
```

The SELECT ... FROM ... JOIN alone does the **CROSS JOIN**. The ON will do the inner-join.

You can filter this down further using the WHERE keyword.

If you have columns that have duplicated names across more than one table, you'll get an error because SQL will not know which column to pull data from.
So you should **Prefix** your column names with your table names.

```SQL
SELECT students.first_name, students.last_name, students.email, student_grades.test, student_grades.grade FROM students
    JOIN student_grades
    ON students.id = student_grades.student_id
    WHERE grade > 90;
```

<!-- Challenge: Bobby's Hobbies
https://www.khanacademy.org/computing/computer-programming/sql/relational-queries-in-sql/pc/challenge-bobbys-hobbies
-->

Now, select the 2 tables with a join so that you can see each person's name next to their hobby.

```SQL
SELECT persons.name, hobbies.name FROM persons
    JOIN hobbies
    ON persons.id = hobbies.person_id;
```

Now, add an additional query that shows only the name and hobbies of 'Bobby McBobbyFace', using JOIN combined with WHERE.

```SQL
SELECT persons.name, hobbies.name FROM persons
    JOIN hobbies
    ON persons.id = hobbies.person_id
    WHERE persons.name = 'Bobby McBobbyFace';
```

### OUTER JOIN

An OUTER JOIN is something that displays the join even if there are no results.
If there are no results, then it'll return with a NULL instead.

#### LEFT OUTER JOIN

The LEFT part of the LEFT OUTER JOIN means to show all the values in the left hand table.

```sql
SELECT students.first_name, students.last_name, student_projects.title
    FROM students
    LEFT OUTER JOIN student_projects
    ON students.id = student_projects.student_id;
```

| first_name | last_name  | title        |
| ---------- | ---------- | ------------ |
| Peter      | Rabbit     | Carrotapault |
| Alice      | Wonderland | NULL         |

#### RIGHT OUTER JOIN

The RIGHT part of the RIGHT OUTER JOIN just forces you to display all the values on the right hand table.

You can just LEFT OUTER JOIN but switch tables around.

#### FULL OUTER JOIN

This will show all values in both tables. Displaying NULL on each side if needed.

<!-- Challenge: Customer's Orders
https://www.khanacademy.org/computing/computer-programming/sql/relational-queries-in-sql/pc/challenge-customers-orders -->

We've created a database for customers and their orders. Not all of the customers have made orders, however. Come up with a query that lists the name and email of every customer followed by the item and price of orders they've made. Use a LEFT OUTER JOIN so that a customer is listed even if they've made no orders, and don't add any ORDER BY.

```SQL
SELECT customers.name, customers.email, orders.item, orders.price from customers
    LEFT OUTER JOIN orders
    ON customers.id = orders.customer_id;
```

Now, create another query that will result in one row per each customer, with their name, email, and total amount of money they've spent on orders. Sort the rows according to the total money spent, from the most spent to the least spent.
(Tip: You should always GROUP BY on the column that is most likely to be unique in a row.)

```SQL
 SELECT customers.name, customers.email, SUM(orders.price) as total from customers
    LEFT OUTER JOIN orders
    ON customers.id = orders.customer_id
    GROUP BY customers.name
    ORDER BY total DESC;
```

### Self Joins

You can do a **join** to your own table.

E.g. if your table has an id that references itself.

```SQL
CREATE TABLE students (id INTEGER PRIMARY KEY AUTOINCREMENT,
    first_name TEXT,
    last_name TEXT,
    email TEXT,
    phone TEXT,
    birthdate TEXT,
    buddy_id INTEGER);
```

This will look like:

```SQL
SELECT students.first_name, students.last_name, buddies.email as buddy_email
    FROM students
    JOIN students buddies
    ON students.buddy_id = buddies.id;
```

In this case the `buddies` in `JOIN students buddies` is an alias.

<!-- Challenge We've created a table with all the 'Harry Potter' movies, with a sequel_id column that matches the id of the sequel for each movie. Issue a SELECT that will show the title of each movie next to its sequel's title (or NULL if it doesn't have a sequel). -->

We've created a table with all the 'Harry Potter' movies, with a sequel_id column that matches the id of the sequel for each movie. Issue a SELECT that will show the title of each movie next to its sequel's title (or NULL if it doesn't have a sequel).

```sql
SELECT movies.title, sequel.title as sequel_title
    from movies
    LEFT OUTER JOIN movies sequel
    ON movies.sequel_id = sequel.id;
```

### Combining Multiple Joins

You can do more than one join at a time by combining joins.

```sql
SELECT a.title, b.title FROM project_pairs
    JOIN student_projects a
    ON project_pairs.project1_id = a.id
    JOIN student_projects b
    ON project_pairs.project2_id = b.id;
```

In this case, two self joins are done on the project pairs to be
able to display the title of each project in the project_pairs list.

<!-- Challenge: https://www.khanacademy.org/computing/computer-programming/sql/relational-queries-in-sql/pc/challenge-friendbook -->

We've created a database for a friend networking site, with a table storing data on each person, a table on each person's hobbies, and a table of friend connections between the people. In this first step, use a JOIN to display a table showing people's names with their hobbies.

```SQL
SELECT persons.fullname, hobbies.name FROM persons
    JOIN hobbies
    ON persons.id = hobbies.person_id;
```

Now, use another SELECT with a JOIN to show the names of each pair of friends, based on the data in the friends table.

```sql
SELECT person1.fullname, person2.fullname FROM friends
    JOIN persons person1
    ON friends.person1_id = person1.id
    JOIN persons person2
    ON friends.person2_id = person2.id;
```

<!-- Project: Famous people https://www.khanacademy.org/computing/computer-programming/sql/relational-queries-in-sql/pp/project-famous-people -->

```SQL
CREATE TABLE authors (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT);

CREATE TABLE books (id INTEGER PRIMARY KEY AUTOINCREMENT, title TEXT);

CREATE TABLE book_authors(book_id INTEGER, author_id INTEGER);

INSERT INTO authors(name) VALUES ('Jane Austen');

INSERT INTO authors(name) VALUES ('Mary Shelley');

INSERT INTO authors(name) VALUES ('Stephen King');

INSERT INTO authors(name) VALUES ('Neil Gaiman');

INSERT INTO authors(name) VALUES ('Terry Pratchett');

INSERT INTO books(title) VALUES ('Pride and Prejudice');

INSERT INTO books(title) VALUES ('Frankenstein');

INSERT INTO books(title) VALUES ('Carrie');

INSERT INTO books(title) VALUES ('Good Omens');

INSERT INTO book_authors(book_id, author_id) VALUES (1, 1);

INSERT INTO book_authors(book_id, author_id) VALUES (2, 2);

INSERT INTO book_authors(book_id, author_id) VALUES (3, 3);

INSERT INTO book_authors(book_id, author_id) VALUES (4, 4);

INSERT INTO book_authors(book_id, author_id) VALUES (4, 5);

SELECT books.title, authors.name from book_authors
    JOIN books
    ON books.id = book_authors.book_id
    JOIN authors
    ON authors.id = book_authors.author_id;
```

## SQL Query Planning and Optimization

A SQL Query needs to be planned out because there may be many ways that it is executed to get the results.

For a simple query like this:

```sql
SELECT * FROM books WHERE author = 'J K Rowling';
```

It could do it two ways:

- A Full Table Scan to look through all the rows in the table.
- Creating an **index** by making a copy of the table sorted by author, doing `binary search` to find row where author is 'J K Rowling', finding matching the ids, then doing binary search on original table to return rows that match the IDS.

The SQL engine would plan out which to use based on what is most efficient. The factors here may be number of rows in table. How long it might call this query.

### Query Tuning

This is a manual optimisation process for improving the response of SQL queries. Oftentimes would use a SQL profiler first to see what queries are taking up the most time.

Then you can tell SQL how it is executing the SQL query bu using `EXPLAIN QUERY PLAN`
in front of the query you want to analyse (this is only for SQLite).

The next step is to then try to figure out how to improve the query based on your SQL engine. This might be creating an index.

More docs for reading
[extra](https://www.sqlite.org/queryplanner.html)

[extra](https://docs.oracle.com/cd/B19306_01/server.102/b14211/sql_1016.htm#g42927)

[extra](https://www.red-gate.com/simple-talk/databases/sql-server/performance-sql-server/execution-plan-basics/)

## UPDATE

The syntax for updating the a row is:

```sql
UPDATE diary_logs SET content = 'I had a horrible fight with OhNoesGuy' WHERE id = 1;
```

## DELETE

Deleting a row is like this:

```sql
DELETE FROM diary_logs WHERE id = 1;
```

Be very careful with the WHERE condition to ensure that it is unique across the table. This is the same with updating too.

<!-- Challenge: Dynamic Documents: https://www.khanacademy.org/computing/computer-programming/sql/modifying-databases-with-sql/pc/challenge-dynamic-documents  -->

We've created a database for a documents app, with rows for each document with it's title, content, and author. In this first step, use UPDATE to change the author to 'Jackie Draper' for all rows where it's currently 'Jackie Paper'. Then re-select all the rows to make sure the table changed like you expected.

```sql
UPDATE documents SET author  = 'Jackie Draper' WHERE author = 'Jackie Paper';

SELECT * FROM documents;
```

Now you'll delete a row, being very careful not to delete all the rows. Only delete the row where the title is 'Things I'm Afraid Of'. Then re-select all the rows to make sure the table changed like you expected.

```sql
DELETE FROM documents WHERE title = 'Things I'm Afraid Of';

SELECT * FROM documents;
```

## ALTER TABLE

After creation, to add a new column, you cannot run the `CREATE TABLE` command again with the new column name because that will wipe out any existing data you have. Instead, you should use the ALTER command. There can be some performance issues for this on large datasets.

```SQL
ALTER TABLE diary_logs ADD emotion TEXT default 'unknown';
```

## DROP TABLE

If you want to completely remove a table, use `DROP`.

```SQL
DROP TABLE diary_logs;
```

<!-- Challenge: Clothing alterations -->

Step 1
We've created a database of clothes, and decided we need a price column. Use ALTER to add a 'price' column to the table. Then select all the columns in each row to see what your table looks like now.

```SQL
ALTER TABLE clothes ADD price INTEGER default 0;
SELECT * from clothes;
```

Step 2
Now assign each item a price, using UPDATE - item 1 should be 10 dollars, item 2 should be 20 dollars, item 3 should be 30 dollars. When you're done, do another SELECT of all the rows to check that it worked as expected.

```SQL
UPDATE clothes SET price = 10 WHERE id = 1;
UPDATE clothes SET price = 20 WHERE id = 2;
UPDATE clothes SET price = 30 WHERE id = 3;

SELECT * from clothes;
```

Step 3
Now insert a new item into the table that has all three attributes filled in, including 'price'. Do one final SELECT of all the rows to check it worked.

```SQL
INSERT INTO clothes (type, design, price) VALUES ('cardigan', 'warm wool', 40);
SELECT * FROM clothes;
```

## Making SQL Safer

Here are some techniques for not accidentally damaging or erasing all your data. Especially when you are running SQL on a live database.

### Avoiding bad updates/deletes

Before running an update:

```SQL
-- run this before the update
SELECT id, deleted FROM users WHERE id = 1;

UPDATE users SET deleted = true WHERE id = 1;

-- You can also use the LIMIT operator to make sure you don't update more rows than you expect.
UPDATE users SET deleted = true WHERE id = 1 LIMIT 1;
```

#### LIMIT

This limits the number of rows to update/delete.

```SQL
DELETE users WHERE id = 1 LIMIT 1;
```

### Transactions

A transaction is a sequence of operations that are treated as one logical piece of work. It must comply with **ACID principles**.

A command like **CREATE**, **UPDATE**, **INSERT**, or **DELETE** automatically start a transaction, but you may want multiple updates to take place one after another.

The syntax for a transaction starts with `BEGIN TRANSACTION` and ends with `COMMIT`.

```sql
BEGIN TRANSACTION;
UPDATE people SET husband = 'Winston' WHERE user_id = 1;
UPDATE people SET wife = 'Winnefer' WHERE user_id = 2;
COMMIT;
```

If the database is unable to do both UPDATE commands, then it'll rollback to how it was before it started.

A transaction also stops multiple concurrency issues. i.e.

A user has earned a user_bade and the recent_activity has updated with this:

```sql
INSERT INTO user_badges VALUES (1, 'SQL Master', '4pm');
UPDATE user SET recent_activity = 'Earned SQL Master badge' WHERE id = 1;
```

However, a second process has added another badge almost at the same time, and this leads to the commands being written in the following order.

```sql
INSERT INTO user_badges VALUES (1, 'SQL Master');
INSERT INTO user_badges VALUES (1, 'Great Listener');
UPDATE user SET recent_activity = 'Earned Great Listener badge' WHERE id = 1;
UPDATE user SET recent_activity = 'Earned SQL Master badge' WHERE id = 1;
```

This will mean the order of recent_activity is not correct.

However, it would not be an issue if the INSERT and UPDATE commands were wrapped in a transaction.

```sql
BEGIN TRANSACTION;
INSERT INTO user_badges VALUES (1, 'SQL Master');
UPDATE user SET recent_activity = 'Earned SQL Master badge' WHERE id = 1;
COMMIT;
```

### Making Backups

You should be making backups at a regular interval. Backups can happen on an hourly, daily, or weekly basis depending on size and space available.

### Replication

Storing multiple copies of databases in different places is important if one copy becomes unavilable. However, it is slower to write. If the data is very time-sensitive, it is better than to wait for an engineer to get the data out of a backup.

### Granting privileges

Most db systems that are stored on a shared server with multiple users will have users and privileges built in. Full access should only be given for a select few.

```SQL
GRANT FULL ON TABLE users TO super_admin;
```

You can limit by granting SELECT to a different type of user. But even this is too much scope as there may be private data. However, big companies may also have anonymized versions of their data.

```SQL
GRANT SELECT ON TABLE users TO analyzing_user;
```

<!-- Project: App impersonator -->

Think about your favorite apps, and pick one that stores your data- like a game that stores scores, an app that lets you post updates, etc. Now in this project, you're going to imagine that the app stores your data in a SQL database (which is pretty likely!), and write SQL statements that might look like their own SQL.

CREATE a table to store the data.
INSERT a few example rows in the table.
Use an UPDATE to emulate what happens when you edit data in the app.
Use a DELETE to emulate what happens when you delete data in the app.

```SQL
/* What does the app's SQL look like? */

CREATE TABLE todos (id INTEGER PRIMARY KEY AUTOINCREMENT, title TEXT, priority INTEGER NOT NULL UNIQUE);


INSERT into todos VALUES (0, 'clean rubbish bin', 0);
INSERT into todos VALUES (1, 'throw out rubbish', 1);
INSERT into todos VALUES (2, 'water plants', 2);


SELECT * from todos;

BEGIN TRANSACTION;
UPDATE todos SET priority = -1 WHERE id = 0;
UPDATE todos SET priority = 0 WHERE id = 1;
UPDATE todos SET priority = 1 WHERE id = 0;
COMMIT;

SELECT * from todos;

DELETE FROM todos WHERE id = 2;

SELECT * from todos;

```
