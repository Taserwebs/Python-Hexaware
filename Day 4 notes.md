SQL exercise [Link](https://sqlbolt.com/lesson/select_queries_introduction)
# SQL Lesson 8: A short note on NULLs
As promised in the last lesson, we are going to quickly talk about NULL values in an SQL database. It's always good to reduce the possibility of NULL values in databases because they require special attention when constructing queries, constraints (certain functions behave differently with null values) and when processing the results.

An alternative to NULL values in your database is to have data-type appropriate default values, like 0 for numerical data, empty strings for text data, etc. But if your database needs to store incomplete data, then NULL values can be appropriate if the default values will skew later analysis (for example, when taking averages of numerical data).

Sometimes, it's also not possible to avoid NULL values, as we saw in the last lesson when outer-joining two tables with asymmetric data. In these cases, you can test a column for NULL values in a WHERE clause by using either the IS NULL or IS NOT NULL constraint.
```sql
{
    SELECT column, another_column, …
    FROM mytable
    WHERE column IS/IS NOT NULL
    AND/OR another_condition
    AND/OR …;
}
```
# Exercise
This exercise will be a sort of review of the last few lessons. We're using the same Employees and Buildings table from the last lesson, but we've hired a few more people, who haven't yet been assigned a building.
![Table](5.png)
1. Find the name and role of all employees who have not been assigned to a building
Answer:
```sql
{
    SELECT name, role FROM employees
    WHERE building IS NULL;
}
```
2. Find the names of the buildings that hold no employees 
```sql
{
    SELECT DISTINCT building_name
    FROM buildings 
    LEFT JOIN employees
    ON building_name = building
    WHERE role IS NULL;
}
```
# SQL Lesson 9: Queries with expressions
In addition to querying and referencing raw column data with SQL, you can also use expressions to write more complex logic on column values in a query. These expressions can use mathematical and string functions along with basic arithmetic to transform values when the query is executed, as shown in this physics example.
Example query with expressions
```sql
{
    SELECT particle_speed / 2.0 AS half_particle_speed
    FROM physics_data
    WHERE ABS(particle_position) * 10.0 > 500;
}
```
Each database has its own supported set of mathematical, string, and date functions that can be used in a query, which you can find in their own respective docs.

The use of expressions can save time and extra post-processing of the result data, but can also make the query harder to read, so we recommend that when expressions are used in the SELECT part of the query, that they are also given a descriptive alias using the AS keyword.
Select query with expression aliases
```sql
{
    SELECT col_expression AS expr_description, …
    FROM mytable;
}
```
In addition to expressions, regular columns and even tables can also have aliases to make them easier to reference in the output and as a part of simplifying more complex queries.

Example query with both column and table name aliases
```sql
{
    SELECT column AS better_column_name, …
    FROM a_long_widgets_table_name AS mywidgets
    INNER JOIN widget_sales
    ON mywidgets.id = widget_sales.widget_id;
}
```
Exercise
You are going to have to use expressions to transform the BoxOffice data into something easier to understand for the tasks below.
![Table](6.png)
1. List all movies and their combined sales in millions of dollars
Answer:
```sql
{
    SELECT title, (domestic_sales + international_sales) / 1000000 AS gross_sales_millions
    FROM movies
    JOIN boxoffice
    ON movies.id = boxoffice.movie_id;
}
```
2. List all movies and their ratings in percent
```sql
{
    SELECT title, rating * 10 AS rating_percent
    FROM movies
    JOIN boxoffice
    ON movies.id = boxoffice.movie_id;
}
```
3. List all movies that were released on even number years
```sql
{
    SELECT title, year
    FROM movies
    WHERE year % 2 = 0;
}
```
