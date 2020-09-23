### Additional markdown resources:

<span style='color:'></span>

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
  

# Duplicates in SQL - DISTINCT clause

----

- To remove duplicates, use <span style='color:green'>DISTINCT</span> clause in the SQL statement:

  ```mysql
  SELECT DISTINCT title
  FROM emp
  ```



# Retrieving Only Some of the Rows

---

- The <span style='color:green'>selection operation</span> creates a new table with some of the rows of the input table

- A condition specifies which rows are in the new table

- The condition is similar to an **if** statement

  - Example:

  ```mysql
  SELECT pno, pname, budget, dno
  FROM proj
  WHERE dno = 'D2'
  ```

# Selection Conditions

---

- The condition in a selection statement specifies which rows are included
- It has a general form of an **if** statment
- The conditions may consist of attributes, constatnts, comparison operators $(\lt,\gt,=, !=, <=, >=)$ and logical operators $(\text{AND, OR, NOT})$

# Join Details and Examples

---

- Listing multiple tables in the $\text{FROM}$ clause separated by commas creates a cross product of tables

- Must specify $\text{JOIN}$ and $\text{ON}$ or provide join condition in $\text{WHERE}$ clause

  - Examples:

    - <span style='color:green'>Wrong! Cross Product</span>

      ```mysql
      SELECT ename, dname
      FROM emp, dept
      ```

    - <span style='color:green'>Correct! JOIN-ON Clause</span>

      ```mysql
      SELECT ename, dname
      FROM emp JOIN dept
      	ON emp.dno = dept.dno
      ```

    - <span style='color:green'>Correct! Join in WHERE</span>

      ```mysql
      SELECT ename, dname
      FROM emp, dept
      WHERE emp.dno = dept.dno
      ```

    - <span style='color:green'>Correct! Order does not matter</span>

      ```mysql
      SELECT ename, dname
      FROM dept JOIN emp
      	ON emp.dno = dept.dno
      ```

# Three Table Join Query Example

----

- Return all projects who have an employee working on the mwhose title is 'EE':

  ```mysql
  SELECT pname
  FROM emp JOIN workson ON emp.eno = workson.eno
  				JOIN proj ON workson.pno = proj.pno
  WHERE emp.title = 'EE'
  ```

  OR:

  ```mysql
  SELECT pname
  FROM emp, proj, workson
  WHERE emp.title = 'EE' and workson.eno = emp.eno 
  			and workson.pno = proj.pno
  ```

# Ordering Result Data

---

- The query result returned is not ordered on any column by default

- We can order the data by using the <span style='color:green'>ORDER BY</span> clause:

  ```mysql
  SELECT ename, salary, bdate
  FROM emp
  WHERE salary > 30000
  ORDER BY salary DESC, ename ASC;
  ```

- <span style='color:yellow'>'ASC' sorts the data in ascending order, and 'DESC' sorts it in descending order</span>

  - <span style='color:yellow'>The default is 'ASC'</span>

- <span style='color:yellow'>The order of sorted attributes is significant</span>

  - <span style='color:yellow'> The first column specified is sorted on first, then the second column is used to break any ties, etc.</span>

# LIMIT and OFFSET

---

- If you only want the first *N* rows, use a <span style='color:green'>LIMIT</span> clause:

  ```mysql
  SELECT ename, salary
  FROM emp
  ORDER BY salary DESC LIMIT 5
  ```

- To start from a row besides the first one, use <span style='color:green'>OFFSET</span>:

  ```mysql
  SELECT eno, salary
  FROM emp
  ORDER BY eno DESC
  LIMIT 3 OFFSET 2
  ```

- <span style='color:yellow'>LIMIT improves performance by reducing amount of data processed and sent by the databse system</span>

- <span style='color:yellow'>OFFSET 0 is the first rwo, so OFFSET 2 would return the 3rd row</span>

- <span style='color:yellow'>LIMIT/OFFSET syntax support differs between databases</span>

# Calculated Fields

----

- Expressions are allowed in SELECT clause to perform calculations

  - <span style='color:yellow'>When an expression is used to define an attribute, the DBMS gives the attribute a unique name such as col1, col2, etc.</span>

- Example: Return how much employeee 'A. Lee' will get paid for his work on each project

  ```mysql
  SELECT ename, pname, salary/52/5/8*hours
  FROM emp JOIN workson ON emp.eno=workson.eno 
  	JOIN proj ON workson.peno=proj.pno
  WHERE ename = 'A. Lee'
  ```

- <span style='color:green'>Results:</span>

  | ename  | pname       | col3   |
  | ------ | ----------- | ------ |
  | A. Lee | Budget      | 192.31 |
  | A. Lee | Maintenance | 923.08 |



# Renaming and Aliasing

---

- Often it is useful to rename an attribute in the final result (especially when using calculated fields)

