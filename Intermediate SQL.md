SQL is the **most popular language for turning raw data stored in a database into actionable insights.** Using a database of films made around the world, this course covers:
- How to filter and compare data
- How to use aggregate functions to summarize data
- How to sort and group your data
- How to present your data cleanly using tools such as rounding and aliasing

`films database`
![[Pasted image 20230930150311.png]]
NOTICE how the table names are plural words or plural in sense like `people` table. 
Also notice the field names are singular. 

`COUNT()` 
- counts the no. of records with a value in a field. 
- always use an alias for `count()` for clarity using `AS` keyword

````SQL
SELECT COUNT(birthdate) AS count_birthdates
FROM people;

-- you can use COUNT() multiple times
SELECT COUNT(name) AS count_names, COUNT(birthdate) AS count_birthdates
FROM people;
````

| count_names | count_birthdates |
|------------ | ---------------- |
|6397 | 6152 |

**NOTE:** the count is different for each field, even though the total no. of rows would be technically same for both fields in the table. **This implies that NULL is not counted** in `COUNT()`

`COUNT(*)` - counts records in a table. (essentially meaning it includes missing values too)
`*` represents all fields. 

````SQL
SELECT COUNT(*) AS total_records
FROM people;

-- 8397
````

Use `COUNT(DISTINCT field_name)` to count distinct values in the field only. 
````SQL
SELECT COUNT(DISTINCT birthdate) AS count_distinct_birthdates
FROM people;

-- 5398
````

**Order of execution in SQL:**
Before SQL can fetch any data, it needs to know where to fetch it `FROM`. 
then our `SELECT`ion is made. and then further refinements like `LIMIT`

**SQL formatting:**
New lines, indentation, capitalization are NOT REQUIRED in SQL. 
Semicolon ; is also NOT REQUIRED in PostgreSQL. However, including a ; at the end of the query is considered best practice because 1. some SQL flavors require it, therefore it is better to get into a habit of adding it, 2. if your file contains multiple queries, semicolon ; helps identify the end of each of your queries. 

**NOTE:** It is possible to create field name with a space in it, ex: `release year`
This is a non-standard way of naming fields but in case you come across such a field name given by someone else in your team, you will have to include it in double quotes when querying it in order for SQL to treat it as a single entity rather than two. ex:
````SQL
SELECT "release year"
FROM films;
````


**Filtering / WHERE clause:**
`WHERE` clause is used to filter the results to answer your business question. 

Some commonly used comparison operators in a WHERE clause are:
`=,<=, >=, <, >, <> (not equal to)`

Order of execution for a query like below would be:
````SQL
SELECT item
FROM closet
WHERE color = 'green'
LIMIT 5;
````

```
1. FROM
2. WHERE
3. SELECT
4. LIMIT
```

3 important operators used in a `WHERE` clause:
`OR, AND, BETWEEN`
````SQL
-- OR: returns rows where atleast one of the criteria is satisfied 
SELECT title
FROM films
WHERE release_year = 2014
  OR release_year = 2020;
  
-- AND: returns rows where both the criteria are satisfied
SELECT title
FROM films
WHERE release_year > 1994
AND release_year < 2000;

-- when combining OR, AND conditions, it may be required to enclose individual conditions in parenthesis for semantics 
SELECT title
FROM films
WHERE (release_year = 1994 OR release_year = 1995)
  AND (certification = 'PG' OR certification = 'R');

-- BETWEEN provides an alternative to a long AND condition
SELECT title
FROM films
WHERE release_year
  BETWEEN 1994 AND 2000;


````

**NOTE:** `BETWEEN` is inclusive. So the results contain films including 1994 and 2000
![[Pasted image 20231001002951.png]]

````SQL
-- films released between 1994 and 2000 and their country is UK
SELECT title
FROM films
WHERE release_year
  BETWEEN 1994 AND 2000 AND country = 'UK';
````


**Filtering texts based on patterns:** `LIKE , NOT LIKE`
`LIKE` and `NOT LIKE` are case sensitive.

`%` matches zero, one or more characters
`_` (underscore) matches a single character

````SQL
-- finds people whose name ends with letter 'r'
SELECT name
FROM people
WHERE name LIKE '%r';

-- finds people whose name has letter 't' in the 3rd place
SELECT name
FROM people
WHERE name LIKE '__t%';
````

`IN` **operator:**
when you have multiple values to be included in a filtering operation, you can use `IN`
````SQL
SELECT title
FROM films
WHERE release_year IN (1994, 1999, 2010);

SELECT title
FROM films
WHERE country IN ('Germany', 'France');
````


**NULL values:**
In SQL, NULL represents a missing or unknown value. Why is this useful? In the real world, our databases will likely have empty fields either because of human error or because the information is not available or is unknown. Knowing how to handle these fields is essential as they can affect any analyses we do

````SQL
-- count how many records do not have a birthday value
SELECT COUNT(*) AS no_birthdates
FROM people
WHERE birthdate IS NULL;
-- 2245

-- count of how many records have a birthdate value
SELECT COUNT(*) AS count_birthdates
FROM people
WHERE birthdate IS NOT NULL;
````

---
**Aggregate functions:** 
- return a single value
`AVG(), SUM(), MIN(), MAX(), COUNT()`
````SQL
SELECT AVG(budget) AS avg_budget
FROM films;
--41072235.18324507

