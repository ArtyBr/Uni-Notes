Is there a partition of $V$ into two sets $V_1$ and $V_2$ such that $V_1$ and $V_2$ are **independent sets**?

- This is bipartite graph:

![[Pasted image 20240115171307.png]]

- This is non-bipartite graph:

![[Pasted image 20240115171413.png]]

- Put the first node into $V_1$
- Second node into $V_2$
- ... Ninth node has to be in $V_1$
	- Ninth node is adjacent to first node..
- Can't be bipartite

**Corollary**:
- A graph that contains an odd cycle is not bipartite

**Using Breadth-first search:**
- High-level idea: explore the vertices of a graph in “layers.” 
	- Layer 0 consists only of the starting vertex s
	- Layer 1 contains all neighbours of s
	- Layer 2 contains all neighbours of layer-1 vertices that do not already belong to layer 0 or 1
	- .. and so on

![[Pasted image 20240115171724.png]]

**Two BFS properties:**
#### BFS **Property 1**: 
Edges not contained in the BFS-tree can only connect successive layers, or may connect nodes on the same layer

**Proof:**
- By contradiction:
	- Assume $\{u,v\}$ is non-tree edge with $u\in L_i$ and $v\in L_j$ so that $i<j-1$
- Recall **Property 2**: 
	- The BFS-tree contains a shortest $(s,v)$-path for every node $v$ reachable from $s$
- Hence, the **shortest** $(s,u)$ path has length $i$ and a shortest $(s,v)$ path has length $j$
- There is a $(s,u)$ path of length $i$ and there is the edge $u,v$
	- Shortest $(s-v)$ path has **length** **at most** $i+1$
- Hence $j \leq i+1$
- Contradiction
#### BFS **Property 2**: 
The BFS-tree contains a shortest $(s,v)$-path for every node $v$ reachable from $s$

- **Proof**:
	- First show the following **claim**:
		- The nodes are added to the BFS-tree $T$ in non-decreasing order of distance (in $T$) to $s$
	
	- By **contradiction**, let $v$ be a vertex **closest** to $s$ in $G$ for which:
		- **no** **shortest path** from $s$ to $v$ is contained in $T$
	- Consider a shortest $(s,v)$-path in $G$ and let $u$ be the neighbour of $v$ on that path
	
	- Observe:
		- Shortest path from $u$ to $s$ **will** be contained in $T$
		- dist$_T$($s,x$) > dist$_T(s,u)$
			- where $x$ is the closest neighbour on the path $s, v$ that is not shortest
	- Follows:
		- **Claim**: $u$ is visited by BFS before $x$
		- Therefore:
			- $(u,v)$ must be explored **before** $(x, v)$
			- So $(x, v) \notin T$ 
			- **Contradiction**!

We can run BFS and analyse the structure of the **explored graph**
- For $G$ to be **bipartite**, there must be no edge $(u,v)$ with $u$ and $v$ on the same level in the BFS-tree

Proof:
- Assume there was such an edge:
- ![[Pasted image 20240518141958.png]]
- The path going from the parent of both nodes down to one node is of length $n$, and the path going down to the other node is also of length $n$
- But we also have an edge connecting the 2 nodes, so there is now a cycle, whose length is $2n+1$!
- This is an odd cycle, and so cannot be bipartite
- Since BFS Property 1 is true, don't need to worry about any other edges

Other way:
- For each level have a label $L_n$
- For each $n$ that is even, label nodes one colour, and other colour for odd levels

This gives:
- A graph is bipartite **iff** it does not contain an odd cycle