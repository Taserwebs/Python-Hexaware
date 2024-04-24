# Ms Sql

SQL exercise [Link](https://sqlbolt.com/lesson/select_queries_introduction)

# SQL Lesson 1: SELECT queries 101
    To retrieve data from a SQL database, we need to write SELECT statements, which are often colloquially refered to as queries. A query in itself is just a statement which declares what data we are looking for, where to find it in the database, and optionally, how to transform it before it is returned. It has a specific syntax though, which is what we are going to learn in the following exercises.

    As we mentioned in the introduction, you can think of a table in SQL as a type of an entity (ie. Dogs), and each row in that table as a specific instance of that type (ie. A pug, a beagle, a different colored pug, etc). This means that the columns would then represent the common properties shared by all instances of that entity (ie. Color of fur, length of tail, etc).

    And given a table of data, the most basic query we could write would be one that selects for a couple columns (properties) of the table with all the rows (instances).
```sql
{
    Select query for a specific columns
        SELECT column, another_column, …
        FROM mytable;
}
```
    The result of this query will be a two-dimensional set of rows and columns, effectively a copy of the table, but only with the columns that we requested.

    If we want to retrieve absolutely all the columns of data from a table, we can then use the asterisk (*) shorthand in place of listing all the column names individually.

#### Select query for all columns
```sql
{
SELECT * 
FROM mytable;
}
```

    This query, in particular, is really useful because it's a simple way to inspect a table by dumping all the data at once.

# Exercise

    We will be using a database with data about some of Pixar's classic movies for most of our exercises. This first exercise will only involve the Movies table, and the default query below currently shows all the properties of each movie. To continue onto the next lesson, alter the query to find the exact information we need for each task.


|Id|Title|Director|Year|Length_minutes| 
|----|-------|----------|------|----------------|
| 1  |Toy Story|John Lasseter|1995|81 |
|2|A Bug's Life|  John Lasseter        |      |                |
|   3 |     	Toy Story 2   | John Lasseter         |      |                |
|    4|    	Monsters, Inc.   |    Pete Docter      |      |                |
|    5|   Finding Nemo    |     	Andrew Stanton     |      |                |

Answer:     
``` sql
{
    SELECT title FROM movies;
}
```
# SQL Lesson 2: Queries with constraints (Pt. 1)
    Now we know how to select for specific columns of data from a table, but if you had a table with a hundred million rows of data, reading through all the rows would be inefficient and perhaps even impossible.

    In order to filter certain results from being returned, we need to use a WHERE clause in the query. The clause is applied to each row of data by checking specific column values to determine whether it should be included in the results or not.

```sql
{
    SELECT column, another_column, …
FROM mytable
WHERE condition
    AND/OR another_condition
    AND/OR …;
}
```

# Exercise
        Using the right constraints, find the information we need from the Movies table for each task below.

ANS : 
```sql
{
    SELECT * FROM movies,
    Where id = 6;
}
```

# SQL Lesson 3: Queries with constraints (Pt. 2)
    When writing WHERE clauses with columns containing text data, SQL supports a number of useful operators to do things like case-insensitive string comparison and wildcard pattern matching. We show a few common text-data specific operators below:

 ![Table](/Screenshot%202024-04-24%20143530.png)
    We should note that while most database implementations are quite efficient when using these operators, full-text search is best left to dedicated libraries like Apache Lucene or Sphinx. These libraries are designed specifically to do full text search, and as a result are more efficient and can support a wider variety of search features including internationalization and advanced queries.

# Exercise
    Here's the definition of a query with a WHERE clause again, go ahead and try and write some queries with the operators above to limit the results to the information we need in the tasks below.



Select query with constraints
```sql
{
    SELECT column, another_column, …
    FROM mytable
    WHERE condition
    AND/OR another_condition
    AND/OR …;
}
```
![Table](/Screenshot%202024-04-24%20145824.png)
 1. Find all the Toy Story movies
        
    ```sql
    {
        Select * from movies 
        where title like "%toy story%";
    }
    ```
 2. Find all the movies directed by John Lasseter
    ```sql
    {
        Select * from movies 
        where Director like "%John Lasseter%";
    }
    ```
 3. Find all the movies (and director) not directed by John Lasseter

    ```sql
    {
      Select * from movies 
      where Director Not like "%John Lasseter%";  
    }
    ```
4. Find all the WALL-* movies
    ```sql
    {
        SELECT * FROM movies where title LIKE "%WALL%";
    }
    ```
# SQL Lesson 4: Filtering and sorting Query results
    Even though the data in a database may be unique, the results of any particular query may not be – take our Movies table for example, many different movies can be released the same year. In such cases, SQL provides a convenient way to discard rows that have a duplicate column value by using the DISTINCT keyword.
```sql
{
    SELECT DISTINCT column, another_column, …
    FROM mytable
    WHERE condition(s);
}
```
    Since the DISTINCT keyword will blindly remove duplicate rows, we will learn in a future lesson how to discard duplicates based on specific columns using grouping and the GROUP BY clause.

# Ordering results
    Unlike our neatly ordered table in the last few lessons, most data in real databases are added in no particular column order. As a result, it can be difficult to read through and understand the results of a query as the size of a table increases to thousands or even millions rows.

    To help with this, SQL provides a way to sort your results by a given column in ascending or descending order using the ORDER BY clause.
```sql
{
    SELECT column, another_column, …
    FROM mytable
    WHERE condition(s)
    ORDER BY column ASC/DESC;
}
```
    When an ORDER BY clause is specified, each row is sorted alpha-numerically based on the specified column's value. In some databases, you can also specify a collation to better sort data containing international text.

# Limiting results to a subset
    Another clause which is commonly used with the ORDER BY clause are the LIMIT and OFFSET clauses, which are a useful optimization to indicate to the database the subset of the results you care about.
    The LIMIT will reduce the number of rows to return, and the optional OFFSET will specify where to begin counting the number rows from.
```sql
{
    SELECT column, another_column, …
    FROM mytable
    WHERE condition(s)
    ORDER BY column ASC/DESC
    LIMIT num_limit OFFSET num_offset;
}
```
    If you think about websites like Reddit or Pinterest, the front page is a list of links sorted by popularity and time, and each subsequent page can be represented by sets of links at different offsets in the database. Using these clauses, the database can then execute queries faster and more efficiently by processing and returning only the requested content.
# Exercise
    There are a few concepts in this lesson, but all are pretty straight-forward to apply. To spice things up, we've gone and scrambled the Movies table for you in the exercise to better mimic what kind of data you might see in real life. Try and use the necessary keywords and clauses introduced above in your queries.
![Table](/1.png)
1. List all directors of Pixar movies (alphabetically), without duplicates
Answer:
```sql
{
SELECT distinct director
FROM movies
order by Director;
}
```
Note: Distinct means no duplicate elements.
 2. List the last four Pixar movies released (ordered from most recent to least)

```sql
{
    SELECT * From
    FROM movies
    order by year desc limit 4;
}
```
 3. List the first five Pixar movies sorted alphabetically
 ```sql
 {
    select * from movies order by title limit 5;
 }
 ```
 4. List the next five Pixar movies sorted alphabetically
 ```sql
 {
    SELECT * FROM movies order by title  limit 5 offset 5;
 }
 ```
 # SQL Review: Simple SELECT Queries
    You've done a good job getting to this point! Now that you've gotten a taste of how to write a basic query, you need to practice writing queries that solve actual problems.
```sql
{
    SELECT column, another_column, …
    FROM mytable
    WHERE condition(s)
    ORDER BY column ASC/DESC
    LIMIT num_limit OFFSET num_offset;
}
```
# Exercise
    In the exercise below, you will be working with a different table. This table instead contains information about a few of the most populous cities of North America[1] including their population and geo-spatial location in the world.
![Table](/2.png)
 1. List all the Canadian cities and their populations
 Answer:
 ```sql
 {
    SELECT city FROM north_american_cities
    Where country Like "%Canada%"
 }
 ```
 2. Order all the cities in the United States by their latitude from north to south
 ```sql
 {
    SELECT * FROM north_american_cities where country like "united states" order by latitude desc;
 }
 ```
 3. List all the cities west of Chicago, ordered from west to east
 ```sql
 {
    SELECT City FROM north_american_cities where longitude < -87.629798 order by longitude;
 }
 ```
 4. List the two largest cities in Mexico (by population)
 ```sql
 {
    SELECT * FROM north_american_cities where country like "mexico" order by population desc limit 2;
 }
 ```
 5. List the third and fourth largest cities (by population) in the United States and their population
 ```sql
 {
    SELECT * FROM north_american_cities where country like "united states" order by population desc limit 2 offset 2
 }
 ```
 # SQL Lesson 6: Multi-table queries with JOINs
    Up to now, we've been working with a single table, but entity data in the real world is often broken down into pieces and stored across multiple orthogonal tables using a process known as normalization[1].

# Database normalization
    Database normalization is useful because it minimizes duplicate data in any single table, and allows for data in the database to grow independently of each other (ie. Types of car engines can grow independent of each type of car). As a trade-off, queries get slightly more complex since they have to be able to find data from different parts of the database, and performance issues can arise when working with many large tables.

    In order to answer questions about an entity that has data spanning multiple tables in a normalized database, we need to learn how to write a query that can combine all that data and pull out exactly the information we need.

# Multi-table queries with JOINs
    Tables that share information about a single entity need to have a primary key that identifies that entity uniquely across the database. One common primary key type is an auto-incrementing integer (because they are space efficient), but it can also be a string, hashed value, so long as it is unique.

    Using the JOIN clause in a query, we can combine row data across two separate tables using this unique key. The first of the joins that we will introduce is the INNER JOIN.
```sql
{
    SELECT column, another_table_column, …
    FROM mytable
    INNER JOIN another_table 
    ON mytable.id = another_table.id
    WHERE condition(s)
    ORDER BY column, … ASC/DESC
    LIMIT num_limit OFFSET num_offset;
}
```
    The INNER JOIN is a process that matches rows from the first table and the second table which have the same key (as defined by the ON constraint) to create a result row with the combined columns from both tables. After the tables are joined, the other clauses we learned previously are then applied.
# Exercise
    We've added a new table to the Pixar database so that you can try practicing some joins. The BoxOffice table stores information about the ratings and sales of each particular Pixar movie, and the Movie_id column in that table corresponds with the Id column in the Movies table 1-to-1. Try and solve the tasks below using the INNER JOIN introduced above.

    