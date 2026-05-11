Playground for SQL: [RelaX - relational algebra calculator (dbis-uibk.github.io)](https://dbis-uibk.github.io/relax/landing)

Based on **tables** which are also known as **relations**; which have known **names** and **attribute (name, type)** pairs
### Schema
The **Schema** of a **relation**: 
- Denoted by $R(A_1, A_2, … A_n)$
- Where $R$ is the **name** of the relation.
- And each $Ax$ is an **attribute**; attributes are the names of each column.
	- Each **attribute** has a **domain** of valid values
- A **tuple** is an ordered set of values, enclosed in angled brackets <>
- Each **value** is derived from the domain of its attribute
- A relation is a **set** of such tuples (rows)

So:
- **Schema** (of a relation) - Signifies the structure of the relation
- **Relation** - Contains the actual values of the data, stored in tuples
- **Tuples** - Ordered set of of values, enclosed in angled brackets 
##### Relation State
The **relation state** is the set of tuples currently in the relation
It is a **subset** of the Cartesian product of the **domains of its attributes**

##### All definitions
- **Database** - collection of relations
- **Relation Schema** - relation name and attribute list
- **Database schema** - set of all relation schemas in the database

![[Pasted image 20231003175615.png]]

- **Table**: Relation
- **Column Headers**: Attributes
- **Row**: Tuple
- **Relation instance (or state)**: A set of rows for a relation schema

**Relational model is set-oriented**
- Multiset of *attributes* in a *tuple*
- Multiset of *tuples* in a *relation*
- Multiset of *relations* in a *database*
#### Databases are also defined by *constraints*
- **Domain** constraint
	- Value of an attribute must be in the domain of the attribute
- **Key** constraints
	- **Superkey** - any valid state of $r(R)$ such that 
		- Formally: $\forall t_1 \forall t_2 \in r(R) . t_1[SK] \neq t_2[SK]$
		- Where $t_1$ and $t_2$ are tuples
	- **Candidate Key** - minimal superkey
		- **Minimal** - least amount of attributes needed in order for it to be a superkey
		- If multiple, one is arbitrarily chosen
- **Entity integrity** constraints
	- The **primary key attributes** $PK$ cannot be `NULL` in any tuple of $r(R)$
	- **Optionally**, any attribute of $R$ **may** **not** be allowed to be `NULL`
- **Referential Integrity** concerns cross-table relationships
	- A set of attributes $FK$ from relation $R1$ is a **foreign key** that **references** relation $R2$ if:
		- Attribute(s) in $FK$ from $R1$ have **same domain** as the attribute(s) in the primary key $PK$ of $R2$
		- A value of $FK$ in a tuple $t1$ of $R2$ must either:
		For tuples $t1$ in $R1$ and $t2$ in $R2$:
		$t1[FK] = t2[PK]$
		or
		$t1[FK]$ is `NULL`
 **All of these constraints are expressed by the `CREATE TABLE` statement in SQL**
### Possible violations for each operation
#### INSERT
- **Domain**
	- One of the attribute values for new tuple is **not in the attribute domain**
- **Key**
	- The value of a **key attribute** in new tuple **already exists**
- **Referential integrity**
	- A foreign key value in new tuple references a primary key value that does not exist in referenced relation
- **Entity integrity**
	- If primary key value is null in new tuple
#### DELETE
May violate **only referential integrity**
- If primary key value of the tuple being deleted is referenced from other tuples in other relations
- Some option must be specified during database design for each foreign key constraint on how to handle such deletions leading to referential integrity violations
#### UPDATE
- **Domain** and **Not `NULL`** constraints may be violated on attributed being modified
- Updating the **primary key**: Key constraint
	- New value may be a **duplicate**
- Updating a foreign key:
	- May violate **referential integrity**
- Updating an ordinary attribute
	- Can only violate **domain** constraints

