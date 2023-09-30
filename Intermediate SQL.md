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
- always use an alias for `count()` for clarity

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

`COUNT(*)` - counts records in a table. 
`*` represents all fields. 

````SQL
SELECT COUNT(*) AS total_records
FROM people;

-- 8397
````

Use `DISTINCT` in `COUNT()`to count distinct values only. 
````SQL
SELECT COUNT(DISTINCT birthdate) AS count_distinct_birthdates
FROM people;

-- 5398
````



















