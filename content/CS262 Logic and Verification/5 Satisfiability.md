SAT: Given a formula in CNF, is there a satisfying assignment for it?
- If **yes**, how can we **compute** it efficiently?
- If **not**, how can we prove this?

**Definitions**:
- **(Positive/Negative) literal** - A variable or negated variable
- **CNF formula** - A formula in CNF
- **$k$-CNF formula** - Every clause has **at most** $k$ literals
- **$k$-SAT problem** - Input of SAT is a $k$-CNF

We can solve the problem $SAT$ in time $2^n{\times}L$ by computing the entire truth table, where $L$ is the total number of literals of the input formula, and $n$ is the number of variables.

**Reductions**
- If we can solve 3-SAT efficiently, then we can solve SAT efficiently (in polynomial time)
	- **Proof**:
	- Consider a clause containing **two literals** $X$ and $Y$
	- Replace $X\lor Y$ in the clause by a **new variable** $Z$, and express $X\lor Y \equiv Z$ by a 3-CNF $F(X,Y,Z):[...]$
	- Take the **conjunction** of the current CNF with $F(X,Y,Z)$
	- Repeat the process on clauses of length > 3

- If we can solve SAT, efficiently, then we can solve COLORING efficiently
	- **Proof**:
	- For each **vertex** $v$ and each **color** $k$, introduce a variable:
	- ![[Pasted image 20240215162940.png]]
	- Add constraints that every vertex receives exactly one color: $[...]$ 
	- Add constraints that the end vertices of every edge receive two distinct colors: $[...]$ 
	- Number of variables: $|V| · k$
	- Size of the formula: polynomial in $|V|$ and $|E|$

#### **Solving $2-SAT$ in polynomial time**
First method: **Resolution**
- Recall the resolution proof method: 
	- Maintains a conjunction of disjunctions 
	- First step: resolution expansion into CNF 
	- Second step: resolution rule - 
		- If you have 2 clauses $[X,Y]$ and $[\lnot X, Z]$
		- Then you can make a new clause $[Y,Z]$
		- i.e. $[X,Y],[\lnot X,Z]\rightarrow [Y,Z]$
- **Key observation**: The resolution rule on clauses of size 2 produces another clause of size ≤ 2
	- (Example: $[x, y], [¬x, ¬z] [y, ¬z]$)
- At most $1 + 2n + 4(\frac{n}{2})=2n^2+1$ clauses can ever occur in the process (n is the number of variables)
- If formula is unsatisfiable, then the empty clause $[ ]$ will occur; otherwise it is satisfiable
- Hence resolution is a polynomial method for deciding 2-SAT

Second method: **SCCs in a directed graph**
- Construct a directed graph, where there is a vertex for each literal ($x$ and $\lnot x$ for each $x$)
- Make an edge from each literal to the negation of another literal if there is a disjunction $[x, y]$ in the graph
	- i.e. make an edge in this case $\lnot x\rightarrow y$
	- Since $A \rightarrow B \equiv \lnot A \lor B$
- Find SCCs in graph:
	- **If** there is some $x$ and $\lnot x$ in 1 SCC, then **not satisfiable**
	- **If** not, **satisfiable**

### $3-SAT$
Want to do **better** than naive approach of $2^n$

**Idea**: Want to use our polynomial-time algorithms for solving $2-SAT$ for solving $3-SAT$ more efficiently i.e. in less than $2^n$ steps ($n$ is the number of variables)

**Definition**: Given a 3-CNF $F$ over $n$ variables, a subset $G$ of clauses of $F$ is called **independent** if **not two clauses** **share** any variables
- i.e. $F=\left<[a,b,\lnot c],[c,d,\lnot e],[\lnot e,f,g],[\lnot f,g,\lnot h],[h,i,j]\right>$
	- Here, $n=10$
- e.g. Independent subset $G=\left<[a,b,\lnot c],[\lnot f,g,\lnot h]\right>$

We want to build a subset $G$ of **maximal** size - i.e. no further clauses can be added without violating independence
- e.g. Independent subset $G'=\left<[a,b,\lnot c],[\lnot e, f,g],[h,i,j]\right>$
- e.g. Independent subset $G'_{2}=\left<[c,d,\lnot e],[\lnot f,g,\lnot h]\right>$
- However, this adjective of being **maximal** is only **local** and **not global**, so there can be other sets $G_{n}$ with more clauses, only rule for our maximal subset is that you can't add any more disjunctions to it given the subset that we are constructing so far

 **Lemma**:
 Consider a maximal set $G$ of independent 3-clauses in $F$. Then we have:
 - $|G| \leq \frac{n}{3}$
	 - $n$ variables, each clause contains 3 variables
