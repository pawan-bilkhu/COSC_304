### Additional markdown resources:

<span style='color:'></span>

# Subqueries

----

- SQL allows a single query to have multiple subqueries nested inside of it. This allows for more complex queries to be written

- When queries are nested, the outer statement determines the contents of the final result, while the inner SELECT statements are used by the outer statement (often to lookup values for WHERE clauses)

  ```mysql
  SELECT ename, salary, bdate
  FROM emp
  WHERE salary > (SELECT AVG(salary) FROM emp)
  ```

- A subquery can be in the SELECT, FROM, WHERE or HAVING clause

# Types of Subqueries

---

- There are three types of subqueries:
  - <span style='color:yellow'>1)</span><span style='color:green'> scalar subqueries</span><span style='color:yellow'>\- return a single value. Often value is then used in a comparison</span>
    - <span style='color:orange'>If query is written so that it expects a subquery to return a single value, and if it returns multiple values or no values, a run-time error occurs</span>
  - <span style='color:yellow'>2)</span><span style='color:green'> row subquery</span><span style='color:yellow'>\- returns a single row which may have multiple columns</span>
  - <span style='color:yellow'>3)</span><span style='color:green'>table subquery</span><span style='color:yellow'>\- returns one or more columns and multiple rows</span>

# Scalar Subquery Example

---

- Return the mployees that are in the 'Accounting' department:

  ```mysql
  SELECT ename
  FROM emp
  WHERE dno = (SELECT dno FROM dept WHERE dname = 'Accounting')
  ```

- Return all employees who work more hours than average on a single project:

  ```mysql
  SELECT ename
  FROM emp JOIN workson ON workson.eno = emp.eno
  WHERE workson.hours > (SELECT AVG(hours) FROM workson)
  ```

# Table Subqueries

----

- A table subquery returns a relation. There are several operators that can be used:
  - <span style='color:yellow'>EXISTS R - true if R is not empty</span>
  - <span style='color:yellow'>s IN R - true if s is equal to one of the values of R</span>
  - <span style='color:yellow'>s > ALL R - true if s is greater than </span><span style='color:green'>every</span> <span style='color:yellow'>value in R</span>
  - <span style='color:yellow'>s > ANY R - true if s is greater than  </span><span style='color:green'>any</span> <span style='color:yellow'>value in R</span>
- Notes:
  - <span style='color:yellow'>• 1) Any of the comparison operators (<, <=, =, etc.) can be used</span>
  - <span style='color:yellow'>2) The keyword NOT can proceed any of the operators</span>
    - <span style='color:orange'>Example: s NOT IN R</span>

# Table Subquery Examples

----

- Return all departments who have a project with a budget greater than $300,000:

  ```mysql
  SELECT dname FROM dept WHERE dno IN
  (SELECT dno FROM proj WHERE budget > 300000)
  ```

- Return all projects that 'J. Doe' works on:

  ```mysql
  SELECT pname FROM proj WHERE pno IN
  	(SELECT pno FROM workson WHERE eno =
  		(SELECT eno FROM emp WHERE ename = 'J. Doe'))
  ```

# EXISTS Example

----

- The EXISTS function is used to check whether the result of a nested query is empty or not

  - <span style='color:yellow'>EXISTS returns true if the nested query has 1 or more tuples</span>

- Example: Return all employees who have the same name as someone else in the company

  ```mysql
  SELECT ename
  FROM emp as E
  WHERE EXISTS (SELECT * FROM emp as E2
  							WHERE E.ename = E2.ename AND E.eno <> E2.eno)
  ```

# ANY and ALL example

----

- ANY means that any value returned by the subquery can satisfy the

  condition

- ALL means that all values returned by the subquery must satisfy the condition

- Example: Return the employees who make more than all the employees with title 'ME' make

  ```mysql
  SELECT ename
  FROM emp as E
  WHERE salary > ALL (SELECT salary FROM emp
  									WHERE title = 'ME')
  ```

# Subquery Syntax Rules

