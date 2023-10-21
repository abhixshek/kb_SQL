`INNER JOIN`

![[Pasted image 20231002133216.png]]

![[Pasted image 20231002133229.png]]

When same column name appears in the two tables being joined, make sure to write  `table_name.column_name` to avoid SQL errors.

````SQL
-- show countries having both a prime minister and a president
SELECT p1.country, p1.continent, prime_minister, president
FROM prime_ministers AS p1
INNER JOIN presidents AS p2
ON p1.country = p2.country;
````

**NOTE:** When joining 2 tables on the same column name, you can use the `USING` keyword as shown below:
````SQL
SELECT p1.country, p1.continent, prime_minister, president
FROM prime_ministers AS p1
INNER JOIN presidents AS p2
USING(country);
````

**NOTE:** A parting word of caution when using `USING`: columns can sometimes have the same name but actually contain vastly different data. Always remember to check what you are joining on by displaying and viewing your data first!


**Table relationships:**

![[Pasted image 20231002150715.png]]

An example of a **one-to-one relationship** is:` finger --- fingerprint`

![[Pasted image 20231002151306.png]]
That is, many languages can be spoken in a country, and a language can be spoken in many countries.

**Chaining JOINS:**
##### Q. Show which countries have both a prime minister and a president and show the term start date of the prime minister as well. 
````SQL
SELECT p1.country, p1.continent, president, prime_minister, president, pm_start
FROM prime_ministers AS p1
INNER JOIN presidents AS p2
USING(country)
INNER JOIN prime_minister_terms AS p3
USING(prime_minster); 
````

![[Pasted image 20231002152546.png]]

````SQL
SELECT name, e.year, fertility_rate, unemployment_rate
FROM countries AS c
INNER JOIN populations AS p
ON c.code = p.country_code
INNER JOIN economies AS e
ON c.code = e.code AND p.year = e.year;
````


**Outer Joins:**
Outer joins can obtain records from other tables, even if matches are not found for the field being joined on.

`LEFT JOIN`:
A LEFT JOIN will return all records in the left_table, and those records in the right_table that match on the joining field provided.
![[Pasted image 20231002221312.png]]

````SQL
SELECT p1.country, prime_minister, president
FROM prime_ministers AS p1
LEFT JOIN presidents AS p2
USING(country);
````
**NOTE:**` LEFT JOIN` can also be written as `LEFT OUTER JOIN`

**NOTE:** In SQL, the 2 most common type of joins are `INNER JOIN` and `LEFT JOIN`.

`RIGHT JOIN`:
![[Pasted image 20231002221826.png]]

````SQL
SELECT p1.country, prime_minister, president
FROM prime_ministers AS p1
RIGHT JOIN presidents AS p2
USING(country);
````

**NOTE:**  The order of the left and right tables will remain the same even in a `RIGHT JOIN`.

**NOTE:**` RIGHT JOIN` can also be written as `RIGHT OUTER JOIN`

The reason `RIGHT JOIN` is used less often, is because they can always be rewritten as a `LEFT JOIN` and secondly, users feel it intuitive writing from left to right, therefore are more comfortable with a `LEFT JOIN`. 


`FULL JOINS`
A `FULL JOIN` combines a` LEFT JOIN` and a `RIGHT JOIN`.
This is because the `FULL JOIN` will return all ids, irrespective of whether they have a match in the other table being joined.
![[Pasted image 20231004224643.png]]

````SQL
SELECT left_table.id, right_table.id, left_table.column2, right_table.column2
FROM left_table
FULL JOIN right_table
ON left_table.left_column_name = right_table.right_column_name;

````

**NOTE:**` FULL JOIN` can also be written as `FULL OUTER JOIN`


**Cross Joins:**
- used when you are interested in all possible combinations between two sets of data. 
![[Pasted image 20231004232543.png]]
````SQL
SELECT id1, id2
FROM table1
CROSS JOIN table2;
````
**NOTE:** we do not specify `ON` keyword in a `CROSS JOIN`.

![[Pasted image 20231004232815.png]]


**SELF JOINS:**
- self joins are tables joined with themselves
- they can be used to compare parts of the same table. 

**NOTE:**
1. There is no keywords in SQL for self join. 
2. Aliasing is MUST in a self join. 

````SQL
-- Countries within the same continent are to have bilateral meetings along themselves. Write a SQL query to return the result. 
SELECT
  p1.country AS country1,
  p2.country AS country2,
  p1.continent
FROM prime_ministers AS p1
INNER JOIN prime_ministers AS p2
ON p1.contient = p2.contient
  AND p1.country <> p2.country;
````

---
### Set theory for SQL Joins:

We have 3 set operations in SQL, `UNION`, `INTERSECT` and `EXCEPT`. 

![[Pasted image 20231007213913.png]]
````SQL
SELECT col1
FROM table1
UNION
SELECT col2
FROM table2;
````

**NOTE:**
`UNION` does not include duplicates. 
`UNION ALL` includes duplicates.

**NOTE:** For all set operations, the no. of selected columns and their respective data types must be identical. 

**NOTE:** The result set will contain names (aliases) of selected columns from the first table. That is, it does not matter what the column names you give in the subsequent tables after the set operation keyword(ex `UNION`), the result set will contain column names as in the first table's query.

**INTERSECT:**
````SQL
SELECT col1
FROM table1
INTERSECT
SELECT col2
FROM table2;
````

#### Q. What is the difference between INTERSECT and INNER JOIN ?
![[Pasted image 20231007230242.png]]
NOTICE how INNER JOIN returns all rows satisfying the condition, but INTERSECT only returns 1 record.  

**EXCEPT:**
![[Pasted image 20231007233309.png]]

````SQL
SELECT col1
FROM table1
EXCEPT
SELECT col2
FROM table2;

--A EXCEPT B is essentially A - B
````

**Semi-joins:**
- uses WHERE clause to filter data in 1 table using data in another table.
````SQL
-- find presidents whose country gained independence before 1800
SELECT president, country, continent
FROM presidents
WHERE country IN
  (SELECT country
   FROM states
   WHERE indep_year < 1800);

-- the query in parenthesis is called a sub-query
````

**Anti-joins:**
- uses WHERE clause to filter data in 1 table using data in another table.
````SQL
-- find presidents whose country did not gain independence before 1800
SELECT president, country, continent
FROM presidents
WHERE country NOT IN
  (SELECT country
   FROM states
   WHERE indep_year < 1800);
````

**NOTE:** The datatype of the result of the sub-query should match the data type of the column used in the WHERE clause of the main table.
**NOTE:** the table used in a sub-query can be the same table as the main query or a different table(usually).

**Sub-queries inside SELECT:**
````SQL
-- count the number of monarchs listed in the monarchs table for each continent in the states table.
SELECT DISTINCT continent, 
    (SELECT COUNT(*) 
     FROM monarchs
     WHERE states.continent = monarchs.continent) AS monarch_count
FROM states;
````

**Sub-queries inside FROM:**
- uses 2 tables in the `FROM` clause
````SQL
SELECT *
FROM left_table, right_table
WHERE left_table.key = right_table.key
````

````SQL
-- query to return all continents with monarchs and the year the most recent country got independence. 

SELECT DISTINCT monarchs.continent, sub.most_recent
FROM monarchs,
  (SELECT continent, MAX(indep_year) AS most_recent
   FROM states
   GROUP BY continent) AS sub
WHERE monarchs.continent = sub.continent
````

















