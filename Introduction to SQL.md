Much of the world's raw data—from electronic medical records to customer transaction histories—lives in organized collections of tables called relational databases. Being able to wrangle and extract data from these databases using SQL is an essential skill within the data industry and in increasing demand.

SQL is an essential language for building and maintaining relational databases, which opens the door to a range of careers in the data industry and beyond. You’ll start this course by covering data organization, tables, and best practices for database construction.

Relational databases store data in objects called tables. 

Relational Database advantages over something like spreadsheets is:
- lot more storage 
- more secure as data is encrypted. 
- multiple users can query the database to find insights simultaneously

SQL (Structured query language) is the most widely used programming language for creating, querying and updating databases. 

Table *rows* and *columns* are commonly referred to as **records** and **fields** respectively. 
Fields are defined when the table is created. And therefore are limited. the no. of rows that can be added to a table are unlimited. 

**NOTE:** Table names should be lower case and should not include spaces. Use underscores instead of spaces. Table names can refer to collective groups or be plural, but they should not be singular. 

**NOTE:**
- Field names should be lower case and should not include spaces. 
- Field names should be singular. ex: year, gender, name, card_num. these are all singular names. 
- Field name should not be same as table name. 
- No 2 field names can be kept the same. 

![[Pasted image 20230927232535.png]]

**NOTE:** Having more tables, each with a clearly marked subject, is generally better than having fewer tables where information about multiple subjects is combined.
In the image below, the top 2 tables store different kind of data, one holds `patrons` registered and the other `checkouts` data. Notice how the bottom table combines the 2 tables into a single table **making it much harder to read and infer the table**. Also note the bottom table now has multiple `card_num` repeated multiple times as Izzy checkout books multiple times. Therefore `card_num` is no longer a unique identifier in the `patron_checkouts` table. 
![[Pasted image 20230927233034.png]]
We can always use SQL to gather information from multiple related tables and connect them if a question requires it, but table topics should remain separate.

When a table is created, a data type must be indicated for each field. The data type is chosen based on the type of data that the field will hold - a number, text, or a date for example. We use data types for several reasons. First, different types of data are stored differently and take up different amounts of storage space. Second, some operations only apply to certain data types. It makes sense to multiply a number by another number, but it does not make sense to multiply text by other text for example.

SQL has several different data types that can hold strings. One of the most commonly used one is `VARCHAR`. `VARCHAR` data type is more flexible and can store small or large strings - up to tens of thousands of characters!

For storing integer data, we have `INT` data type. There are more data types available that can hold integer data, but `INT` is the most common. 

For storing float data, we have `NUMERIC` data type. The NUMERIC data type can store floats which have up to 38 digits total - including those before and after the decimal point.

**Schemas:**
Schemas are often referred to as "blueprints" of databases. A schema shows a database's design, such as what tables are included in the database and any relationships between its tables.

A schema also lets the reader know what data type each field can hold. The schema for our library database shows the `VARCHAR` data type is used for strings like book title, author, and genre. We can also see that the patrons table is related to the checkouts table, but not the books table.
![[Pasted image 20230928001156.png]]

Finally, let's discuss storage. **The information we find in a database table is physically stored on the hard disk of a server.** Servers are centralized computers that perform services via requests made over a network. In our case, the service performed is data access, but servers are also used to access websites or files stored on the server. Any computer can be a server if it is set up to provide a service, even a laptop! However, servers are generally very powerful and large machines, because they are best equipped to handle a high volume of requests and data.

