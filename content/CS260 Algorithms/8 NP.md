We group problems according to the sort of answer they require
- **Search problems**: `Input -> Output`
	- (Where output must fulfill some predicate)
- **Optimization problems**: `X : Input -> Output` 
	- (Such that Output is also “optimal”) 
- **Decision problems**: `X : Input -> Yes / N`
### Independent Set and Vertex Cover
An **independent set** in $G$ is a **set of vertices** such that not two vertices are **adjacent** in $G$

![[Pasted image 20231109172727.png]]

>[!note] Independet Set problem
**Independent set problem** - Given an undirected graph $G = (V, E)$ and an integer $k$, does there exist a set $S\subseteq V$ such that $|S|=k$ and no two vertices in $S$ are adjacent?

A **vertex** **cover** is a set of vertices such that every edge in the graph has at least one endpoint in the set.
$$S \text{ is a vertex cover} \Leftrightarrow \forall (u,v)\in E,u\in S \lor v \in S$$
$$S \text{ is an independent set} \Leftrightarrow \forall (u,v)\in E,u\notin S \lor v \notin S$$
**Theorem**: The **complement** of a vertex cover is an **independent set**
**Corollary**: A graph with $n$ vertices has an independent set of size $k$ iff it has a vertex cover of size $n-k$
**Corollary**: The complement of a vertex cover of **maximal** size is an independent set of **minimal** size

>[!note] Vertex-Cover
>Given an undirected graph $G=(V,E)$ and an integer $k$, does there exist a set $S \subseteq V$ such that $|S|=k$ and every edge in $G$ has at least one endpoint in $S$?

Write the reduction
**Algorithm**: Given input $G=(V,E),k$, call `INDEPENDENT-SET` with input $G=()$
## NP 
NP is a complexity class containing problems that are 'easy to verify'
- A problem is in NP if it has a solution which can be **checked** in polynomial time

A problem $X$ is in NP iff an algorithm $A$ exists such that:
- $A$ takes **two inputs**:
	- The **problem instance** $i$
	- A **witness** $s$
- If the answer to $X(i)$ is Yes, then:
	- $A$ runs in polynomial time, and
	- $A(i,s)$ returns Yes iff $s$ proves that the answer to $X(i)$ is Yes
We call $A$ a **verifier** for $X$, and $s$ a **witness** for $i$

**Witnesses**:

![[Pasted image 20231109175042.png]]

**Theorem**: $P\subseteq NP$
**Proof**: We show that every problem in $P$ has a verifier that runs in polynomial time
- Let $X$ be a (decision) problem in $P$. Then an algorithm $Alg$ exists that solves $X$ in **polynomial time**
- Define $A(i, s)=Alg(i)$. Then $A$ is a verifier for $X$ (the witness $s$ is ignored)

$NP$ is short for **nondeterministic polynomial time**.
- If we have a verifier for a problem $X$ that runs in polynomial time, we have a simple **nondeterministic** algorithm for $X$
Given an instance $i$ of $X$:
- **Guess** a possible polynomially-sized **witness** $s$ for $i$
- Run a **verifier** $A$ on the input $i$ and $s$
If $s$ is a witness for $i$, the algorithm will return **yes**, and so we get a **yes** answer

>[!note] **Definition**: 
>A problem $X$ is $NP$-complete if:
>- $X$ is in $NP$
>- For every problem $Y$ in $NP$, it is possible to reduce $Y$ to $X$ in polynomial time. $(Y\leq_{p}X)$
>$X$ is at least as hard as every other problem in $NP$

In short:
- Problems in $P$ are problems which you can **solve** in polynomial time
- Problems in $NP$ are problems which you can **verify** in polynomial time

**Fact**: It is **not known** whether $P=NP$
**Corollary**: There are **no known problems** that admit polynomially-checkable witnesses that are harder than $P$

**Problems** have a *complexity*; **Algorithms** have *running time*
**Problems** are *easy* or *hard*; **Algorithms** are *fast* or *slow*
### Beating NP
We can't solve $NP$-complete problems in polynomial time

Can use fixed-parameter-tractability:
- e.g. `K-SET-COVER` is solvable in time $O(n^{k})$

**General rule**: 
- **Restricting** the possible inputs to a problem makes it **easier** to solve
- **Relaxing** the possible inputs makes it **harder** to solve
**Example**:
- The `INDEPENDENT-SET` decision problem is $NP$-complete
- The `TREE-INDEPENDENT-SET` problem, where the input must be a tree, is in $P$
#### `TREE-INDEPENDENT-SET`
Suppose we are given a tree $T$ and a number $k$, how can we determine whether an **independent set** exists of **size** $k$?
- **Idea**: Start at the leaves and **work up** to the root
- **Algorithm**: 
	- If $T$ is the empty graph, return $k=0$
	1. $S\leftarrow \emptyset$
	2. While $T$ has at least **one edge** and $|S|<k$:
		2.1. Select any leaf $v$ in $T$ and the edge $(u, v)$ incident to $v$ 
		2.2. $S\leftarrow S\cup \{v\}$
		2.3. Delete $u, v,$ and all edges incident to $u$ and $v$ from $T$
	3. Let $S'$ be the union of $S$ and all remaining vertices in $T$
	4. Return $S'\geq k$
## EXP Time
A problem is in `EXPTIME` iff it admits an algorithm that can **solve** it in time $O(k^n)$
**Theorem**: `3-SAT` is in `EXPTIME`
**Proof**: A solution to a `3-SAT` instance is an assignment to the variables
- The number of variables is $O(n)$ in the size $n$ of the instance, and each can be assigned by True or False. So there are at most $2^n$ possible assignments.
	- **Each** can be **checked** in polynomial time
- So a complete algorithm for `3-SAT` is:
	- Generate **each possibility** in turn and check them until the term evaluates to True. 
	- Runs in $O(2^{n}\times n^{k})=O(2^{n})$

**Theorem**: $NP\subseteq$ `EXPTIME`
**Proof**: 
- Let $X$ be a problem in $NP$. 
- Since `3-SAT` is complete for $NP$, there is a polynomial-time reduction from $X$ to `3-SAT`
- So, since `3-SAT` is in `EXPTIME`, $X$ can be solved in time at most $O(2^{n}\times n^{k})=O(2^{n})$. So $X$ is in `EXPTIME` as well
**Corollary**: This algorithm uses polynomial space, so $NP\subseteq \text{PSPACE}$ as well
