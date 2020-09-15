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
  - <span style='color:yellow'>The predicate uses operands that are attributes or constants/expressions, comparison operators</span> $(\lt,\gt,=,\ne,\le,\ge)$ <span style='color:yellow'>, and logical operators (AND, OR, NOT)</span>
- <span style='color:RED'>NOTE: Can only filter tuples (rows) NOT attributes (columns)</span>

# Projection Operator

---

- The <span style='color:green'>projection operation</span> takes a relation as input and returns a new relation as output that contains a subset of the attributes of the input relation and all non-duplicate tuples
  - <span style='color:yellow'>The output relation has the same number of tuples as the input relation unless removing the attributes caused duplicates to be present</span>
  - <span style='color:yellow'>Question: When are we guaranteed to never have duplicates when performing a projection operation?</span> 
- Projection on relation $R$ with output attributes $A_1,…,A_m$ is denoted by $\Pi_{A_1,…,A_m}(R)$
  - <span style='color:yellow'>Order of </span>$A_1,...,A_m$ <span style='color:yellow'> is significant in the result</span>
  - <span style='color:yellow'>Cardinality of </span> $\Pi_{A_1,...,A_m}(R)$ <span style='color:yellow'>may not be the same as R due to duplicate removal</span>
- <span style='color:RED'>NOTE: Can only filter  attributes (columns) NOT tuples (rows)</span>

# Union

-----

- <span style='color:green'>Union</span> takes two relations R and S as input and produces an output relation that includes all tuples that are either in R, or in S, or in both R and S. Duplicate tuples are eliminated. Syntax: $R \cup S$ 
- $R$ and $S$ <span style='color:red'>MUST</span> be <span style='color:green'>union-compatible:</span>
  - <span style='color:yellow'>Both relations have the same number of attributes </span>
  - <span style='color:yellow'>Each attribute pair, </span>$R_i$ and $S_i$, <span style='color:yellow'>have compatible data types for all attribute indices </span>$i$
  - <span style='color:yellow'>Note that attributes do note need to have the same name</span>
  - <span style='color:yellow'>Result has attribute names of first relation</span>

# Set Difference

___

- <span style='color:green'>Set difference</span> takes two relations R and S as input and produces an output relation that contains all the tuples of R that are not in S

- Syntax: $R – S$

- <span style='color:red'>NOTE</span>:
  - $R-S \neq S-R$
  - $R$ and $S$ must be union compatible

# Intersection

---

- <span style='color:green'>Intersection </span>take two relations $R$ and $S$ as input and produces an output relation which contains all tuples that are both $R$ and $S$
  - <span style='color:yellow'>R and S must be union-compatiable</span>
- Syntax: $R \cap S$ 
- Note that $R \cap S = R - (R-S) = S - (S - R)$

# Cartesian Product

---

- <span style='color:green'>Cartesian product </span>of relations $R$ (of degree $k_1$) and S (of degree $k_2$) is a relation of degree ($k_1+k_2$ that consists of all ($k_1+k_2$)-tuples where each tuple is a concatenation of one tuple of R with one tuple of S
- Syntax: $R \times S$
- The cardianlity of $R \times S$ is $|R|\times|S|$
- The Cartesian product is also known as <span style='color:green'>cross product</span>

# $\theta$-Join

---

- Theta ($\theta$) join is a derivative of the Cartesian product. Instead of taking all combinations of tuples from $R$ and $S$, we only take a subset of those tuples that match a given condition $F$
- Syntax: $R \bowtie_{F} S$
- Note that $R \bowtie_{F} S = \sigma_{F}(R \times S)$

#Types of Joins

----

- The $\theta$-Join is a general join that allows any expression for condition F. However, there are more specialized joins that are frequentyl used
- An <span style='color:green'>equijoin</span> only contains the equality operator (=) in formula $F$
  - <span style='color:yellow'>e.g.</span> WorksOn $\bowtie_{\text{WorksOn.pno = Proj.pno}}$Proj
- A <span style='color:green'>natural join</span> over two relations $R$ and $S$ denoted by $R \bowtie S$ is the equijoin of $R$ and $S$ over a set of attributes common to both $R$ and $S$
  - <span style='color:yellow'>It removes the "extra copies" of the join attributes</span>
  - <span style='color:yellow'>The attrivutes must have the same name in both relations</span>

# Outer Joins

---

- Outer joins are used in cases where performing a join "loses" some tuples of the relations. These are called dangling tuples
- There are three types of outer joins:
  - <span style='color:yellow'>1) </span><span style='color:green'>Left outer join</span> <span style='color:yellow'>-</span> $R \sqsupset\bowtie S$ <span style='color:yellow'> - The output contains all tuples of R that match with tuples of S. If there is a tuple in R that matches with no tuple in S, the tuple is included in the final result and is padded with nulls for the attributes of S</span>
  - <span style='color:yellow'>2) </span><span style='color:green'>Right outer join</span><span style='color:yellow'> - </span> $ R \bowtie\sqsubset S$<span style='color:yellow'> - The output contains all tuples of S that match with the tuples of R. If there is a tuple in S that matches with no tuple in R, the tuple is included in the final result and is padded with nulls for the attributes of R</span>
  - <span style='color:yellow'>3) </span><span style='color:green'>Full outer join</span> <span style='color:yellow'> - </span> $R \sqsupset\bowtie\sqsubset S$ <span style='color:yellow'> - All tuples of R and S are included in the result whether or not they have matching tuple in the other relation.</span>

# Semi-Join and Anti-Join

---

- A <span style='color:green'>semi-join</span> between tables returns rows from the first table where one or more matches are found in the second table
  - <span style='color:yellow'>Semi-joins are used in EXISTS and IN constructs in SQL</span>
- An <span style='color:green'>anti-join</span> between two tables returns rows from the first table where <span style='color:green'>no</span> matches are found in the second table
  - <span style='color:yellow'>Anti-joins are used with NOT EXISTS, NOT IN and FOR ALL</span>
  - <span style='color:yellow'>Anti-join is the complement of semi-join: </span>$R \triangleright S = R \propto S$

# Objectives

- Given a relational schema and instance be able to translate english queries into relational algebra and show the resulting relation

