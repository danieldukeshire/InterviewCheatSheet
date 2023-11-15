
# SQL Interview Questions

1. What is a Database?   
   A database is an organized collectoopn of data, stored and retrieved digitally from a remote or local computer system. Databases can be vast and complex.
2. What is a DBMS v RDBMS   
   RDBMS stands for Relational Database Management System. RDBMS compared to DBMS is that RDBMS stores data in the form of a collection of tables, and relations can be defined between the common fields  of these tables. Most modern database management systems are based on RDBMS>
3. What is the difference between SQL and MySQL?   
   SQL is a standard language for retrieving  and manipulating structured databases. On the contratry, MySQL is a relational database management system. 
4. What are Tables and Fields?   
   A table is an organized collection of data stored in the form of rows and columns. The columns in the table are called fields where rows are records.
5. What are Constraints in SQL?
   Contraints are used to specify the rules concerning data in the table. It can be applied for single or multiple fields in a SQL table during the creation of the table or after creating using the ALTER TABLE command.
	- NOT NULL - Restricts NULL value from being inserted into a column. 
	- CHECK - Verifies that all values in a field satisfy a condition.
	- DEFAULT - Automatically assigns a defauly value if no value has been specified.
	- UNIQUE - Ensures unique values to be inserted into the field. 
	- INDEX - Indexes a field providing faster retrieval of results.
	- PRIMARY KEY - Uniquely identifies each record in a table.
	- FOREIGN KEY - Ensures referential integrity for a record in another table.

8. What is a Primary Key?   
   The Primary Key constraing uniquely identifies each row in a table. It must contatin unique values and has an implicit NO NULL constraint. A table in SQL is strictly restricted to have one and only one primary key, which is comprised of single or multiple fields.
9. What is a Unique Constraint?   
   A Unique constriant ensures that all values in a column are different. This provides uniqueness for the columns and helps identify each row uniquely. Unlike primary keys, there can be multiple unique constraints defined per table. 
10. What is a Foreign Key?   
    A Foreign Key comprises of single or collection of fields in a table that refers to the Primary Key in another table. Foreign Key constraints ensures referential integrity in the relation betwen two tables.
11. What is a join?   
    The SQL Join clause is used to comb9ine records from two or more tables in a SQL database on a related column between the two.
	1. (INNER) JOIN - Retrieves records that have matching values in both tables involved in the join. 
	2. LEFT (OUTER) JOIN - Retrieves all the records/rows from the left and the matches records from the right. 
	3. RIGHT (OUTER) JOIN - Rretrieves all the records from the right and mataches the records from the left.
	4. FULL (OUTER) JOIN - Retrieves all the records where there is a match in the left or right table.
12. What is a CROSS JOIN?   
    A cartesian product of the two tables included in the join. The table after join contains the same number of rows as in the cross product of the number of rows in the two tables. IF a where clause is used in across join then the query will work like an INNER JOIN.
13. What is an Index?   
    A database index is a data structure that provides a quick lookup of data in a column or columns of data. It enhances the speed of operations accessing data from a database table at the cost of additional writes and memory to maintain the index data structures. 
	1. Unique and Non-Unique Index   
	   Unique Indexes are indexes that help maintain data integrity by ensuring that no two rows of data in a table have identical key values. Once a unique index has been defined for a table, uniquen ess is enforces whenever keys are added or changes within the index.   
	   Non Unique Indexes, on the other hand, are not used to enforce constraints on the tables with which they are associated. Instead, they are used solely to improve query performance by maintaining a sorted order of data values used frequently.
	2. Clustered and Non-Clustered   
       Clustered Indexes are indexes whose order of the rows in the database corresponds to the order of the rows in the index. This is why only one clustered index can exist in a given table, whereas, multiple non clustered indexes can exist in a table. The only difference between clustered and non clustered is that the database manager attempts to keep the data in the database in the same order as the coresponding keys appear in the clustered index. Clustering indexes can improve the performance of most query operations because they provide a linear accesss path to data stored in the database.   
	   Clustered indexes are best suited for columns that are frequently used for range-based queries or sorting, as they provide a significant performance boost for such operations.
	   They are typically used when you want to minimize data retrieval time for a specific set of columns or frequently used queries.
14. What is the difference between Clustered and Non Clustered Index?   
    - Clustered Index:
        A clustered index determines the physical order of data rows in a table.
        There can be only one clustered index per table because the data rows are physically sorted based on the clustered index key.
        Typically, the primary key of a table is used as the clustered index because it enforces uniqueness and defines the physical order of data.
        A table with a clustered index is also known as a clustered table.
        Clustered indexes are best suited for columns that are frequently used for range-based queries or sorting, as they provide a significant performance boost for such operations.
        They are typically used when you want to minimize data retrieval time for a specific set of columns or frequently used queries.

    - Non-Clustered Index:
        Non-clustered indexes are separate data structures that store a copy of the indexed columns along with a pointer to the actual data row.
        You can have multiple non-clustered indexes on a single table.
        Non-clustered indexes are used to improve the performance of SELECT, WHERE, and JOIN queries by allowing the database engine to quickly locate rows that meet specific criteria without scanning the entire table.
        Non-clustered indexes are suitable for columns often used in filtering and searching operations.
        They do not affect the physical order of data in the table and are typically used for columns other than the primary key.

15. When to use Clustered vs. Non-Clustered Indexes:
	Use a clustered index when you have a primary key or a column that is frequently used for range-based queries, as it will speed up these types of queries.
    Use non-clustered indexes when you need to improve the performance of SELECT, WHERE, or JOIN queries on specific columns, and the primary key or clustered index is not suitable for these queries.
    Consider creating non-clustered indexes on columns that are frequently used in WHERE clauses and joins, as they can significantly reduce query execution time.
    Be mindful of the trade-offs between index creation and maintenance. Indexes improve read performance but can slow down write operations, so you should carefully assess your application's read and write patterns when deciding which indexes to create.

16. What are UNION, MINUS, and INTERSECT commands?   
    - UNION combines and returns the result set retrieved by two or more SELECT statements.
    - MINUS operator is used to remove duplicates from the result set obtained by the second selcet query from the result set obtained by the first select query and then return the filtered results from the first.
    - INTERSECT combines the result set fetchedd by the two select statements where the records from one match the other and then returns this intersection.

17. What is a CURSOR?

18. Using transactions in sql