----

- 1) The ORDER BY clause may not be used in a subquery.
- 2) The number of attributes in the SELECT clause in the subquery must match the number of attributes compared to with the comparison operator.
- 3) Column names in a subquery refer to the table name in the FROM clause of the subquery by default. You must use aliasing if you want to access a table that is present in both the inner and outer queries

# Correlated Subqueries

----

- Most queries involving subqueries can be rewritten so that a subquery is not needed
  - <span style='color:orange'>This is normally beneficial because query optimizers may not do a good job at optimizing queries containing subqueries</span>
- A nested query is <span style='color:green'>correlated</span> with the outside query if it must be re- computed for every tuple produced by the outside query. Otherwise, it is <span style='color:green'>uncorrelated</span>, and the nested query can be converted to a non- nested query using joins
- A nested query is correlated with the outer query if it contains a reference to an attribute in the outer query

# Correlated Subquery Example

----

- Return all employees who have the same name as another employee:

  ```mysql
  SELECT ename
  FROM emp as E
  WHERE EXISTS (SELECT eno FROM emp as E2
  						WHERE E.ename = E2.ename AND E.eno <> E2.eno)
  ```

- A more efficient solution with joins:

  ```mysql
  SELECT E.ename
  FROM empasEJOINempasE2ON
            E.ename = E2.ename AND E.eno <> E2.eno
  ```

# Types of Joins

----

- A <span style='color:green'>equijoin</span> only contains the equality operator (=)
  - <span style='color:yellow'>e.g. workson JOIN Proj ON workson.pno=proj.pno</span>
- A <span style='color:green'>natural join</span> is equijoin of two tables with commonly named fields
  - <span style='color:yellow'>Removes the “extra copies” of the join attributes</span>
  - <span style='color:yellow'>The attributes must have the same name in both relations</span>
  - <span style='color:yellow'>e.g.workson NATURAL JOIN Proj</span>
- <span style='color:green'>Left outer join</span> – contains all tuples of first table even if no match
- <span style='color:green'>Right outer join</span> – contains all tuples of second table even if no match
- <span style='color:green'>Full outer join</span> – contains all tuples of either table even if no match. For a tuple that does not have a match, missing fields are NULL

# Specifying Outer Joins in SQL

----

- Types: NATURAL JOIN, FULL OUTER JOIN, LEFT OUTER JOIN, RIGHT OUTER JOIN, INNER JOIN, JOIN

  - <span style='color:orange'>he keyword "outer" can be omitted for outer joins. Same with "inner"</span>

- Example: Return all departments (even those without projects) and their projects

  ```mysql
  SELECT dname, pname
  FROM dept LEFT OUTER JOIN proj ON dept.dno = proj.dno
  SELECT dname, pname
  FROM dept LEFT OUTER JOIN proj USING (dno)
  SELECT dname, pname
  FROM dept NATURAL LEFT JOIN proj
  ```

# Subqueries in FROM Clause

----

- Subqueries are used in the FROM clause to produce temporary table

  results for use in the current query

- Example: Return the departments that have an employee that makes more than $40,000.

  ```mysql
  SELECT dname
  FROM Dept D, (SELECT ename, dno FROM Emp
  							WHERE salary > 40000) E 
  WHERE D.dno = E.dno
  ```

# SQL Queries using SELECT

----

- A query in SQL has the form:

  ```mysql
  SELECT (list of columns or expressions) FROM (list of tables)
  WHERE (filter conditions)
  GROUP BY (columns)
  HAVING (group filter conditions) ORDER BY (columns)
  LIMIT (count) OFFSET (start)
  ```

# Conclusion

----

- <span style='color:green'>SQL</span> is the standard language for querying relational databases
- The <span style='color:green'>SELECT</span> statement is used to query data and combines the relational algebra operations of selection, projection, and join into one statement
  - <span style='color:yellow'>There are often many ways to specify the same query</span>
- Queries may involve aggregate functions, outer joins, and subqueries

