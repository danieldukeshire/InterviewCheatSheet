# Queries

#### **SELECT**: used to select data from a database
* ```sql
  SELECT * FROM table_name;
  ```

#### **DISTINCT**: filters duplicates
* ```sql
  SELECT DISTINCT column_name
  ```

#### **ORDER BY**: used to sort results in ascending or descending order
* ```sql
  SELECT * FROM table_name ORDER BY column DESC;
  ```
* ```sql
  SELECT * FROM table_name ORDER BY column1 ASC, column2 DESC;
  ```

#### **WHERE**: used for filtering records
* ```sql
  SELECT column1, column2 FROM table_name WHERE condition;
  ```
* ```sql
  SELECT * FROM table_name WHERE EXISTS (SELECT column_name FROM table_name WHERE condition);
  ```

#### **LIKE**: operator used in a WHERE clause to search for a pattern within a column
* ```sql
  SELECT column FROM table_name WHERE column_name LIKE pattern;
  ```
* Find values that start with "a"
  * ```sql
    LIKE 'a%'
    ```
* Find values that start end with "a"
  * ```sql
    LIKE '%a'
    ```
* Find values that contain "ab"
  * ```sql
    LIKE '%ab%'
    ```

#### **IN**: operator used to specify multiple values in a WHERE clause
* ```sql
  SELECT columns FROM table_name WHERE column_name IN (value1, value2, ...);
  ```

#### **BETWEEN**: operator used to select values within a given range
* ```sql
  SELECT columns FROM table_name WHERE column_name BETWEEN value1 AND value2;
  ```
* ```sql
  SELECT columns FROM table_name WHERE (column_name BETWEEN value1 AND value2) AND NOT column_name2 IN (value3, value4);
  ```