- For any truth assignment $\alpha$ to the variables in $G$, $F^{[\alpha]}$ is a 2-CNF
	- Here, $F^{[\alpha]}$ is the formula obtained from $F$ by setting all variables defined by $\alpha$ to true or false (remove clauses with true literals and remove false literals from clauses)
- The number of truth assignments satisfying $G$ is $7^{|G|}\leq 7^{\frac{n}{3}}$
	- Since $2^3=8$  assignments possible, **except** the one where every literal is false, so actually $8-1=7$
This comes out to around about $O(1.913^n)$

### Horn $SAT$
A **Horn clause** is a clause in which there is **at most one positive literal**
- A **Horn CNF** is a CNF in that has only Horn clauses

Looks like this:
- $[\lnot x,\lnot y, \lnot z, a]=(x\land y\land z)\rightarrow a$

Therefore can be written in prolog as:
- `a :- x, y, z`

Simple observations:
- If $F=\left< \right>$, then it is satisfied by **any assignment**
- If every clause has size $\geq 2$, then the formula can be satisfied by setting **all variables to false**
- If the formula has a clause of size $=1$, then we have to set the literal this clause to **true** to satisfy the formula
- If the formula contains the empty clause $[]$ then the formula is **not satisfiable**

![[Pasted image 20240225202910.png]]

$O(n)$ solution

### $SAT$ solving
**Brute force** approach and **heuristics**

Two different paradigms:
- **Complete methods**
	- Either finds satisfying assignment or a proof that none exists (aka systematic solvers)
- **Incomplete methods**
	- Do no provide a guarantee for eventual results, typically run with timing limit, based on stochastic local search
#### Naive Backtracking (complete)

![[Pasted image 20240225204522.png]]

Search tree which looks through all possible assignments of variables in $O(2^n)$ time

**Improvements**:
- 2 **observations**
	- **Unit clauses** $[l]$ force the assignment $l:=T$; this is called **unit clause propagation**
	- **Pure literals** i.e. literals $l$ for which $\lnot l$ does **not appear** in the formula - can be set to $l:= T$ w.l.o.g.

![[Pasted image 20240225205950.png]]
#### DPLL Algorithm (Complete) - better backtracking

![[Pasted image 20240225210403.png]]

**Improvements**:
- Which literal to choose in the next recursion step?
- And which branch $l$ or $\lnot l$ to try first?

**Static heuristics**
- Linear ordering of variables **fixed**, before start, usually very fast to compute, can thus use more expensive algorithms
**Dynamic heuristics**
- Determine ordering based on current formula $F$, typically from number of occurrences of literals (in clauses of current $F$)
- **DLIS** (Dynamic Largest Individual Sum): choose literal which occurs most frequently 
- **MOMS** (Maximum Occurrence in Clauses of Minimum Size): choose literal which occurs most frequently in clauses of minimum size
#### Watched literals
In the algorithms before, every time a variable is assigned/unassigned a value, we would inspect **all clauses** containing this variable

**Improvement**:
When does a clause matter during search? 
- Going from 2 non-false literals to 1 (triggers **unit clause propagation**)
- Going from 1 non-false literal to 0 (**conflict**)

Choose 2 **watched literals** in each clause
- Maintain **invariant**: Watched literals are **non-false** (either true or not yet assigned) if clause is not satisfied

Each literal has a **watch list** $W(l)$ of all clauses watching $l$
- When literal $l$ is **falsified**, visit every clause $C\in W(l)$:
	- If any literal is assigned true, **continue**
	- If all literals are assigned false, **return** (backtrack)
	- If all but one literal $l'\in C$ is assigned **false**, assign $l'$ **true** (unit clause propagation) and **continue**
	- **Otherwise**, **add** $C$ to the watch list of one of its remaining unassigned literals and **remove it** from the watch list $W(l)$

**Observation**: Backtracking does not need any updates of the watch lists, as it only unassigns values, but never falsifies a literal (invariant is maintained) $\rightarrow$ **lazy data structures**

![[Pasted image 20240225213430.png]]

#### Clause learning
When backtracking, we reset to start of branch but this may not have been the cause of the conflict - might have been a **subset** of the branch which will be explored over and over again

**Idea**: Rule out this subset by **adding a clause**
- ![[Pasted image 20240225213647.png]]
