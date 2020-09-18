### Additional markdown resources:

<span style='color:'></span>

# SQL CREATE TABLE

----

- The <span style='color:green'>CREATE TABLE</span> command is used to create a table in the database. A table consists of a table name and a set of fields with their name and data types

- Example:

  ~~~mysql
  CREATE TABLE emp(
  	eno	CHAR(5),
  	ename VARCHAR(30) NOT NULL, //This means this field must always have a value
  	bdate	DATE,
  	title	CHAR(2),
  	salary DECIMAL(9,2),
  	supereno	CHAR(5),
  	dno	CHAR(5),
  	PRIMARY KEY (eno)
  )
  ~~~

- Data Types:
  - CHAR(5) - always 5 characters long
  - VARCHAR(30) - up to 30 characters long
  - DECIMAL(9,2) - 123456.99
  - DATE - 1998/01/18

# SQL CREATE TABLE Full Syntax

---

- Full syntax of CREATE TABLE statement

  ```mysql
  CREATE TABLE tableName (
      { attrName attrType  [NOT NULL] [UNIQUE] [PRIMARY KEY]
          [DEFAULT value] [CHECK (condition)] [, ...] }
       [PRIMARY KEY (colList) [, ...]]
       {[FOREIGN KEY (colList) REFERENCES tbl [(colList)]
          [ON UPDATE action]
          [ON DELETE action] [, ...] ] }
       {[CHECK (condition)] }
  );
  ```


# Example Database

---

```mysql
CREATE TABLE workson (
		eno CHAR(5),
		pno CHAR(5),
		resp VARCHAR(20),
		hours SMALLINT,
		PRIMARY KEY (eno,pno),
		FOREIGN KEY (eno) REFERENCES emp(eno)
						ON DELETE NO ACTION ON UPDATE CASCADE, FOREIGN KEY (pno) 			REFERENCES proj(pno)
						ON DELETE NO ACTION ON UPDATE CASCADE
   );
```



# Creating Schemas

----

- A <span style='color:green'>schema</span> is a collection of database objects (tables, views, domains, etc.) usually associated with a single user.

- Creating a schema: (User Joe creates the schema)

  ```mysql
  CREATE SCHEMA employeeSchema AUTHORIZATION Joe;
  ```

- Dropping a schema:

  ```mysql
  DROP SCHEMA employeeSchema;
  ```

  

# ALTER TABLE

---

- The <span style='color:green'>ALTER TABLE</span> command can be used to change an existing table. This is useful when the table already contains data and you want to add or remove a column or constraint
  - <span style='color:orange'>DB vendors may support only parts of ALTER TABLE or may allow additional changes including changing the data type of a column</span>
- General form:

```mysql
ALTER TABLE tableName
			[ADD [COLUMN] colName dataType [NOT NULL] [UNIQUE]
					[DEFAULT value] [CHECK (condition)] ]
			[DROP [COLUMN] colName [RESTRICT | CASCADE]
			[ADD [CONSTRAINT [constraintName]] constraintDef]
      [DROP CONSTRAINT constraintName [RESTRICT | CASCADE]]
      [ALTER [COLUMN] SET DEFAULT defValue]
			[ALTER [COLUMN] DROP DEFAULT]
```

#ALTER TABLE Examples

---

- **Add column** location **to** dept **relation:**

  ```mysql
  ALTER TABLE dept
  ADD location VARCHAR(50);
  ```

- **Add field** SSN to Emp **relation:** 

  ```mysql
  ALTER TABLE emp
  ADD SSN CHAR(10);
  ```

- **Indicate that** SSN **is** UNIQUE **in** emp:

  ```mysql
  ALTER TABLE emp
  	ADD CONSTRAINT ssnConst UNIQUE(SSN);
  ```

# Indexes

---

- Indexes are used to speed up access to the rows of a table based on the values of certain attributes.

  - <span style='color:orange'> An index will often significantly improve the performance of a query, however they represent an overhead as they must be updated every time the table is updated</span>

- The general syntax for creating and dropping indexes is:

  ```mysql
  CREATE [UNIQUE] INDEX indexName
  		ON tableName (colName [ASC|DESC] [,...])
  DROP INDEX indexName;
  ```

- <span style='color:yellow'>UNIQUE means that each value in the index is unique</span>

- <span style='color:yellow'>ASC/DESC specifies the sorted order of index</span>

# Indexes Example

---

- Creating an index on eno and pno in WorksOn is useful as it will speed up joins with the Emp and Proj tables respectively.

  - <span style='color:orange'>Index is not UNIQUE as eno (pno) can occur many times in WorksOn</span>

  ```mysql
  CREATE INDEX idxEno ON workson (eno);
  CREATE INDEX idxPno ON workson (pno);
  ```

- Most DBMSs will put an index on the primary key, but if they did not, this is what it would like for workson:

  ```mysql
  CREATE UNIQUE INDEX idxPK ON workson (eno,pno);
  ```

# Adding Data using INSERT

---

- Insert a row using the INSERT command:

  ```mysql
  INSERT INTO emp VALUES ('E9','S. Smith','1975-03-05', 'SA',60000,'E8','D1')
  ```

  - Fields: eno, ename, bdate, title, salary, supereno, dno

- If you do not give values for all fields in the order they are in the table, you must list the fields you are providing data for:

  ```mysql
  INSERT INTO emp(eno, ename, salary) VALUES ('E9','S. Smith',60000)
  ```

  - Note: If any columns are omitted from the list, they are set to NULL.

# INSERT Multiple Rows

---

- INSERT statement extended by many databases to take multiple rows:

  ```mysql
  INSERT INTO tableName [(column list)] 
  	VALUES (data value list) [, (values) ]*
  ```

- Example:

  ```mysql
  INSERT INTO emp (eno, ename) VALUES
  	('E10', 'Fred'), ('E11', 'Jane'), ('E12', 'Joe')
  ```

#INSERT rows from SELECT

---

- Insert multiple rows that are the result of a SELECT statement:

  ```mysql
  INSERT INTO tableName [(column list)] SELECT ...
  ```

- Example: Add rows to a temporary table that contains only employees with title ='EE'.

  ```mysql
  INSERT INTO tmpTable 
  	SELECT eno, ename 
  	FROM emp
  	WHERE title = 'EE'
  ```

# UPDATE Statement

---

- Updating existing rows using theUPDATEstatement. Examples:

  -  <span style='color:yellow'>1) Increase all employee salaries by 10%</span>

     ```mysql
    UPDATE emp SET salary = salary*1.10;
     ```

    

  -  <span style='color:yellow'>2)  Increase salary of employee E2 to $1 million and change his name:</span>

    ```mysql
    UPDATE emp SET salary = 1000000, name='Rich Guy' 
    	WHERE eno = 'E2';
    ```

- Notes: 

  -  <span style='color:yellow'>May change (SET) more than one value at a time. Separate by commas</span>
  -  <span style='color:yellow'>Use WHERE to filter only the rows to update</span>

# DELETE Statement

---

- Rows are deleted using the DELETE statement. Examples:

  - 1) Fire everyone in the company

    ```mysql
    DELETE FROM emp;
    ```

  - 2) Fire everyone making over $35,000

    ```mysql
    DELETE FROM emp
    	WHERE salary > 35000;
    ```

    

