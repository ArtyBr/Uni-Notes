The **boolean formula** is a formula built of from Boolean **variables** and Boolean **operations**
- Boolean operators are defined by **truth tables**
- There are four **unary** Boolean operations (taking **one** argument)
	- ![[Pasted image 20231116171132.png]]
- There are sixteen **binary** Boolean operations (taking **two** arguments)
	- ![[Pasted image 20231116171350.png]]
- A **literal** is either a **variable** or its **negation**: either $x$ or $\lnot x$
- A **clause** is a **disjunction** ('OR') of literals: $x \lor y \lor z$

>[!note] CNF
>A formula is in **conjunctive normal form** (CNF) if it is a **conjunction** of **clauses** (an 'AND' of 'OR's)
>$$(x\lor y)\land(y\lor z)$$

**Theorem** - Every boolean formula $\phi$ has an equivalent formula $\phi '$ in CNF

**Exercise** - Convert $x\rightarrow (y\land z)$ to CNF
- $a \implies b= \lnot a \lor b$
- so:
- $\lnot x \lor (y \land z)$
- $(\lnot x \lor y)\land (\lnot x \lor z)$

>[!note] Boolean satisfiability problem (SAT)
>Given a Boolean formula $\phi$ in CNF, is there some **assignment** of values to the variables that make $\phi$ **true** overall?
>If **yes**, we say $\phi$ is **satisfiable**

**Theorem** - SAT is **complete** for $NP$

Easy: Prove that it's **in** $NP$
Hard: Prove that **every** problem in $NP$ **reduces** to it

The $k$-SAT problem asks whether a given Boolean formula $\phi$ is satisfiable, where:
- $\phi$ is in CNF, **and**
- each clauses in $\phi$ has at **most** $k$ literals

![[Pasted image 20231116173744.png]]

![[Pasted image 20231116173903.png]]

**Theorem** - `INDEPENDENT-SET` is $NP$-complete
**Proof** - Already shown that **in** $NP$
- Remains to prove that $NP$-**complete**

**Working:**
- **Prop 1** - Independent set in NP 
	- An independent set of size $k$ can be **checked** in **polynomial time**
- **Prop 2** - Recall that `3-SAT` is NP-complete
	- Reduce *from* `3-SAT` *to* `Independent Set`
	- Given formula $y$, **construct** a graph $G$ such that $G$ has an independent set of size $k$ iff 
		- $y$ is **satisfiable**
		- $y$ has exactly $k$ **clauses**
		- $(x_{1}\lor x_{2}\lor x_{3})\land (\lnot x_{1}\lor ...)$
	- ![[Pasted image 20231120144151.png]]
	- We construct $G$ such that:
		- Every **vertex** is a **literal** in some **clause**
		- Every **vertex** is **connected** to every other vertex in the **same clause**
		- Every **vertex** is **connected** to its **negation**
	- $G$ has an **independent set** of size $k$ iff $y$ is **satisfiable**
	- Prove if independent set of size $k$ $\rightarrow$ $y$ is satisfiable
		- There must be **at most** $1$ element per triangle in the independent set
		- Since there are $k$ triangles, there must be **exactly** $1$ per triangle
		- We know there are **no** literals $x, \lnot x$ in the independent set
		- So our values in the independent set label a **satisfying** assignment of $y$
	- Prove if $y$ is satisfiable $\rightarrow$ independent set of size $k$
		- **At least** one literal in each clause is `TRUE`
		- Take vertices labelling the literals which exclude to `TRUE`
		- **Choose** one vertex from **each** cluster
## The `Set-Cover` problem
>[!note] The Set-Cover Problem
>Given:
>- A set $X$ of $n$ elements
>- A set $S\subseteq P(X)$ of $X$
>- An integer $k\leq |S|$
>Is there a set $S'\subseteq S$ of size $k$ such that $\cup S'=X$?

**Exercise**
Find a solution to the following instance of `Set-Cover`
- $X=\{1, 2, 3, 4, 5\}$
- $S=\{\{4\}, \{1, 4\}, \{1, 2\}, \{5\}, \{1, 3, 5\}\}$
- $k=3$

**Theorem**: `Set-Cover` is NP-complete
**Proof** - By reduction *from* `Vertex-Cover` *to* `Set-Cover`
