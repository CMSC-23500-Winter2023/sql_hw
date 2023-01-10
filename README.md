# CMSC 23500: SQL Assignment
## Introduction

For this assignment you will be writing SQL and will be using a PostgreSQL server, which provides a standards-compliant SQL implementation.  
We are using version 10.6 of Postgres, so the documentation at https://www.postgresql.org/docs/10.6/static/ may be useful as well.

You will need access to a postgresql client that supports version 10 or later. On debian/ubuntu machines this is as simple as installing the PSQL client 
via:
`sudo apt install postgresql-client-common postgresql-client-10`

For this homework we will be using the database server addison.cs.uchicago.edu which is publicly accessible from anywhere.


## Dataset
We will use a slightly expanded database of the boardgames dataset.
This now includes attributes minplaytime and maxplaytime for the games relation.
In addition there is a categories relation and gamecat relation that maps games to categories.

Once connected, there are two kinds of commands useful to a database user. The first kind are the psql client meta-commands
and the second are SQL statements (which end in a ;).
The most important command is \?, which gives you help on meta-commands. There are two others you will find very useful.  

First, you can list the tables in the database with \dt

```
boardgames=> \dt
           List of relations
 Schema |    Name    | Type  |  Owner   
--------+------------+-------+----------
 public | categories | table | postgres
 public | desgame    | table | postgres
 public | designers  | table | postgres
 public | gamecat    | table | postgres
 public | games      | table | postgres
(5 rows)


```

Second, you can view the schema  of a given table with \d *table_name*

```
boardgames=> \d games
                         Table "public.games"
   Column    |          Type          | Collation | Nullable | Default
-------------+------------------------+-----------+----------+---------
 g_id        | integer                |           | not null |
 name        | character varying(100) |           | not null |
 avgscore    | numeric(7,6)           |           |          |
 numvotes    | numeric(6,1)           |           |          |
 minplayers  | smallint               |           |          |
 maxplayers  | smallint               |           |          |
 minplaytime | smallint               |           |          |
 maxplaytime | smallint               |           |          |
Indexes:
    "games_pkey" PRIMARY KEY, btree (g_id)
Referenced by:
    TABLE "desgame" CONSTRAINT "desgame_g_id_fkey" FOREIGN KEY (g_id) REFERENCES games(g_id)
    TABLE "gamecat" CONSTRAINT "gamecat_g_id_fkey" FOREIGN KEY (g_id) REFERENCES games(g_id)
```

The second class of useful commands are SQL commands. All SQL queries in PostgreSQL must be terminated with a semi-colon.
For example, to get a list of 10 records in the `games` table, you would type:

`SELECT * FROM games LIMIT 10;`

This query  requests a max 10 rows from the table. Using **LIMIT**  in this manner one can explore the data small bits at a time. If you really wanted to produce all the records, though, the query is:

`SELECT * FROM games;`

You can use Ctrl+C to end a query that is taking too long (it is very possible to write such bad queries even unintentionally).
Note that using the LIMIT keyword by itself offers no guarantee on which 10 rows from the result are
returned, so do not assume an ordering. \\

Finally, you can change the way psql displays the result sets to suit you better.
In particular, wrapped lines in `less` can make the output of wide tables hard to read. If this bothers you, you can try exiting the client
(you can use Ctrl+D) and starting it again with the LESS  env. variable set like this:

` LESS='-S' psql`



### Notes

If you are trying to add a predicate for a character attribute, you must wrap the value in a single quote (e.g name = 'Bob').


## Autograder
To allow you quickly debug your SQL statements we will provide an autograder on gradescope that will let you enter your answer and compare this against our expected right answer. **If your schema is not what
we expect then it will not compare results.** If all your answers are identical, you will see no output. Otherwise, if your answer is identical it will let you know. If you answer is partially right the comments give some hints and in some cases it will give you a sample record you missed or a sample record in your result that we are not expecting. The autograder is not perfect so if you are confident in your result and it does not think so let us know!

The output messages are not the best (blame Aaron). For example on prob 1 in the homework if you give `select name from games where avgscore > 6` you will get the following output, which is trying to tell you that your answer is different than ours as your records are wrong and it thinks you 96 more records than the answer does.

```
Autograder thinks:
SQL Result Comparison [TooManyRecords quantity=96]
SQL Result Comparison [RecordsWrong]
Your SQL: select name from games where avgscore > 6
```

To use the autograder, simply zip your answers/ directory and upload the zip file in a submission on gradescope. You will see both the autograder output with a score and a brief explanation about the error for each question.

### Submission

You have to submit 1 file for each item in a problem:  your query in SQL
(the file extension should be .sql).

For example, let’s assume that there is a problem 0 and item a, which
asks you to find the distinct names of 10 games. You have to create the following file named 0a.sql:

Contents:

`select distinct name from games limit 10;`

All the .sql  files for your answers are already creates, so you just need to insert your answers.
*No output is needed*

## Problems
For the below problems, you will connect to Postgres to use a preloaded database using:

`psql -d boardgames -U dbstudent -h addison.cs.uchicago.edu`

when it asks you for the password use *dbrocks33*  
Note that you can add a password to avoid having to type in the password every time https://www.postgresql.org/docs/10.6/static/libpq-pgpass.html

psql should be installed on the csil machines(linux.cs.uchicago.edu) so you can use it from there.

Alternatively you can use OmniDB (or another SQL Gui) to connect. 

Database Name=boardgames
host = addison.cs.uchicago.edu
Database User = dbstudent

Schemas for output as provided in parentheses. *Note that all SQL must run as a single SQL statement (eg no temp tables).*
However, note that CTEs (i.e., WITHs) are acceptable.

