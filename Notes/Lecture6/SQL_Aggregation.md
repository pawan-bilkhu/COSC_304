### Additional markdown resources:

<span style='color:'></span>

# Aggregate Functions

---

- Five common aggregate functions are:
  - <span style='color:yellow'>COUNT - returns the # of values in a column</span>
  - <span style='color:yellow'>SUM - returns the sum of the values in a column</span>
  - <span style='color:yellow'>AVG - returns the average of the values in a column</span>
  - <span style='color:yellow'>MIN - returns the smallest value in a column</span>
  - <span style='color:yellow'>MAX - returns the largest value in a column</span>
- Notes:
- <span style='color:yellow'>1) COUNT, MAX, and MIN apply to all types of fields, whereas SUM and AVG apply to only numeric fields</span>
- <span style='color:yellow'> 2) Except for COUNT(*) all functions ignore nulls. COUNT(*) returns the number of rows in the table</span>
- <span style='color:yellow'>3) Use DISTINCT to eliminate duplicates</span>

# Aggregate Function Example

---

- Return the number of employees and their average salary

  ```mysql
  SELECT COUNT(eno) AS numEmp, AVG(salary) AS avgSalary
  FROM emp
  ```

# GROUP BY Clause

---

- Aggregate functions are most useful when combined with the GROUP BY clause. The <span style='color:green'>GROUP BY</span> clause groups the **TUPLES** based on the values of the attributes specified
- When used in combination with aggregate functions, the result is a table where each tuple consists of unique values for the group by attributes and the result of the aggregate functions applied to the tuples of that group

# GROUP BY Example

----

- For each employee title, return the number of employees with that title, and the minimum, maximum, and average salary

  ```mysql
  SELECT title, COUNT(eno) AS numEmp,
  			MIN(salary) as minSal,
  			MAX(salary) as maxSal, AVG(salary) AS avgSal
  FROM emp
  GROUP BY title
  ```

# GROUP BY Clause Rules

----

- There are a few rules for using the GROUP BY clause:
  - <span style='color:yellow'>1) A column name cannot appear in the SELECT part of the query unless it is part of an aggregate function or in the list of group by attributes</span>
    - <span style='color:orange'>Note that the reverse is allowed: a column can be in the GROUP BY without being in the SELECT part</span>
  - <span style='color:yellow'>2) Any WHERE conditions are applied before the GROUP BY and aggregate functions are calculated</span>
  - <span style='color:yellow'>3) You can group by multiple attributes. To be in the same group, all attribute values must be the same </span>

# HAVING Clause

---

- The <span style='color:green'>HAVING</span> clause is apllied ***AFTER*** the GROUP BY clause and aggregate functions are calculated
- It is used to filter out entire *groups* that do not match certain criteria
- The <span style='color:green'>HAVING</span> clause can contain any  condiation that references aggregate functions and the group by attributes themselves
  - <span style='color:yellow'>However, any conditions on the GROUP BY attributes should be specified in the WHERE clause if possible due to performance reasons</span>

# HAVING Example

----

- Return the title and number of employees of that title where the number of employees of the title is at least 2

  ```mysql
  SELECT title, COUNT(eno) AS numEmp
  FROM emp
  GROUP BY title
  HAVING COUNT(eno) >= 2
  ```

- <span style='color:green'>Result:</span>

  | title | numEmp |
  | :---: | :----: |
  |  EE   |   2    |
  |  SA   |   3    |
  |  ME   |   2    |

# GROUP BY/HAVING Example

----

- For employees born after December 1, 1965, return the average salary by department where the average is > 40,000

  ```mysql
  SELECT dname, AVG(salary) AS avgSal
  FROM emp JOIN dept ON emp.dno = dept.dno
  WHERE emp.bdate > DATE '1965-12-01'
  GROUP BY dname
  HAVING AVG(salary) > 40000
  ```

  - Step #1: Preform Join and Filter in WHERE clause

    | eno  |   name   |   bdate    | title | salary | supereno | dno  |   dname    | mgreno |
    | :--: | :------: | :--------: | :---: | :----: | :------: | :--: | :--------: | :----: |
    |  E2  | M. Smith | 1966-06-04 |  SA   | 50000  |    E5    |  D3  | Accounting |   E5   |
    |  E3  |  A. Lee  | 1966-07-05 |  ME   | 40000  |    E7    |  D2  | Consulting |   E7   |
    |  E5  | B. Casey | 1971-12-25 |  SA   | 50000  |    E8    |  D3  | Accounting |   E5   |
    |  E7  | R. Davis | 1977-09-08 |  ME   | 40000  |    E8    |  D1  | Management |   E8   |
    |  E8  | J. Jones | 1972-10-11 |  SA   | 50000  |   null   |  D1  | Management |   E8   |

  - Step #2: GROUP BY on dname

    | eno  |   name   |   bdate    | title | salary | supereno | dno  |   dname    | mgreno |
    | :--: | :------: | :--------: | :---: | :----: | :------: | :--: | :--------: | :----: |
    |  E2  | M. Smith | 1966-06-04 |  SA   | 50000  |    E5    |  D3  | Accounting |   E5   |
    |  E5  | B. Casey | 1971-12-25 |  SA   | 50000  |    E8    |  D3  | Accounting |   E5   |
    |  E3  |  A. Lee  | 1966-07-05 |  ME   | 40000  |    E7    |  D2  | Consulting |   E7   |
    |  E7  | R. Davis | 1977-09-08 |  ME   | 40000  |    E8    |  D1  | Management |   E8   |
    |  E8  | J. Jones | 1972-10-11 |  SA   | 50000  |   null   |  D1  | Management |   E8   |

  - Step #3: Calculate aggregate functions

    |   dname    | avgSal |
    | :--------: | :----: |
    | Accounting | 50000  |
    | Consulting | 40000  |
    | Management | 45000  |

  - Step #4: Filter groups using HAVING clause

    |   dname    | avgSal |
    | :--------: | :----: |
    | Accounting | 50000  |
    | Management | 45000  |

# GROUP BY Examples

----

- Return the average budget per project:

  ```mysql
  SELECT AVG(budget) FROM proj
  ```

- Return the average # of hourse worked on each project:

  ```mysql
  SELECT pno, AVG(hours) FROM workson GROUP BY pno
  ```

- Return the departments that have projects with at least 2 'EE's working on them:

  ```mysql
  SELECT DISTINCT proj.dno
  FROM proj JOIN workson ON workson.pno = proj.pno
  					JOIN emp ON workson.eno=emp.eno
  WHERE emp.title = 'EE' GROUP BY proj.dno, proj.pno
  HAVING COUNT(*) >=2
  ```

# GROUP BY/HAVING Multi-Attribute Example

----

- Return the employee number, department number and hours the employee worked per department where the hours is >=10

  ````mysql
  SELECT W.eno, D.dno, SUM(hours)
  FROM workson AS W JOIN proj AS P ON W.pno = P.pno
  			JOIN dept AS D ON P.dno = D.dno
  GROUP BY W.eno, D.dno
  HAVING SUM(hours) >= 10
  ````

- Result:

  | eno  | dno  | SUM(hours) |
  | :--: | :--: | :--------: |
  |  E1  |  D1  |     12     |
  |  E2  |  D1  |     24     |
  |  E3  |  D2  |     48     |
  |  E3  |  D3  |     10     |
  |  E4  |  D2  |     18     |
  |  E5  |  D2  |     24     |
  |  E6  |  D2  |     48     |
  |  E7  |  D3  |     36     |

