### Additional markdown resources:

<span style='color:'></span>

# Relational Algebra Operators

___

|     OPERATOR      |  Symbol   |                     Purpose                     |
| :---------------: | :-------: | :---------------------------------------------: |
|     Selection     | $\sigma$  |                   Filter rows                   |
|    Projection     |   $\Pi$   |            Keep only certain columns            |
| Cartesian Product | $\times$  |     Combine two tables in all possible ways     |
|       Join        | $\bowtie$ |     Combine two tables based on a condition     |
|       Union       |  $\cup$   |        Keep rows in either of two tables        |
|    Difference     |     -     | Keep rows in first table that are not in second |
|   Intersection    |  $\cap$   |        Keep rows that are in both tables        |



# Selection Operation

---

- The <span style='color:green'>selection operator</span> takes a relation as input and returns a new relation as output that contains tuples that satisfy a condition called a <span style='color:green'>predicate</span>
  - <span style='color:yellow'>The predicate is similar to a condition in an <i>if</i> statement</span>
  - <span style='color:yellow'>The output relation has the same number of columns as the input relation, but may have less rows</span>
- Selection operation on relation $R$ with predicate $F$ is denoted by $\sigma_{F}(R)$
  - <span style='color:yellow'>The predicate uses oeprands that are attributes or constants/expressions, comparison operators $(\lt, \gt , = , \neq, \geq,\leq )$, and logical operators (AND, OR, NOT)</span>
- <span style='color:RED'>NOTE: Can only filter tuples (rows) NOT attributes (columns)</span>

# Projection Operator

---

- The <span style='color:green'>projection operation</span> takes a relation as input and returns a new relation as output that contains a subset of the attributes of the input relation and all non-duplicate tuples
  - <span style='color:yellow'>The output relation has the same number of tuples as the input relation unless removing the attributes caused duplicates to be present</span>
  - <span style='color:yellow'>Question: When are we guaranteed to never have duplicates when performing a projection operation?</span> 
- Projection on relation $R$ with output attributes $A_1,…,A_m$ is denoted by $\Pi_{A_1,…,A_m}(R)$
  - <span style='color:yellow'>Order of $A_1,…, A_m$ is significant in the result</span>
  - <span style='color:yellow'>Cardinality of $\Pi_{A_1,…,A_m} (R)$ may not be the same as $R$ due to duplicate removal</span>
- <span style='color:RED'>NOTE: Can only filter  attributes (columns) NOT tuples (rows)</span>

# Union

-----

- <span style='color:green'>Union</span> takes two relations R and S as input and produces an output relation that includes all tuples that are either in R, or in S, or in both R and S. Duplicate tuples are eliminated. Syntax: $R \cup S$ 
- $R$ and $S$ <span style='color:red'>MUST</span> be <span style='color:green'>union-compatible:</span>
  - <span style='color:yellow'>Both relations have the same number of attributes </span>
  - <span style='color:yellow'>Each attribute pair, $R_i$ and $S_i$, have compatible data types for all attribute indices i </span>
  - <span style='color:yellow'>Note that attributes do note need to have the same name</span>
  - <span style='color:yellow'>Result has attribute names of first relation</span>

# Set Difference

___

- <span style='color:green'>Set difference</span> takes two relations R and S as input and produces an output relation that contains all the tuples of R that are not in S

- Syntax: $R – S$

- <span style='color:red'>NOTE</span>:
  - $R-S \neq S-R$
  - $R$ and $S$ must be union compatible