### 1
What is the name of the game with an avgscore of 6.226650 (given as name)

### 2
What are the names of games that either have (a) an avgscore >7.3 and < 7.6 or (b) have maxplayers = 9

### 3
What is the highest and lowest avgscore among all games (given as max,min)

### 4
What is the mean (eg average) avgscore, broken down by maxplayers (hint group by), sorted by maxplayers ascending (given as avg, maxplayers)?

### 5
For the above query, only show results where the mean avgscore (eg avg) is above 6, still sorted by maxplayers ascending (given as avg, maxplayers)

### 6
What is the mean (eg average) avgscore, broken down by maxplayers (hint group by) only considering games that have a maxplaytime < 100, sorted by maxplayers ascending (given as avg, maxplayers)?

### 7
What are the names and avgscore of the 10 highest rated games (avgscore) by avgscore in descending order (given as name, avgscore)

### 8
 Find games whose name contains the word *edition* with either uppercase or lowercase 'e'. (given as name)

### 9
Find the 10 longest game titles (by characters)  that allow for between 3 and 5 players inclusive (e.g. games that need at minimum 3 people to play (minplayers) and support a maximum of 5 players (maxplayers)) . (given as name, namelen)

Hint: See https://www.postgresql.org/docs/10/functions-string.html 

### 10
Find the category id and count of games in the category, for categories that have at least 15 games in them. (given as c_id, cnt)

### 11
Find the category name of the most 5 popular categories, where popularity is defined as the highest average(e.g. mean) of avgscore for all  games in the category (given as category, avg). Order the categories by average (e.g. mean) of avgscore descending, followed by category name ascending. Avgscore is an attribute of games that indicates what the game’s score is (where the values contributing to this come from users and are not included in the dataset).

### 12
Extend the query for problem 4 to only show categories that have at least 15 games in them. (given as category, avg). Maintain the same sort order.

### 13
For all groupings of minplayers and maxplayers (e.g.  group by minplayers,maxplayers) show the minimum number of hours and maximum **number of hours** sorted minplayers and then maxplayers (ascending for both). *Note that min/maxplaytime is given in minutes*. Do not round or truncate hours. (given as minhours,maxhours,minplayers,maxplayers)

### 14
For each category show the designer that has the longest possible game as max (maxplaytime) -- considering only the games that have designers associated with them (e.g. you can omit games that have no designer).

Subsequently, only consider categories that have at least one designer associated with its respective games (e.g. 'if no designer exists for any game in a category, then omit the category' or alternatively 'if there does not exist any game in this category with a designer, then ignore the category').
In case of a tie for the longest game and/or if a game has multiple designers, show all designers. Do not show the same designer multiple times per category. 

The final output relation should given as (designer,category,max)  and sorted by category descending, designer ascending

Two categories  from my expected answer to pay attention to or hopefully help guide you 

```
  Matt Leacock        | Word Game                 |   0
```

```
  Francesco Nepitello | Novel-based               | 240
  Marco Maggi         | Novel-based               | 240
  Roberto Di Meglio   | Novel-based               | 240
```

For the below problems, you will connect to Postgres to use a preloaded database using:

`psql -d yelp -U dbstudent -h addison.cs.uchicago.edu`

when it asks you for the password use *dbrocks33*  
Note that you can add a password to avoid having to type in the password every time https://www.postgresql.org/docs/10.6/static/libpq-pgpass.html

psql should be installed on the csil machines(linux.cs.uchicago.edu) so you can use it from there.

Alternatively you can use OmniDB (or another SQL Gui) to connect. 

Database Name=yelp
host = addison.cs.uchicago.edu
Database User = dbstudent

In the problems below we specify to only consider users that exist in their respective tables (e.g. do not count a review if there is no corresponding user for the review).  However, you should count/include reviews that do not have corresponding businesses tuples.
Note that queries running longer than 300 seconds will terminate.

* Note that all SQL must run as a single SQL statement (eg no temp tables)*

### 15
Find the 10 users with the most reviews. Only consider reviews that have a corresponding user tuple. In case of ties, break with name
ascending (e.g. bob before sarah).  (given as user_id, name, review_count);

### 16
For the 10 users from the prior question, calculate the average stars of all their reviews.
 Use a nested subquery to calculate the  result. (given as avg)

### 17
Find the number of distinct business_ids that do not have any reviews. For this query, consider the full review table regardless of matching users (e.g consider reviews that do not have any matching user_id in the users table). (given as count)

### 18
Amongst users from the user table that have at least 200 reviews, find the 2 “most similar” users, 
where the similarity of two users is defined as
the number of shared businesses they’ve reviewed. (given as user_id1, user_id2, similarity)

Count business_ids even if there is no corresponding id in the business table. Do not double count pairs (eg James, Sinclair is the same as Sinclair, James).



### 19
Amongst users from the user table that have at least 200 reviews, find the 2 “most similar” users, 
where the similarity of two users is defined as
the number of shared businesses they’ve visited. Assume a user has visited a business if he
or she has a review or a tip on a business. (given as user_id1, user_id2, similarity)
 Count business_ids even if there is no corresponding id in the business table. Do not double count pairs (eg James, Sinclair is the same as Sinclair, James). Do not double count visits (eg do not count user’s total reviewing and tipping of a location, rather count as a user visiting that location at least once).



### 20
Out of the users in the user table who have more than 50 reviews, find the user id whose reviews have the lowest average star rating (there may be 1 or more users with the same lowest average star rating); for this user(s), find the names and ids of the businesses they gave the highest star ratings to (again, there may be more than 1 such business). Your final output should be given as (user_id, user_name, business_id, business_name).

*Use an outer join to account for business_ids that do not match records in the business table*
