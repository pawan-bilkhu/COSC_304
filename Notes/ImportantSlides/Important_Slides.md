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