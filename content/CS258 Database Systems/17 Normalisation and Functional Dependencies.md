## Relational DB Design
- Idea is to have some understanding of the **principles** that can guide us to coming up with a **good relational model**

Mostly, focusing on how to **define** the **right tables**
- How to **group attributes together** to form tables
- How to be **sure** a grouping has **desirable properties**

Hence, we need a way to know when a design is:
- **Good** (and **how** good)
- And what **pitfalls** exist
### Guidelines of good RB design
#### 1: Understandable
The **schema** and the **attributes** within each schema should **make sense** and be easy to **understand**
- Each **tuple** in a relation should represent one **entity** or **relationship instance**
- **Attributes** of different **entities** should not be in the same table
- Referring to **other** entities should **only** occur using **foreign keys**
#### 2: Redundancy
**Redundancy** of tuples information is **harmful** and must be avoided
Two main reasons:
- **Storage costs**
	- Replicating info in various tuples **wastes** storage space
	- **Consistency** costs and potential **anomalies**
		- Replicas must be kept consistent during **updates**
			- **Updates** of tuples
			- **Insertion** of tuples
			- **Deletion** of tuples
		- If updates are **not** carried out on all replicas, then **anomalies** can occur
		- Anomalies can **also** occur as **deletions** can cause unnecessary **loss** of information in the DB

##### Examples:

![[Pasted image 20231116161843.png]]

![[Pasted image 20231116161853.png]]

![[Pasted image 20231116161913.png]]

![[Pasted image 20231116161924.png]]
#### 3: NULL values
`NULL` values within tuples are **harmful** and must be avoided
- Waste **storage space**
- Understanding selections, joins, aggregate semantics are difficult

Attributes whose value in a relation is expected to be **frequently `NUL`** should **not** be part of that schema
- Another relation schema should be created
#### Decompositions
- To avoid aforementioned anomalies, a bad relational schema will have to be **decomposed** into smaller relation schemas
- Given such a decomposition, the ability to **join** decomposed relations to collect all original information is vital
- But if a decomposition is done poorly, the joining of the decomposed relations can create problems in the form of previously non-existent **spurious tuples**
#### 4: Spurious tuples
Design relations to **avoid** spurious tuples

![[Pasted image 20231116163457.png]]

Relations to be joined should have **keys** as **join attributes** (primary/foreign key combinations)

![[Pasted image 20231116163537.png]]


## Functional Dependencies (FDs)
FDs are used to specify **formal measures** of the "goodness" of relational designs 
- Will be used to define **keys** which are central 
	- Used to define **normal forms** for relations 
- Then we can prove **how good** a relational schema design is
### What are FDs?
- **Constraints** that are derived from the **meaning** and **interrelationships** of the data attributes of a relation schema $R$
	- Constraints on the **possible values** different attributes may have as they **depend** on other attributes
- Given a set of attributes $X$ and a set of attributes $Y$, from a relation schema $R$, we say that $X$ **functionally determines** $Y$
	- Denoted as $X\rightarrow Y$
	- Indicating that the **values** of the $Y$ component of a tuple of $r(R)$ **depend on** (or are **uniquely determined** by) the values of the $X$ component of the tuple

![[Pasted image 20231116164457.png]]

![[Pasted image 20231116164511.png]]
### Depicting FDs

![[Pasted image 20231120131000.png]]

- *SSN* determines *Ename*, *Bdate*, *Address*, *Dnumber*
- *Dnumber* determines *Dname*, *Dmngr_ssn*

![[Pasted image 20231120131733.png]]

![[Pasted image 20231120131740.png]]
### Overview

![[Pasted image 20231120131808.png]]

**Proving/disproving FDs by inspection**

![[Pasted image 20231120131849.png]]

- Functional dependencies are determined for the **schema**, not the specific instance of the table
## Relational DB Design and Normalisation
A DB design **determines** how attributes are grouped into relations (tables)
- A **good** DB design must:
	- Make it **easier** for users to **understand** and **retrieve** information
	- **Avoid** storing (and needing to maintain) **redundant information**
		- Discovering and avoiding redundancy fundamentally depends on the notions of **functional dependencies**
		- FDs **specify** which attributes can become **keys**

### Normalisation theory
- A way to ensure the above desired data
- Based on the observation that relations with certain properties are more effective in **inserting, updating** and **deleting** data than other sets of relations containing the same data
**Process** is:
- Starting from a 'universal' all-attribute-listing relation, progressively **remove** redundant data from your tables
	- And thus **avoid anomalies**
- In the relational model, methods exist for **quantifying** how efficient a database design is
	- Called **normal forms (NF)**
	- There are algorithms for converting a given database from one NF to another
- Normalisation generally involves **decomposing** existing tables into multiple ones
	- Must be **re-joined** each time a query is issued, so that no information is **lose**
- Fundamental concepts are:
	- **FDs** and **keys**
	- **Lossless** joins
	- **Dependency preservation**
#### Normal Forms

![[Pasted image 20231120132954.png]]
#### Keys and FDs

![[Pasted image 20231120133318.png]]

![[Pasted image 20231120133323.png]]
### Decomposing Relations
Normalisation progressively breaks large relation schemas into smaller ones
- However, not all decompositions are good
- **Spurious tuples**
	- Extra rows in a table that occur as a result of joining two tables in the wrong manner

The following properties are important:
- **Lossless** or **non-additive** decomposition
	- **Critical**
- **Dependency preservation** property
	- **Desirable**
#### Lossless or Non-additive join decomposition

![[Pasted image 20231120133609.png]]

![[Pasted image 20231120133616.png]]

![[Pasted image 20231120133621.png]]