SELECT SUM(budget) AS total_budget
FROM films;

SELECT MIN(budget) AS min_budget
FROM films;

SELECT MAX(budget) AS max_budget
FROM films;
````

**NOTE:** `AVG() `and` SUM()` can only be used with numerical fields. While `COUNT(), MIN(), MAX()` can be used with other some of the other data types as well. 

```
MIN()                 <-> MAX()

--in numeric values
Minimum numeric value <-> Maximum numeric value

-- in strings
A                     <-> Z

-- in dates
2010-05-24            <-> 2023-03-28
```

Use `ROUND()` to limit the no. of decimal places:
````SQL
SELECT ROUND(AVG(budget), 2) AS avg_budget
FROM films
WHERE release_year >= 2010;
--41072235.18
````

**NOTE:** 
The 2nd argument to ROUND() is optional, when its not passed, the answer is rounded to the nearest whole number. 
````SQL
SELECT ROUND(AVG(budget)) AS avg_budget
FROM films
WHERE release_year >= 2010;
-- 41072235

-- passing 0 as the 2nd argument gives the same result as above
SELECT ROUND(AVG(budget), 0) AS avg_budget
FROM films
WHERE release_year >= 2010;
-- 41072235

SELECT title, ROUND(duration / 60.0, 2) AS duration_hours
FROM films;
````

**NOTE:** 
passing a negative value as the 2nd argument rounds digits to the left of the decimal point. 
````SQL
SELECT ROUND(AVG(budget), -5) AS avg_budget
FROM films
WHERE release_year >= 2010;
-- 41100000
````

-1 means rounding to the nearest tens
-2 means rounding to the nearest hundreds, etc

**Arithmetic operations:**
`+, -, *, /`

````SQL
SELECT (4 + 3);
-- 7
SELECT (4 - 3);
-- 1
SELECT (4 * 3);
-- 12
SELECT (4 / 3);
-- 1
-- INT divided by INT gives an INT

SELECT (4.0 / 3.0);
-- 1.333...

````

````SQL
SELECT (gross - budget) AS profit
FROM films;
````

**Order of execution:**
1. `FROM`
2. `WHERE`
3. `SELECT` (aliases are defined here)
4. `LIMIT`

because `SELECT` happens after `WHERE` clause you cannot use the alias in the WHERE clause.
example below query gives an error:
`SELECT budget as movie_budget FROM films WHERE movie_budget > 1000;`

**Sorting results:**
`ORDER BY`
- by default it sorts in ascending order

````SQL
SELECT title, budget
FROM films
ORDER BY budget;

SELECT title, budget
FROM films
ORDER BY budget ASC;
--same result as the first query

SELECT title, budget
FROM films
ORDER BY budget DESC;
--sorts in descending order
````

![[Pasted image 20231001170243.png]]

NOTICE above that when sorting by a string field, values starting with symbols or numbers come before letter A. 

**NOTE:** when ordering by in descending order a field that has NULL values, then NULL values come up before the highest non-null value. 

**NOTE:** the `ORDER BY` field, does not necessarily have to come in the SELECT clause. 
ex:
````SQL
SELECT title
FROM films
ORDER BY gross DESC;
-- notice that gross is not in the SELECT clause but is only being used for sorting the results.
````

**NOTE:** `ORDER BY` can have multiple fields to sort on.
ex:
````SQL
SELECT title
FROM films
ORDER BY release_year, gross DESC;
-- sorts on release_year ascending and when there is a tie in release year value, it looks at the gross field to determine which row should come first. 
````

**Order of execution:**
1. `FROM`
2. `WHERE`
3. `SELECT`
4. `ORDER BY`
5. `LIMIT`


**GROUPING:**
`GROUP BY`
- find summary statistics for each group of data.
````SQL
SELECT release_year, COUNT(title) AS total_titles
FROM films
GROUP BY release_year;

SELECT release_year, language, COUNT(title) AS total_titles
FROM films
GROUP BY release_year, language;

-- using ORDER BY in conjuction with GROUP BY to sort the most important information on the top
SELECT release_year, language, COUNT(title) AS total_titles
FROM films
GROUP BY release_year, language
ORDER BY total_titles DESC;
--NOTE the alias of the SELECT clause can come in the ORDER BY clause because its after select in the order of execution
````
**NOTE:** the alias of the `SELECT` clause can come in the` ORDER BY` clause because its after select in the order of execution.

**Order of execution:**
1. `FROM`
2. `GROUP BY`
3. `SELECT`
4. `ORDER  BY`
5. `LIMIT`

**Filtering grouped data:**
`HAVING` clause
In SQL, we cant filter aggregate functions with `WHERE` clauses. 
`WHERE` filters individual records, while `HAVING` filters grouped records
````SQL
SELECT release_year, COUNT(title) AS total_titles
FROM films
GROUP BY release_year
HAVING COUNT(title) > 10;
````

**Order of execution:**
1. `FROM`
2. `WHERE`
3. `GROUP BY`
4. `HAVING`
5. `SELECT`
6. `ORDER BY`
7. `LIMIT`

**NOTE:** because `HAVING` comes before `SELECT` in the order of execution you cannot use the alias `total_titles` in the `HAVING` clause.


