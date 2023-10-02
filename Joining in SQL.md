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


