Where Relational **Algebra** defines **operations** and **sequences** on relations, Relational **Calculus** defines **predicates** and **propositions**

**Predicates**:
- The **meaning** of a sentence
- **Declarative** sentence - a **statement**
- Truth value functions - either **true** or **false**
Declarative statements **specify**:
- **What** to retrieve
- Not **how** to retrieve it

**Examples**:

![[Pasted image 20231113131436.png]]
#### Predicates and Propositions

![[Pasted image 20231113131728.png]]

**Intension** of a predicate:
- The predicates **meaning**
- Intension implies **properties** or **qualities** associated with the statement
**Extension**:
- The **set** of **all instantiations** for which the predicate holds `true`

Could **remember** as:
- **Intension** is more **intense** (specific)
- **Extensions** is more **extensive** (generic, wide-ranging)
### Predicate logic (first order logic)
Predicates are declarative statements about **entities**
- **Entities**:
	- Can be **referred** to with **variables**
	- Variables can be **instantiated**
- Statements can be **composed** of **other statements**
	- Combined with **logical operators** (AND/OR/NOT)
- Variables can be **quantified**
	- **Universal** quantification:
		- **Every** student...
	- **Existential** quantification:
		- **There exists** a student...


**Logical Operators**: *conjunction*, *disjunction* and *negation*
- Student s is on Course c AND Course c is called n 
- x is y ∨ x is z 
- x is ¬y 
**Conditionals**: *implication*, *only if*, *if and only if* 
- If it is Day w, Student s has a Lecture l 
- Student s has a Lecture l only if it is day w 
- Student s has a Lecture l if and only if it is Day w
### Relating this to the relational model
- **Predicate** ≈ **Schema** 
	- Predicate defines Parameters for a Truth valued function 
	- Schema defines attributes/domains for a valid data record 
- **Proposition** ≈ **Tuple** 
	- Proposition is a set of values for the parameters of a predicate 
	- Tuple is a set of values which we hold to be true for the schema
- **Extension** ≈ **Relation of a Schema** a.k.a **relation state** 
	- Set of all instantiations that are true 
	- Technically, any combination of tuples from the set of cross-domain permutations (that satisfies constraints) is a valid relation 
	- Current relation state is the set of tuples that represent truth valued propositions 
		- These form the facts, defining truth
**Closed world assumption:**
- What is **not** currently **known** to be **true** is **assumed** to be **false**
#### Example

![[Pasted image 20231113133151.png]]


## Relational Calculus
In addition to defining what are valid DB tables and the ground truth for relational model 
- RC can be used to express queries 
	- In a declarative manner 
Two Types of RC: 
- Tuple Relational Calculus
- Domain Relational Calculus
### Tuple Relational Calculus
#### Queries

![[Pasted image 20231113133646.png]]

![[Pasted image 20231113133628.png]]

![[Pasted image 20231113133635.png]]

**Example**:

![[Pasted image 20231113134033.png]]

![[Pasted image 20231113134259.png]]

![[Pasted image 20231113134306.png]]


#### Quantification: Existential and Universal

![[Pasted image 20231113134550.png]]



Unfinished from slide 35 in Slides..

