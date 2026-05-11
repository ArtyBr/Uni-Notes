Resolution focuses on **CNF** rather than DNF
- Use standard representation, $\left< [], [] \right>$, one disjunction on each line 

Here we look for the **empty clause** - since $[]=\bot$

**Resolution rule**:
- Supposed $D_1$ and $D_2$ are two **disjunctions**, with $X$ occurring in $D_{1}$ and $\lnot X$ in $D_2$ Let $D$ be the results of the following:
	- **Delete** all occurrences of $X$ from $D_1$
	- **Delete** all occurrences of $\lnot X from D_2$
	- **Combine** the resulting disjunctions
	- *Special case*: If a disjunction contains $\bot$, delete all occurrences of $\bot$ and call the resulting disjunction the **trivial resolvent**
- $D$ is the **result** of **resolving** $D_1$ **and** $D_2$ **on** $X$
- $D$ is the **resolvent** of $D_1$ and $D_2$, and $X$ is the formula being **resolved on**
	- If $X$ is atomic, then this is an **atomic** application of the resolution rule
So:
$[R_{1},X],$
$[R_{2},X]$
$\downarrow$
$[R_{1},R_{2}]$

Justification:
$\left< [X,Y],[\lnot X,Z]\right> =\left< [X,Y],[\lnot X,Z],[Y,Z]\right>$ (resolution **expansion**)

- A resolution expansion is **closed** if it contains the empty clause []
- A **resolution proof** for $X$ is a **closed resolution expansion** for $\lnot X$
- We write $\lnot_r X$ if $X$ has a resolution proof

Resolution proof is 
- **Sound** 
	- **If** $X$ has a resolution proof, **then** $X$ is a tautology
and
- **Complete**
	- **If** $X$ is a tautology, **then** the resolution system will terminate with a proof for it

**Propositional consequence**
Definition of propositional consequence $S$ |= $X$
- For every instance that is true for $S$, $X$ is true also

- **$S$-introduction rule for tableau**
	- Any formula $Y \in S$ can be added to the end of any tableau branch. We write $S$ |-$_t$ $X$ if there is a closed tableau for $¬X$ allowing the $S$-introduction rule for tableau
- **$S$-introduction rule for resolution**
	- For any formula Y ∈ S, the line [Y ] can be added as a line to a resolution expansion. We write $S$ |-$_r$ $X$ if there is a closed resolution expansion for $¬X$, allowing the $S$-introduction rule for resolution.