**Relational algebra** is a **formal language** for DBs - As opposed to SQL 
- SQL is a '*practical*' language, developed **on top of** the ideas of **relational algebra**

What **should** a DB **do**?
- **Abstract thinking** for RDBs (Relational databases)
	- Define **key concepts** and **constructs**
	- Define **key operators** on the **concepts**
- Enter **relational algebra**
	- Key **operators** on the key RDB concept (relations) and their properties
## Relational Algebra
An '*Algebra*' defines:
- **Symbols** (variables)
- **Operators**
- **Properties** (of the operators)
**Operates** on *relations*

![[Pasted image 20231102161053.png]]

Relational Algebra is **procedural**
- RA expressions define **what** each operation **does**
- Shows **how** result emerges as **output** of **sequence of operators**
	- Like an SQL query (despite SQL being declarative)
### Key RA operators

![[Pasted image 20231102161621.png]]

#### **Projection** $\pi$
$\pi_\text{<selection condition>}(R)$
- Projection chooses **Columns**
- e.g. ![[Pasted image 20231102164819.png]]
##### Algebraic Properties
Returns proper sets (not multisets) - **no duplicates**
- ![[Pasted image 20231102164908.png]]
![[Pasted image 20231102164947.png]]

![[Pasted image 20231102165044.png]]
#### Selection $\sigma$
$\sigma_\text{<selection condition>}(R)$
- Selection chooses **Rows**

Essentially the `WHERE` part - `Project name, Select all rows where <condition>`
- **Selection condition** consists of clauses of the shape:
	- `<attributename> <comparison op> <constant value>`
	- `<attributename> <comparison op> <attribute name>`
- Where:
	- **Attribute name** is the name of an attribute of R 
	- **Comparison op** is **one of** {=, <, ≤, >, ≥, ≠}
	- **Constant value** is a value from the **attribute domain**
##### Algebraic properties
- **Commutativity**
	- ![[Pasted image 20231102164103.png]]
	- Demonstrating commutativity of $\sigma$
	- ![[Pasted image 20231102164253.png]]
- **Sequencing**
	- ![[Pasted image 20231102164111.png]]
	- Demonstrating Sequencing of $\sigma$
	- ![[Pasted image 20231102164137.png]]
#### **Product**/Cross Join $\times$
- aka `CROSS JOIN`
- Returns **Cartesian Product**
The result of is a relation $R(A_{1}, ..., A_{m})\times S(B_{1}, ..., B_{n})$ is a relation $Q(A_{1},..., A_{m}, B_{1}, ..., B_{n})$ 
- Such that each tuple in $Q$ is a **combination** of: 
	- An $m$-tuple from $R$ and 
	- An $n$-tuple from $S$

![[Pasted image 20231106132111.png]]

![[Pasted image 20231106132137.png]]

Generally use $\sigma$ after $\times$ to **eliminate meaningless rows** 
#### **Natural Join** $*$
- Specifically joins when both R and S have an **attribute in** **common**, where the attribute name is the same
Join attributes must have the same name 
- Join condition is **specified** **implicitly** 
- **Duplicate** columns are **omitted** 
- Often preceded by a rename, to make sure the tables have the appropriate matching headings

![[Pasted image 20231106133041.png]]

![[Pasted image 20231106133243.png]]

![[Pasted image 20231106133259.png]]
#### **Join** $\Join$
- Like natural join but can specify different attribute names

![[Pasted image 20231106132403.png]]

![[Pasted image 20231106132409.png]]

![[Pasted image 20231106132452.png]]
#### **Rename**
**Relation renaming**
- $\rho_S(R)$ 
	- $S$ is the **new relation name** of $R$
**Attribute renaming**
- $\rho_{(B_{1}, ..., B_{n})}(R)$
- $B_i$ are the **new attribute names** for $R$
**General renaming**
- $\rho_{S(B_{1}, ..., B_{n})}(R)$
- **Both** renaming **relation** and **attributes**

**Renaming** is useful for describing **complex** operations on relations in a **succinct** way

![[Pasted image 20231106131036.png]]
#### Division $\div$
$S\times T=R \rightarrow R\div T=S$

- Essentially, all the values of attributes in $T$ that are matched with all other values of the other attributes in $S$

![[Pasted image 20231109161217.png]]

![[Pasted image 20231109161355.png]]
##### An SQL approach to division

![[Pasted image 20231109161651.png]]

![[Pasted image 20231109161708.png]]

$T$ is the formula for division in SQL

![[Pasted image 20231109162407.png]]
##### More queries for division (examples)

![[Pasted image 20231109162801.png]]
#### Null
Technically, `null` isn't included in relational algebra, at least originally
- Design database in such a way that tables with non-null values exist
- Separate other tables with attributes that can contain null values as not needed in main table
### Examples for optimality

![[Pasted image 20231102162749.png]]

![[Pasted image 20231102162803.png]]

Order of operators gets **same result** but first is **faster** since not having to iterate through all values after **cartesian product**
- Saves cost on **both** the **selection** operator **and** the **join** operator
#### From Specification to SQL ta RA
**Specification** - For every project located in Stafford, list the project number, the controlling department number, department manager's last name, address and birthdate

```sql
SELECT Pnumber, Dnum, Lname, Address, Bdate 
FROM (
	(
		(SELECT * 
		 FROM PROJECT 
		 WHERE Plocation='Stafford') 
		JOIN DEPARTMENT ON Dnum=Dnumber) 
	JOIN EMPLOYEE ON Mgr_ssn=Ssn)
```

![[Pasted image 20231102163441.png]]

**Expression tree**:

![[Pasted image 20231102163517.png]]



## Set-theoretic Operations
- RA uses **set** semantics for relations 
	- So set-theoretic operations are **directly applicable** 
- Set theoretic operations require **type-compatibility** 
	- Two relations $R(A_i, ..., A_m)$ and $S(B_{1},..., B_{n})$, and are **type-compatible** if:
	- $m = n$ and $dom(A_i) = dom(B_i)$ for $1 < i < n$

In other words, the two relations must have **same number** of **attributes**, and **each type** must **match** the **corresponding attribute** in the other relation

![[Pasted image 20231106131547.png]]

**Convention** - Resultant relations have the **same attributes** as $R$

![[Pasted image 20231106131835.png]]

![[Pasted image 20231106131840.png]]

![[Pasted image 20231106131847.png]]

![[Pasted image 20231106131857.png]]

![[Pasted image 20231106131903.png]]


## Completeness
A language can be relationally complete without having all of the operators so far described
- Some are included for **convenience** or **brevity**
- An example of a complete set of operations could be:
  $\{\sigma, \pi, \rho, \cup, -, \times\}$

![[Pasted image 20231106134107.png]]


## Some extensions to relational algebra

![[Pasted image 20231109163342.png]]

**Generalised Projection**

![[Pasted image 20231109163714.png]]

**Aggregate functions**

![[Pasted image 20231109163725.png]]

![[Pasted image 20231109163737.png]]

![[Pasted image 20231109163908.png]]