- Renaming is accomplished using the keyword <span style='color:green'>AS</span>:

  ```mysql
  SELECT ename, pname, salary/52/5/8*hours AS pay
  FROM emp JOIN workson ON emp.eno=workson.eno 
  	JOIN proj ON workson.peno=proj.pno
  WHERE ename = 'A. Lee'
  ```

- <span style='color:green'>Results:</span>

  - | ename  | pname       | pay    |
    | ------ | ----------- | ------ |
    | A. Lee | Budget      | 192.31 |
    | A. Lee | Maintenance | 923.08 |

  

#Renaming and Aliasing Tables

----

- Renaming is also used when two or more copies of the smae table are in a query

- Using <span style='color:green'>aliases</span> allows you to uniquely identify what table you are talking about

  - Example: Reurn the employees and their managers where the managers make less than the employee

    ```mysql
    SELECT E.ename, M.ename
    FROM emp as E JOIN emp as M ON E.supereno = M.eno
    WHERE E.salary > M.salary
    ```

    

# Advanced Conditions - BETWEEN

---

- Used when the condition in the WHERE clause will request tuples where one attribute value must be in a *range* of values

  - Example: Return the employees who make at least $\$$20,000 and less than $\$$45,000

    ```mysql
    SELECT ename
    FROM emp
    WHERE salary >= 20000 and salary <= 45000
    ```

    We can use the keyword <span style='color:green'>BETWEEN</span> instead:

    ```mysql
    SELECT ename
    FROM emp
    WHERE salary BETWEEN 20000 and 45000
    ```

# Advanced Conditions - LIKE

---

- For strings, the <span style='color:green'>LIKE</span> operator is used to search for partial matches

  - <span style='color:yellow'>Partial string matches are specified by using either "<span style='color:green'>%</span>" that replaces zero or more characters or underscore "<span style='color:green'>_</span>" that replaces a single character.</span>

  - Example: Return all employee names that start with 'A'.

    ```mysql
    SELECT ename
    FROM emp
    WHERE ename LIKE 'A%'
    ```

  - Example: Return all employee names who have a first name that starts with 'J' and whose last name is 3 characters long

    ```mysql
    SELECT ename
    FROM emp
    WHERE ename LIKE 'J. _ _ _'
    ```

- <span style='color:green'>Warning</span>: Do not use the LIKE operator if you do not have to

  - It is often an inefficient operation as the DBMS may not be able to optimize lookup using LIKE as it can for equal (=) comparisons
  - The result is the DBMS often has to examine ALL TUPLES in the relation.
  - In almost all cases, adding indexes will not increase the performance of LIKE queries because the indexes cannot be used
    - <span style='color:yellow'>Most indexes are implemented using B-trees that allow for fast equality searching and efficient range searches</span> 

# Advanced Conditions - IN

---

- To specify that an attribute value should be in a given set of values, the IN keyword is used

  - Example: Return employees who are in one of the departments {'D1', 'D2', 'D3'}

    ```mysql
    SELECT ename
    FROM emp
    WHERE dno IN ('D1','D2','D3')
    ```

  - Note that this is equivalent to using OR:

    ```mysql
    SELECT ename
    FROM emp
    WHERE dno = 'D1' OR dno = 'D2' OR dno = 'D3'
    ```

  - We will see more uses of IN and NOT IN with nested subqueries

# Advanced Conditions - NULL

---

- Remember NULL indicates that an attribute does not have a value. To determine if an attribute is NULL, we use the clause <span style='color:green'>IS NULL</span>

  - <span style='color:yellow'>Note that you should not test NULL values using = and <></span>

  - Example: Return all employees who are not in a department

    ```mysql
    SELECT ename
    FROM emp
    WHERE dno IS NULL
    ```

    

  - Example: Return all departments that have a manager.

    ```mysql
    SELECT dname
    FROM dept
    WHERE mgreno IS NOT NULL
    ```

    

  

# Set Operations

---

- Union, intersection, and difference combine results of two queries

  - <span style='color:yellow'>UNION , INTERSECT, EXCEPT, UNION ALL (returns all rows)</span>

  - <span style='color:green'>Union-compatible: </span><span style='color:yellow'>same # of attributes and compatible data types. Do not need to have the same name</span>

  - Example: Return the employees who are either directly supervised by 'R. Davis' or directly supervised by 'M. Smith'

    ```mysql
    (SELECT E.ename
    FROM emp as E JOIN emp as M ON E.supereno = M.eno
    WHERE M.ename='R. Davis')
    UNION
    (SELECT E.ename
    FROM emp as E JOIN emp as M ON E.supereno = M.eno
    WHERE M.ename='M. Smith')
    ```

# SELECT INTO

---

- The result of a select statement can be stored in a temporary table using the <span style='color:green'>INTO</span> keyword

  ```mysql
  SELECT E.ename
  INTO davisMgr
  FROM emp as E JOIN emp as M ON E.supereno = M.eno
  WHERE M.ename = 'R. Davis'
  ```

  