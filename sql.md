`SQL`, which stands for Structured Query Language, is the programming language used to communicate with a relational database.

`SQL` is a powerful language that uses simple English sentences that, with a few lines, allow you to Select (find), Insert (add), Update (change), and Delete (remove) a large amount of data.

`SQL` can be pronounced as "Sequel" or as "Ess-Queue-Ell".

## Useful Resources

https://hunter-ducharme.gitbook.io/sql-basics/

https://launchschool.com/books/sql/read/introduction

https://blog.codinghorror.com/a-visual-explanation-of-sql-joins/

https://www.sqlteaching.com/

## Statements:

SELECT
CREATE TABLE
DROP TABLE
CREATE INDEX
DROP INDEX
UPDATE
DELETE
INSERT INTO
CREATE DATABASE
DROP DATABASE
COMMIT (concept)
ROLLBACK (concept)

## Clauses:

DISTINCT
WHERE
IN
AND
OR
BETWEEN
LIKE
ORDER BY
COUNT

## Functions

GROUP BY
HAVING
AVG
COUNT
MIN
MAX
SUM

SQL is the language used to talk to many relational databases. These databases use lots of tables to store different types of data (e.g. “users” and “posts” tables).

Tables are long lists like spreadsheets where each row is a different record (or object, e.g. a single user) and each column is one of that record’s attributes (like name, email, etc). The one column that all tables include is an “ID” column, which gives the unique row numbers, and is called the record’s “primary key”.

You can “link” tables together by making one of the columns in one table point to the ID of another table, for instance a row in the “posts” table might include the author’s ID under the column called “user_id”. Because the “posts” table has the ID of another table in it, that column is called a “foreign key”.

## Setting Stuff Up

SQL lets you do everything. The first category of commands are for setting up the database (CREATE DATABASE), setting up an individual table (CREATE TABLE), and similar commands for altering or destroying them.

## The Schema

The setup information for your database is stored in a special file called the “Schema”, and this is updated whenever you make changes to the structure of your database. Think of the schema as saying “here’s our database and it’s got a couple tables. The first table is ‘users’ and it’s got columns for ‘ID’ (which is an integer), ‘name’ (which is a bunch of characters), ‘email’ (which is a bunch of characters) …”

In addition to setting up tables, you can tell your database to only allow unique values in a particular column (e.g. for usernames) or to index a column for faster searching later with CREATE INDEX. Create indexes, which basically do all the hard work of sorting your table ahead of time, for columns that you’ll likely be using to search on later (like username)… it will make your database much faster.

> SQL likes semicolons at the end of lines and using single quotes (‘) instead of double quotes(“)

## Mucking Around with Data

Once your database is set up and you’ve got empty tables to work with, you use SQL’s statements to start populating it. The main actions you want to do are CRUD (which we’ve seen before) – Create, Read, Update, and Destroy.

## Every CRUDdy command in SQL contains a few parts:

- the action (“statement”),
- the table it should run on,
- and the conditions (“clauses”).

> NOTE: If you just do an action on a table without specifying conditions, it will apply to the whole database and you’ll probably break something.

For “Destroy” queries, the classic mistake is typing `DELETE _ FROM users` without a WHERE clause, which removes all your users from the table. You probably needed to delete just one user, who you would specify based on some (hopefully unique) attribute like “name” or “id” as part of your condition clause, e.g. DELETE \_ FROM users WHERE users.id = 1.

“Read” queries, which use SELECT, are the most common, e.g. SELECT _ FROM users WHERE created_at < '2013-12-11 15:35:59 -0800'. The _ you see just says “all the columns”. Specify a column using both the table name and the column name. You can get away with just the column name for simple queries but as soon as there are more than one table involved, SQL will yell at you so just always specify the table name: SELECT users.id, users.name FROM users.

A close cousin of SELECT, for if you only want unique values of a column, is SELECT DISTINCT. Say you want a list of all the different names of your users without any duplicates… try SELECT DISTINCT users.name FROM users.

Mashing Tables Together
If you want to get all the posts created by a given user, you need to tell SQL which columns it should use to zip the tables together with the ON clause. Perform the “zipping” with the JOIN command. But wait, if you mash two tables together where the data doesn’t perfectly match up (e.g. there are multiple posts for one user), which rows do you actually keep? There are four different possibilities:

Note: the “left” table is the original table (the one that the FROM clause was ON), e.g. “users” in examples below.\*

See “A visual explanation of sql joins” by Jeff Atwood for good visuals.\*

INNER JOIN, aka JOIN – Your best friend and 95% of what you’ll use. Keeps only the rows from both tables where they match up. If you asked for all the posts for all users (SELECT _ FROM users JOIN posts ON users.id = posts.user_id), it would return only the users who have actually written posts and only posts which have specified their author in the user_id column. If an author has written multiple posts, there will be multiple rows returned (but the columns containing the user data will just be repeated).
LEFT OUTER JOIN – keep all the rows from the left table and add on any rows from the right table which match up to the left table’s. Set any empty cells this produces to NULL. E.g. return all the users whether they have written posts or not. If they do have posts, list those posts as above. If not, set the columns we asked for from the “posts” table to NULL.
RIGHT OUTER JOIN – the opposite… keep all rows in the right table.
FULL OUTER JOIN – Keep all rows from all tables, even if there are mismatches between them. Set any mismatched cells to NULL.
Joins naturally let you specify conditions too, like if you only want the posts from a specific user: SELECT _ FROM users JOIN posts ON users.id = posts.user_id WHERE users.id = 42.

Read through W3 Schools’ Joins lesson for a better explanation.

Using Functions to Aggregate Your Data
When you run a vanilla SQL query, you often get back a bunch of rows. Sometimes you want to just return a single relevant value that aggregates a column, like the COUNT of posts a user has written. In this case, just use one of the helpful “aggregate” functions offered by SQL (most of which you’d expect to be there – functions like SUM and MIN and MAX etc). You include the function as a part of the SELECT statement, like SELECT MAX(users.age) FROM users. The function will operate on just a single column unless you specify \*, which only works for some functions like COUNT (because how would you MAX a column for “name”?).

You often see aliases (AS) used to rename columns or aggregate functions so you can call them by that alias later, e.g. SELECT MAX(users.age) AS highest_age FROM users will return a column called highest_age with the maximum age in it.

Now we’re getting into the fun stuff. Aggregate functions like COUNT which return just a single value for your whole dataset are nice, but they become really useful when you want to use them on very specific chunks of your data and then group them together, e.g. displaying the COUNT of posts for EACH user (as opposed to the count of all posts by all users).
