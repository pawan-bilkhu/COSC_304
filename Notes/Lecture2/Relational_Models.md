### Additional markdown resources:

<span style='color:'></span>

# The Relational Model: Terminology

----

- The <span style='color:green'>relational model</span> organizes data into tables called relations
- A  <span style='color:green'>relation</span> is a table with columns and rows
- An  <span style='color:green'>attribute</span> is a named column of a relation
- A  <span style='color:green'>tuple</span> is a row of a relation
- A  <span style='color:green'>domain</span> is a set of allowable values for one or more attributes
- The  <span style='color:green'>degree</span> of a relation is the number of attributes it contains
- The  <span style='color:green'>cardinality</span> of a relation is the number of tuples it contains.
- The  <span style='color:green'>intension</span> is the structure of the relation including its domains
- The  <span style='color:green'>extension</span> is the set of tuples currently in the relation

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
  - <span style='color:green'>Entity integrity constraint</span> - <span style='color:yellow'>No attribue of a primary key can be null</span>
  - <span style='color:green'>Referential integrity constraing</span> - <span style='color:yellow'>If a foreign key exists in a relation, then the foreign key value must match a primary key value of a tuple in the referenced relation or be null</span> 