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


