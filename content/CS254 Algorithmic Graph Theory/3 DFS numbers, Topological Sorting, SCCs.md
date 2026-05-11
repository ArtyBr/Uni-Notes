**Properties of DFS**:
- For **undirected graphs**, edges not contained in the DFS tree can **only** connect **ancestors** and **descendants**

**DFS numbers** record in which order the recursive calls initiated by the nodes in the network **finish**

Properties of DFS-numbering:
- All numbers are distinct
- If $(u,v)$ is a **tree**-edge with $u$ being the parent of $v$, then $N[u]>N[v]$
- If $u$ is an ancestor of $v$ in the DFS-tree then $N[u]>N[v]$
- If $(u, v)$ is a **non-tree** edge then either:
	- $N[u]>N[v]$
	- $u$ and $v$ have ancestor/descendant relationship in the DFS-forest

![[Pasted image 20240123152930.png]]

Cross-edges only go from **right** subtrees **to left** subtrees
### Topological Sorting
A mapping that maps **vertices** to **numbers**
$\phi : V \rightarrow {1,...,n}$ such that $\phi(u)<\phi(v)$ for all $((u,v)\in E)$

Numbers at the beginning of the edge is lower the number at the end of the edge

Such a sort can be produced **iff** a graph is **acyclic** (and directed) - DAG

- A vertex with **in-degree 0** is called a **source**
- A vertex with **out-degree 0** is called a **sink**

Algorithm for topological ordering:
- Do a DFS on $G$ and compute the DFS-numbering $N$
- Set $\phi(v) := n-N[v]+1$

Algorithm for finding if a directed graph has cycles:
- Run topological ordering algorithm
- For every edge check whether $\phi(u)<\phi(v)$
- If there is an edge that **fails** that test, then $G$ is **not** a DAG
- Otherwise, $G$ is a DAG
### Strongly Connected Components
Can use DFS to find SCCs via DFS

**Kosaraju's algorithm:**
- Compute graph $G'$ obtained from $G$ by **reversing** all edges
- Run DFS on $G'$ and compute DFS numbers
- Run DFS on $G$ where the (re-)starting is always done with the node that has the **maximal** DFS-number in the previous DFS run
The DFS-trees in the DFS-forest resulting from running DFS on G correspond exactly to the SCCs of $G$

$O(n+m)$

Need to do the first run on $G'$ in order to make sure we know where to start the next run - otherwise we may just get a strongly connected component consisting of 2 connected components
#### Meta-graphs
Each directed graph $G=(V,E)$ has a corresponding **meta-graph** $H$ such that:
- The vertices of $H$ are the **SCCs** of $G$
- There is an edge from $x$ to $y$ in $H$ iff there is an edge in $G$ from a vertex in the SCC corresponding to $x$ to a vertex in the SCC corresponding to $y$

The meta-graph is **always acyclic**
- If it wasn't, you could connect the nodes in a cycle to make a new connected component (eliminating the redundant nodes)

**Observation**:
- If we start DFS at any node $x$ belonging to a **sink** of the **meta-graph**, we explore the **whole SCC** to which $x$ belongs, but **no other vertices** 

Key **Lemma**:
- Let $C_1$ and $C_2$ be two SCCs of $G$ so that there is an edge in the meta-graph from $C_1$ to $C_2$. Then $$max_{v\in C_1}N[v]>max_{v\in C_{2}}N[v]$$
- Since $C_1$ will necessary be a **higher** node in the topological ordering than $C_2$ (Since the edge is going $C_{1}\rightarrow C_2$)
**Corollary**:
- The vertex with the **maximal DFS number** is contained in a source vertex of the meta-graph
- But we are interested in sinks
Idea: **Reverse** the graph

Let $G'$ be the **reverse graph** of $G$
- i.e. the graph with the **same vertex set** and all **edges reversed**

The meta-graph of $G'$ is the **reverse** of the meta-graph of $G$

**Corollary**:
- If we run DFS on $G'$ then the vertex with the **maximal** DFS number is contained in **sink** SCC of the meta-graph of $G$

**Useful Recipe from this**:
- Find SCCs and **construct meta graph**
- **Topologically sort** meta graph
- Use **dynamic programming** on DAGs to solve a problem