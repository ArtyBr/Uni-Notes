# 1

![[Pasted image 20231124170545.png]]

$(A\land B\land C)\lor(B\land C\land D)\lor(C\land D\land A)\lor(D\land A\land B)$
Satisfiable if **all True**

![[Pasted image 20231124170906.png]]

Check if satisfiable: 
- For each clause:
	- If there exists a variable **and** its negation, remove it from the list of clauses
- If at the end, no clauses - **not satisfiable**
- If satisfiable:
	- From remaining clauses, pick one at random, and assign variables such that: 
	- If variable **isn't** `not`-ed, set it to True
	- Otherwise set it to False
# 2

![[Pasted image 20231124171656.png]]

Contra-positive: if there is an algorithm for `4DEG-IND-SET`, then exists an algorithm for `2LIT-3SAT`
Prove that there is a reduction from `4DEG-IND-SET`to `2LIT-3SAT`


Let $\phi = C_{1}\land C_{2} \land ...C_m$ be an instance of `2LIT-3SAT`
- Every instance of a literal in a clause is connected by an edge to each other
- Every instance of a literal is connected by an edge to its negation
Finding an independent set of size $m$ implies a node from each literal belongs to a corresponding assignment such that if they were set to true, the formula is satisfied (and therefore satisfiable)

Instance of `4DEG-IND-SET` as:
For each variable there is at most 1 negation, so Degree for each variable in a graph would be at most 4 (2 other variables in the clause and its negation can appear twice).

Efficient as:
Number of nodes is at most $n$
Number of edges is at most 4 for every $n$
Graph is linear size
So creating the graph can be done in polynomial time, and calls `4DEG-IND-SET` only once, which is polynomial time
# 3

![[Pasted image 20231124173357.png]]

Reduce from `IND-SET` to `CLIQUE`

Given an instance of `IND-SET`, Graph $G$ and an integer $k$
Then construct 'complementary graph': $G'=(V,E'), E'=\{\{u,v\}:u\in V,v\in V,\{u,v\}\notin E\}$
**Claim**: $G'$ contains a $k$-clique iff $G$ contains `k-IND-SET`

**Efficient**:
Constructing $E'$ requires at most going through each pair - $|V|^{2}$ time - $\leq n^2$
# 4

![[Pasted image 20231124174512.png]]

![[Pasted image 20231124174626.png]]

Certifier: 
**Input: 
- **Instance** 
	- $(G, k)$
- **Certificate** (witness)
	- List of $2k$ nodes in 2 halves
	- $A$ - Head of the kite
	- $B$ - Tail of the kite
- Now:
	- For $u\in A$
		- For $v \in A$, check $\{u, v\}\in E(G)$
	- For $B$:
		- For $i=1$ to $k$, check $\{b_{i},b_{i+1}\}\in E(G)$
- $\rightarrow$ ${A,B}\in G$

