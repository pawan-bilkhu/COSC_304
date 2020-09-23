# Lecture 1

# What is a Database?

--------------

- A database is a collection of logically related data for a particular domain.
- A <span style='color:green'>database management system (DBMS) </span>is software designed for the creation and management of databases.
  - <span style='color:yellow'>e.g. Oracle, DB2, Microsoft Access, MySQL, SQL Server, MongoDB</span>
- Bottom line: A <span style='color:green'>database</span> is the <span style='color:yellow'>data</span> stored and a <span style='color:green'>database system </span> is the <span style='color:yellow'>software</span> that manages the data

# Database Terminology

----

- A <span style='color:green'>data model</span> is a collection of concepts that is used to describe the structure of a database. E.g. relational, XML, graphs, objects, JSON
  - <span style='color:yellow'>In the relational model, data is represented as tables and fields</span>
- <span style='color:green'>Data Definition Language (DDL)</span> allows the user to create data structures in the data model used by the database. A <span style='color:green'>schema</span> is a description of the structure of the database and is maintained and stored in the <span style='color:green'>system catalogue</span>. The schema is <span style='color:green'>metadata</span>.
  - <span style='color:yellow'>A schema contains structures, names, and types of data stored.</span>
- Once a adatabse has been created using DDL, the user accesses data using a <span style='color:green'>Data Manipulation Language (DML)</span>.
  - <span style='color:yellow'> The DML allows for the insertion, modification, retrieval, and deletion of data</span>.
- SQL is a standard DDL and DML for the relational model.

# Lecture 2

# The Relational Model: Terminology

----

- The <span style='color:green'>relational model</span> organizes data into tables called relations
- A  <span style='color:green'>relation</span> is a table with columns and rows
- An  <span style='color:green'>attribute</span> is a named column of a relation
- A  <span style='color:green'>tuple</span> is a row of a relation
- A  <span style='color:green'>domain</span> is a set of allowable values for one or more attributes
- The  <span style='color:green'>degree</span> of a relation is the number of attributes it contains
- The  <span style='color:green'>cardinality</span> of a relation is the number of tuples it contains.
- The  <span style='color:green'>intension</span> is the structure of the relation including its domains (approximately the number of columns)
  - Typically represented as SQL create statement
- The  <span style='color:green'>extension</span> is the set of tuples currently in the relation (# of rows)
  - Extension is usually bigger than intension

# Relational Keys

-----

- Keys are used to unequal identify a tuple in a relation
  - <span style='color:orange'>Note that keys apply to the schema not to the data. That is, looking at the current data cannot tell you for sure if the set of attributes is a key</span>
- A <span style='color:green'>superkey</span> is a set of attributes that uniquely identifies a tuple in a relation
- A <span style='color:green'>(candidate) key</span> is a minimal set of attributes that uniquely identifies a tuple in a relation
  - <span style='color:yellow'>There may be more than 1 candidate key for a relation with different # of attribues</span> 
- A <span style='color:green'>primary key</span> is the candidate key designated as the distinguishing key of a relation.
- A <span style='color:green'>foreign key</span> is a set of attributes in one relation referring to the primary key of a relation.
  - <span style='color:orange'>Foreign keys enforece referential integrity. Note: A FK may refer to its own relation </span>

NOTE: ABSTRACT AS MUCH AS POSSIBLE ASSUME THE DATABASE CONTAINS INFORMATION ABOUT THE ENTIRE EARTH

# Relational Integrity

----

- Integrity rules are used to insure the data is accurate.

- <span style='color:green'>Constraints</span> are rules or restrictions that apply to the database and limit the data values it may store.
- Types of constraints:
  - <span style='color:green'>Domain constraint</span> - <span style='color:yellow'>Every value for an attribute must be an element of the attribute's domain or be <i>null</i></span>
    - <span style='color:orange'><i>null</i> represents a value that is currently unknown or not applicable</span>
    - <span style='color:orange'><i>null</i> is not the same as zero or an empty string</span>
  - <span style='color:green'>Entity integrity constraint</span> - <span style='color:yellow'>No attribue of a primary key can be null (count rows)</span>
  - <span style='color:green'>Referential integrity constraing</span> - <span style='color:yellow'>If a foreign key exists in a relation, then the foreign key value must match a primary key value of a tuple in the referenced relation or be null</span> 

# Lecture 3

#Types of Joins

----

- The $\theta$-Join is a general join that allows any expression for condition F. However, there are more specialized joins that are frequentyl used
- An <span style='color:green'>equijoin</span> only contains the equality operator (=) in formula $F$
  - <span style='color:yellow'>e.g.</span> WorksOn $\bowtie_{\text{WorksOn.pno = Proj.pno}}$Proj
- A <span style='color:green'>natural join</span> over two relations $R$ and $S$ denoted by $R \bowtie S$ is the equijoin of $R$ and $S$ over a set of attributes common to both $R$ and $S$
  - <span style='color:yellow'>It removes the "extra copies" of the join attributes</span>
  - <span style='color:yellow'>The attrivutes must have the same name in both relations</span>

# Objectives

- Given a relational schema and instance be able to translate english queries into relational algebra and show the resulting relation

# Lecture 4

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

# Lecture 5

# SQL Queries using SELECT

----

- A query in SQL has the form:

  ```mysql
  SELECT (list of columns or expressions)
  FROM (list of tables)
  WHERE (filter conditions)
  GROUP BY (columns)
  ORDER BY (columns)
  ```

  

- Notes:
  - <span style='color:yellow'>1) Separate the list of columns/expressions and list of tables by </span><span style='color:green'>commas</span>
  - <span style='color:yellow'>2) The "*" is used to select all columns</span>
  - <span style='color:yellow'>3) Only SELECT required. FROM, WHERE, GROUP BY, ORDER BY are optional</span>



# SQL and Relational Algebra

---

- The SELECT statement can be mapped directly to relational algebra
  $$
  \begin{align}
  &\text{SELECT } A_1,A_2, ..., A_n \\
  &\text{FROM } R_1, R_2, ..., R_m \\
  &\text{WHERE } P\\\\
  
  \text{Is equivalent to:}\\\\
  
  &\Pi_{A_1,A_2,...,A_n}(\sigma_{p}(R_1 \times R_2 \times \cdots R_m))
  
  \end{align}
  $$
  