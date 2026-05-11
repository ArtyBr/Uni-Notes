### Graph traversal
Two problems:
**$s-t$** **connectivity** problem
- Given two nodes $s$ and $t$ is there a **path** between them?
$s-t$ **shortest path** problem
- Given two nodes $s$ and $t$, what is the **length** of the **shortest path** between them?
#### Depth first search
- Start at $s$, visit an arbitrary **neighbour** $v$ then a **neighbour** of $v$...
- Until you reach a node where **all** neighbours are **already visited**
- Then **backtrack** to a previous node with **unexplored** neighbours
- Keep going until **all** neighbours are **visited**
#### Breadth First Search
**Intuition** - Explore outward from $s$ in all possible directions, adding nodes one 'layer' at a time
**Algorithm**:
- $L_{0}= \{s\}$
- $L_1$ = all neighbours of $L_0$
- $L_2$ = all nodes that **do not belong** to $L_0$ or $L_1$, and that **have an edge** to a node in $L_i$
- $L_{i+1}$ = all nodes that do **not belong** to an **earlier** layer, and that have an edge to a node $L_i$
**Theorem** - For each $i$, $L_i$ consists of **all nodes** at distance **exactly** $i$ from $s$. There is a **path** from $s$ to $t$ *iff* $t$ **appears** in some layer

**Property** - Let $T$ be a BFS tree of $G=(V, E)$, and let $(x, y)$ be an edge of $G$. Then the levels of $x$ and $y$ differ by **at most** 1
##### BFS Analysis
**Theorem**: The above implementation of BFS runs in $O(m+n)$ time if the graph is given by its **adjacency representation**
- Easy to prove $O(n^2)$ running time:
	- At most $n$ lists $L[i]$
	- Each node occurs on **at most** one list; for loop runs $\leq n$ times
	- When we consider node $u$, there are $\leq n$ **incident** edges $(u, v),$ and we spend $O(1)$ processing **each edge**
- Actually runs in $O(m+n)$ time
	- When we consider node $u$, there are deg($u$) incident edges $(u, v)$
	- **Total** time **processing** edges is $\sum_{u\in V}$deg($u$)$=2m$
### Testing Bipartiteness
Given a graph $G$, is it bipartite?
Many graph problems **become**:
- **Easier** if the underlying graph is bipartite (*matching*)
- **Tractable** if the underlying graph is bipartite (*independent set*)

A graph is **bipartite** if the nodes can be colored red or blue such that **every edge** has **one** red and **one** blue end

**Lemma** - If a graph $G$ is bipartite, it **cannot** contain an **odd length cycle**
- Proof: **Not possible** to **2-color** the **odd** cycle, let alone $G$
---
In fact, **absence** of odd cycles **characterizes** bipartiteness

![[Pasted image 20231025202044.png]]

**Lemma** - Let $G$ be a connected graph, and let $L_{0}, ..., L_{k}$ be the **layers produced** by BFS starting at node $s$. Exactly **one** of the following holds:
1. **No** **edge** of $G$ joins two nodes of the same layer, and $G$ is **bipartite**
2. **An** edge of $G$ joins two nodes of the same layer, and $G$ **contains** an **odd-length cycle** (and hence is **not** bipartite)

![[Pasted image 20231025202055.png]]
##### Proof for case 1:
- Suppose **no** edge joins **two** nodes in same layers
- By BFS **property**, all edges must join nodes on **adjacent** nodes
- **Bipartition**: **Red** = nodes on **even** layers, **Blue** = Nodes on **odd** layers

![[Pasted image 20231025211015.png]]

##### Proof for case 2:
- Suppose $(x, y)$ is an edge with $x, y$ in same level $L_j$
- Let $z-lca(x, y)=$ **lowest common ancestor**
- Let $L_{i}$ be level **containing** $z$
- Consider **cycle** that takes edge from $x$ to $y$, then **path** from $y$ to $z$, then **path** from $z$ to $x$
- Its length is $1+(j-i)+(j-i)$, which is **odd**

![[Pasted image 20231025212202.png]]


### Connectivity in Directed Graphs via BFS
#### Connected Component
Find all nodes reachable from $s$
```js
R will consist of nodes to which s has a path
Initially R={s}
While there is an edge (u, v) where u is in R and v not in R
	Add v to R
Endwhile
```
Upon termination, $R$ is the connected component containing $s$