![[Pasted image 20231120134903.png]]
### Implied FDs
Given a relation schema $R$, the closer (cover) $F*$ is a set of FDs, $F:$
- $F*$ represents the set of all **implied** FDs from $F$

![[Pasted image 20231120135014.png]]

![[Pasted image 20231121091457.png]]
#### Proofs:
![[Pasted image 20231123165321.png]]

![[Pasted image 20231123165329.png]]
#### Test for Non-additive Join Decompositions

![[Pasted image 20231121092642.png]]

For first one: $B\rightarrow A$, which isn't true
For second one: $B\rightarrow C$, also not true
So not lossless
### Dependency Preservation
**Intuition**: Say $R$ is given with a set of FDs
1. If $R$ is decomposed into $R_{1}, R_{2}$ and $R_3$, and
2. We enforce the FDs that hold individually on $R_1$, on $R_2$ and on $R_3$
3. Then do all original FDs holds?
Helps **prevent** checking updates via computing **joins**
- This would be **expensive** otherwise
#### Examples

![[Pasted image 20231121093239.png]]

It is lossless as Postcode $\rightarrow$ city (which is $R1-R2$)
Not dependency preserving as can't check if city and street&no will determine postcode with joining

![[Pasted image 20231121094130.png]]

Yes - lossless-join, as $B\rightarrow C$
## Normalisation
Multi-step process **beginning** with an '**unnormalized**' relation
**Goal**: Decompose relations by
- Reducing **redundancy/avoid anomalies**
- While preserving **dependencies**
- In a **lossless-join** manner
### Normal Form (NF) definitions
- **1NF**: All attributes are **atomic** and there is a **key**
	- "The **key**"
- **2NF**: Non-key attributes must be **dependent** on the **full key** (actually any candidate key)
	- "The **whole key**"
- **3NF**: Non-key attributes must **only depend** on the key (actually any candidate key)
	- "**Nothing but** the key"
**BCNF**: A relation is in Boyce-Codd Normal Form if **every determinant** is a **superkey**
#### Unnormalized relations
- **First step** is to **convert** the data into a **two-dimensional table**
- Relations are **decomposed** to normalise
- Functional Dependencies must **not** be **lost**
- In unnormalized relations data can **repeat** within a column
### First Normal Form (1NF)
To move to First Normal Form a relation must:
- Contain attributes with **atomic values**
	- Have no **repeating groups**
	- i.e. attributes can **not** have a substructure
		- **Multi-valued attributes** 
			- (e.g., phone: cell, landline, …)
		- **Composite attributes** 
			- (e.g., address: streetname, number, zipcode, city ..)
	- Formally, the domains of attributes must be **atomic**
	- This **simplifies** attributes
		- Queries become **easier**
- Define a **key**
Relational model definition these days **assumes atomicity** of **values**
##### Example

**Unnormalized hospital relation**:

![[Pasted image 20231121095117.png]]

**Ensure value atomicity**: (each attribute has only 1 value associated)

![[Pasted image 20231121095203.png]]

**Define** **key** and **FDs**

![[Pasted image 20231121095424.png]]
#### 1NF Storage Anomalies
- **Insertion**:
	- A new patient has **not yet** undergone surgery 
		- Hence **no** surgeon #
		- Since surgeon # is part of the **key** we **can't insert**
- **Update**:
	- If a patient comes in for a **new procedure**, and has **moved**, we need to change **multiple** address entries
- **Deletion**
	- Type 1:
		- Deleting a patient record may **also** delete **all info** about a **surgeon**
	- Type 2:
		- When there are **functional dependencies** (like side effects and drug) **removing** one item can **eliminatae** other information
	
![[Pasted image 20231123161655.png]]
### Second Normal Form
In 2NF if:
- It is in 1NF **and**
- **Every** non-key attribute is **fully functionally dependent** on any key
2NF improves **data integrity**
- **Prevents** update, insert and delete **anomalies**

![[Pasted image 20231123161940.png]]
##### Example

![[Pasted image 20231123162001.png]]

![[Pasted image 20231123162357.png]]

![[Pasted image 20231123162303.png]]

![[Pasted image 20231123162311.png]]

![[Pasted image 20231123162911.png]]

![[Pasted image 20231123162919.png]]
#### 2NF Storage anomalies

![[Pasted image 20231123162925.png]]
### Third Normal Form (3NF)
In 3NF if:
- In 2NF and
- There is **no transitive functional dependency** from a **key** to a **non-key** attribute
	- When one non-key attribute **determines** another non-key attribute this leads to a **transitive functional dependency**

![[Pasted image 20231123163103.png]]

The `SideEffects` column in the Surgery table is determined by the drug administered.
- There exists a Transitive Functional Dependency
	- (Key -> Drug -> SideEffect)
- Surgery is not 3NF

![[Pasted image 20231123163204.png]]

![[Pasted image 20231123163210.png]]

![[Pasted image 20231123163216.png]]
#### 2NF storage anomalies removed

![[Pasted image 20231123163254.png]]
### Boyce-Codd Normal Form
In BCNF if:
- Whenever an FD $X\rightarrow A$ holds in $R$, $X$ is a **superkey** of $R$
**Most** 3NF relations are **also** BCNF relations
A 3NF relation is **not** in BCNF if:
- There is a determinant attribute that is **not** a superkey in a non-trivial FD
- **Key difference** from 3NF is that the RHS of an FD must not be a prime (key) attribute, unless FD is trivial

![[Pasted image 20231123163712.png]]

![[Pasted image 20231123163722.png]]

![[Pasted image 20231123163918.png]]

![[Pasted image 20231123164153.png]]

### Normalising to what costs?

![[Pasted image 20231123164704.png]]

