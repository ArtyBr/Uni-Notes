$m$ - number of **edges**
$n$ - number of **nodes**

Multiple ways to represent a graph in a computer:
- **Edge list**
	- Maintain a list of edges
		- ![[Pasted image 20240118212804.png]]
	- **Storage** $O(m)$
		- Useful for I/O
		- Not very useful for many graph operations
			- e.g. $d(v)$ takes time $O(m)$
- **Adjacency Matrix**
	- Represent $G=(V,E)$ as an $n\times n$ **boolean matrix**
		- ![[Pasted image 20240118214544.png]]
		- ![[Pasted image 20240118214551.png]]
	- **Storage** $O(n^{2})$
		- Wasteful on sparse graphs ($m<n^{2}$)
	- Conceptually useful
- **Adjacency List**
	- *Usually assume a graph is given in this representation unless stated otherwise*
	- One linear list for every node
		- ![[Pasted image 20240118220201.png]]
	- **Storage** $O(n+m)$
		- Each edge stored only a constant number of times
- **Implicit Representation**
	- Some graphs can be represented using special structures
		- e.g.
		- ![[Pasted image 20240118220527.png]]
**Running time** of **breadth first search** - $O(n+m)$

![[Pasted image 20240118220601.png]]

Why does graph representation **matter**?
Using **adjacency list**:
- Find **connected components** in $O(n+m)$
- Test if graph is **bipartite** in $O(n+m)$
- One can find **strongly connected components** in $O(n+m)$
- **Topological sort** can be done in $O(n+m)$
#### Connected components
- Run BFS or DFS from a vertex $s$, and **remove** the connected component
- **Repeat** until **all** the vertices of the graph are **exhausted**
- If a component $C$ has $n_c$ vertices and $m_c$ edges, BFS or DFS takes time $O(n_{c}+m_{c})$
### Adjacency Lists vs Adjacency Matrix
Almost **everything** requires $\Omega (n^2)$ time in the **matrix representation**