**Directed graph**
$G = (V , E)$ - edges are **ordered pairs**
- Edge $(u, v)$ goes from node $u$ to node $v$ **only**
![[Pasted image 20231026171027.png]]
#### Strong connectivity
Nodes u and v are mutually reachable if there is a path from u to v and also a path from v to u 
- G is *strongly connected* if **all node pairs** are **mutually reachable**
**Lemma**: Let s be any node. 
- G is strongly connected iff every node is reachable from s, and s is reachable from every node 
**Proof** ->Follows from definition of strongly connected 
**Proof** <- Path from u to v: concatenate u-s path with s-v path Path from v to u: concatenate v-s path with s-u path
#### Strong Connectivity Algorithm
**Theorem** - can test if $G$ is **strongly connected** in $O(m+n)$ time
**Proof** by **construction**
- Pick any node $s$
- Run BFS from $s$ to $G$
- Run BFS from $s$ to $G^{rev}$: reverse orientation of every edge in $G$
- Return true iff all nodes are reached in both BFS executions
- Correctness follows immediately from previous lemme
![[Pasted image 20231026171353.png]]
### Minimum Spanning Trees
Given a connected graph $G = (V, E)$ with real-valued edge weights $c_e$, an MST is a subset of the edges $T \subseteq E$ such that $T$ is a spanning tree whose sum of edge weights is minimized
![[Pasted image 20231026171603.png]]

**Cayley's formula** - There are $n^{n-2}$ **spanning trees** of $K_n$
- So **brute force** is **not** a **feasible option**
#### Greedy algorithms for MST
**Kruskal's algorithm.** Start with $T = \phi$. Consider edges in ascending order of cost. Insert edge e in T unless doing so would create a cycle 
**Reverse-Delete algorithm.** Start with T = E. Consider edges in descending order of cost. Delete edge e from T unless doing so would disconnect T 
**Prim's algorithm.** Start with some root node s and greedily grow a tree T from s outward. At each step, add the cheapest edge e to T that has exactly one endpoint in T 
**Boruvka’s algorithm.** Start with each node in its own cluster. Add the cheapest edge outgoing from each cluster (in parallel), and merge these clusters. Iterate until no more merges 
**Remark.** **All** these algorithms produce an **MST**
#### Cycles and Cuts
**Cycle**. Set of edges the form $a-b, b-c, c-d, …, y-z, z-a$
**Cutset**. 
- A **cut** is a **subset** of nodes $S$. 
- The corresponding **cutset** $D$ is the subset of **edges** with exactly **one endpoint** in $S$
![[Pasted image 20231026172458.png]]

**Cycle-Cut intersection**
**Claim** - A **cycle** and a **cutset** **intersect** in an **even number** of edges

![[Pasted image 20231026172541.png]]

![[Pasted image 20231026172924.png]]

![[Pasted image 20231026173431.png]]

![[Pasted image 20231026173759.png]]


### Dijkstras algorithm
**Shortest path problem** - find shorted directed path from $s$ to $t$, considering **weights**
- Maintain a set of **explored nodes** S for which we have determined the shortest path distance $d(u)$ from $s$ to $u$
- Initialise $S=\{s\}, d(s)=0$
- Repeatedly choose unexplored node $v$ which minimises $$c(v)=min_{e=(u,v), u\in S}d(u)+w(e)$$
- Then add $v$ to $S$, and set $d(v)=c(v)$
#### Proof of Correctness
**Invariant**: For all $u \in S, d(u)$ is the length of the shorted $s-u$ path
**Proof** (By **induction** on $|S|$)
- **Base case** - $|S|=1$ is trivial as we set $d(s)=0$
- **Inductive hypothesis** - Assume true for $|S|=k \geq 1$
	- Let $v$ be **next node** added to $S$, and let $u-v$ be the **chosen edge**
	- The **shortest** $s-u$ path **plus** $(u, v)$ is an $s-v$ path of **length** $c(v)$
	- Consider **any** $s-v$ path $P$. We'll show that it's **no shorter** than $c(v)$
		- Let $x-y$ be the **first edge** in $P$ that **leaves** $S$, and let $P'$ be the **subpath** to $x$
		- $P$ is **longer** than $c(v)$ when it reaches y:
		- ![[Pasted image 20231029171351.png]]
#### Implementation
![[Pasted image 20231029171412.png]]

