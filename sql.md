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

## Mashing Tables Together

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

In the `WHERE` part of a query, you can search for rows that match any of multiple attributes by using the OR keyword. For example, if you wanted to find the friends of Pickles that are over 25cm in height or are cats, you would run:
SELECT \* FROM friends_of_pickles WHERE height_cm > 25 OR species = 'cat';

> By putting DISTINCT after SELECT, you do not return duplicates.

For example, if you run
`SELECT DISTINCT` gender, species `FROM` friends_of_pickles WHERE height_cm < 100;, you will get the gender/species combinations of the animals less than 100cm in height.

Using the WHERE clause, we can find rows where a value is in a list of several possible values.

SELECT \* FROM friends_of_pickles WHERE species IN ('cat', 'human'); would return the friends_of_pickles that are either a cat or a human.

> To find rows that are not in a list, you use NOT IN instead of IN.

## ORDER BY

If you want to sort the rows by some kind of attribute, you can use the ORDER BY keyword. For example, if you want to sort the friends_of_pickles by name, you would run: SELECT \* FROM friends_of_pickles ORDER BY name;. That returns the names in ascending alphabetical order.

> In order to put the names in descending order, you would add a DESC at the end of the query.
> SELECT \* FROM friends_of_pickles ORDER BY height_cm DESC;

## LIMIT # of returned rows

Often, tables contain millions of rows, and it can take a while to grab everything. If we just want to see a few examples of the data in a table, we can select the first few rows with the LIMIT keyword. If you use ORDER BY, you would get the first rows for that order.

If you wanted to see the two shortest friends_of_pickles, you would run: SELECT \* FROM friends_of_pickles ORDER BY height_cm LIMIT 2;

> Note:

- Some variants of SQL do not use the LIMIT keyword.
- The LIMIT keyword comes after the DESC keyword.

SELECT \* FROM friends_of_pickles ORDER BY height_cm DESC LIMIT 1;

## COUNT(\*)

Another way to explore a table is to check the number of rows in it. For example, if we are querying a table states_of_us, we'd expect 50 rows, or 500 rows in a table called fortune_500_companies.

SELECT COUNT(\*) FROM friends_of_pickles; returns the total number of rows in the table friends_of_pickles

## COUNT(\*) ... WHERE

We can combine COUNT(\*) with WHERE to return the number of rows that matches the WHERE clause.

For example, SELECT COUNT(\*) FROM friends_of_pickles WHERE species = 'human'; returns 2.

## SUM

We can use the SUM keyword in order to find the sum of a given value.

For example, running SELECT SUM(num_legs) FROM family_members; returns the total number of legs in the family.

## AVG

We can use the AVG keyword in order to find the average of a given value.

For example, running SELECT AVG(num_legs) FROM family_members; returns the average number of legs of each family member.

## MAX and MIN

We can use the MAX and MIN to find the maximum or minimum value of a table.

To find the least number of legs in a family member (2), you can run
SELECT MIN(num_legs) FROM family_members;

## GROUP BY

You can use aggregate functions such as COUNT, SUM, AVG, MAX, and MIN with the GROUP BY clause.

When you GROUP BY something, you split the table into different piles based on the value of each row.

For example,
SELECT COUNT(\*), species FROM friends_of_pickles GROUP BY species; would return the number of rows for each species.

## Nested queries

In SQL, you can put a SQL query inside another SQL query.

For example, to find the family members with the least number of legs,
you can run:
SELECT \* FROM family_members WHERE num_legs = (SELECT MIN(num_legs) FROM family_members);

The SELECT query inside the parentheses is executed first, and returns the minimum number of legs. Then, that value (2) is used in the outside query, to find all family members that have 2 legs.

## NULL

Sometimes, in a given row, there is no value at all for a given column. For example, a dog does not have a favorite book, so in that case there is no point in putting a value in the favorite_book column, and the value is NULL. In order to find the rows where the value for a column is or is not NULL, you would use IS NULL or IS NOT NULL.

> Can you return all of the rows of family_members where favorite_book is not null?
> SELECT \* FROM family_members WHERE favorite_book IS NOT NULL;

## Date

Sometimes, a column can contain a date value. The first 4 digits represents the year, the next 2 digits represents the month, and the next 2 digits represents the day of the month. For example, 1985-07-20 would mean July 20, 1985.

You can compare dates by using < and >. For example, SELECT \* FROM celebs_born WHERE birthdate < '1985-08-17'; returns a list of celebrities that were born before August 17th, 1985.

## Inner joins

Different parts of information can be stored in different tables, and in order to put them together, we use INNER JOIN ... ON. Joining tables gets to the core of SQL functionality, but it can get very complicated. We will start with a simple example, and will start with an INNER JOIN.

As you can see below, there are 3 tables:
character: Each character is a row and is represented by a unique identifier (id), e.g. 1 is Doogie Howser
character_tv_show: For each character, which show is he/she in?
character_actor: For each character, who is the actor?

See that in character_tv_show, instead of storing both the character and TV show names (e.g. Willow Rosenberg and Buffy the Vampire Slayer), it stores the character_id as a substitute for the character name. This character_id refers to the matching id row from the character table.

This is done so data is not duplicated. For example, if the name of a character were to change, you would only have to change the name of the character in one row.

This allows us to "join" the tables together "on" that reference/common column.

To get each character name with his/her TV show name, we can write
SELECT \* character.name, character_tv_show.tv_show_name
FROM character
INNER JOIN character_tv_show
ON character.id = character_tv_show.character_id;
This puts together every row in character with the corresponding row in character_tv_show, or vice versa.

Note:

- We use the syntax table_name.column_name. If we only used column_name, SQL might incorrectly assume which table it is coming from.
- The example query above is written over multiple lines for readability, but that does not affect the query.

## Multiple joins

In the previous exercise, we explained that TV show character names were not duplicated, so if the name of a character were to change, you would only have to change the name of the character in one row.

However, the previous example was a bit artificial because the TV show names and actor names were duplicated.

In order to not duplicate any names, we need to have more tables, and use multiple joins.

We have tables for characters, TV shows, and actors. Those tables represent things (also known as entities).

In addition those tables, we have the relationship tables character_tv_show and character_actor, which capture the relationship between two entities.

This is a flexible way of capturing the relationship between different entities, as some TV show characters might be in multiple shows, and some actors are known for playing multiple characters.

To get each character name with his/her TV show name, we can write
SELECT character.name, tv_show.name
FROM character
INNER JOIN character_tv_show
ON character.id = character_tv_show.character_id
INNER JOIN tv_show
ON character_tv_show.tv_show_id = tv_show.id;

## Joins with WHERE

You can also use joins with the WHERE clause.

To get a list of characters and TV shows that are not in "Buffy the Vampire Slayer" and are not Barney Stinson, you would run:
SELECT character.name, tv_show.name
FROM character
INNER JOIN character_tv_show
ON character.id = character_tv_show.character_id
INNER JOIN tv_show
ON character_tv_show.tv_show_id = tv_show.id WHERE character.name != 'Barney Stinson' AND tv_show.name != 'Buffy the Vampire Slayer';
