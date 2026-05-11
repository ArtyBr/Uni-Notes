A set of connectives is said to be **complete** if we can represent **every** truth function $\{T,F\}^{n}\rightarrow \{T,F\}$ using only these connectives 
- There are $2^{2^{n}}$ such functions
##### Example: Is $\{\lnot, \land, \lor\}$ complete?
First, look at all unary functions, i.e. the case $n=1$

![[Pasted image 20240116131118.png]]

This is not too difficult:

![[Pasted image 20240116131246.png]]

![[Pasted image 20240116131303.png]]
#### Proof by construction
To create a formula that is **logically equivalent** to any function $f$ on variables $x_{1}, ..., x_{n}$ given by its **truth table**, do the following:
- For every valuation which maps to T construct the conjunction $L_1 ∧ . . . ∧ L_n$ where $L_j$ is $x_j$ if $x_j$ is assigned T under the valuation and $L_j$ is $¬x_j$ if $x_j$ is assigned F under the valuation 
- Take the **disjunction** of all **conjunctions** from the previous step 
- For the function that is everywhere F, by convention take ⊥ 

This shows that {¬, ∧, ∨} is indeed complete
The resulting formula is called a **disjunctive normal form (DNF)** of $f$ .

![[Pasted image 20240118161830.png]]
### Conjunctive Normal Form
A formula in **disjunctive** normal form (DNF) is a **disjunction** of **conjunctions** of literals 
- (p ∧ ¬q) ∨ (¬p ∧ q ∧ ¬r)
A formula in **conjunctive** normal form (CNF) is a **conjunction** of **disjunctions** of literals
- (¬p ∨ ¬q ∨ r) ∧ (¬p ∨ q ∨ ¬r)

A literal is a variable or its negation, or ⊥ or >
## Normal form algorithms
**Problem**
- Given a formula, can we **derive** its DNF or CNF in a **systematic way**, **other** than by writing down the **entire truth table**?
This can be achieved by normal form algorithms: 
- The input is any propositional formula, the output is a semantically equivalent formula in DNF or CNF
- In every step, the algorithm applies a single rewriting rule given by one of the laws of Boolean algebra
### Generalised disjunctions/conjunctions
Let $X1, X2, . . . , Xn$ be a sequence of propositional formulas
- We write a generalized disjunction as:$$[X1, X2, . . . , Xn] := X1 ∨ X2 ∨ · · · ∨ Xn$$
	- (recall associativity!) 
- We write a generalized conjunction as: $$\left<X1, X2, . . . , Xn\right> := X1 ∧ X2 ∧ · · · ∧ Xn$$
- If $X1, . . . , Xn$ are literals, then $[X1, X2, . . . , Xn]$ is called a clause, and $\left<X1, X2, . . . , Xn\right>$ is called a **dual clause**.

**Valuations** on generalised disjunctions/conjunctions:
- $v([X1, . . . , Xn]) = T$ if and only if $v(Xi) = T$ for **at least one** member of the list $X1, . . . , Xn$
- $v(\left<X1, . . . , Xn\right>) = T$ if and only if $v(Xi) = T$ for **every** member of the list $X1, . . . , Xn$
- $v([ ]) = v(⊥) = F$ (neutral element of disjunction) 
- $v(\left<\right>) = v(\top) = T$ (neutral element of conjunction)
### $\alpha$ and $\beta$ formulas

![[Pasted image 20240118164334.png]]

The formula on the left is equivalent to the symbols on the right with either a $\land$ (conjunctive) or $\lor$ (disjunctive) operator between them

**Valuations** on $\alpha$ and $\beta$ formulas
- $v(\alpha)=v(\alpha_{1})\land v(\alpha_{2})$
- $v(\beta)=v(\beta_{1})\lor v(\beta_{2})$
- i.e. we have $\alpha = \alpha_{1}\land \alpha_{2}$ and $\beta = \beta_{1}\lor \beta_{2}$

This is for the same logic that:
- $X \lor F = X$
- $X \land T = X$
### CNF Algorithm

![[Pasted image 20240119163312.png]]

 