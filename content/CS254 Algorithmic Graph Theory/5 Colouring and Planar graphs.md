Let $G=(V,E)$ be a graph, and let $C$ be a “set of colours.” 
- A (proper) vertex-colouring of $G$ with colour-set $C$ is a function $c:V\rightarrow C$ such that $c(u)\neq c(v)$ for all edge $(u,v)\in E$ 
- If $|C|=k$ we call the colouring $c$ a (proper) $k$-colouring of $G$

A **chromatic number** $\chi(G)$ of a graph $G$ is the smallest $k$ such that there exists a $k$-colouring of $G$

**Examples**
- A **bipartite** graph $G$ has $\chi(G)=2$
- A **tree** $T$ has $\chi(G)=2$
- An **odd cycle** $C$ has $\chi(C)=3$
- On a complete graph $K_n$ on $n$ vertices has $\chi(K_n)=n$
- If a graph $G$ contains a **subgraph** $G'$ then $\chi(G)\geq \chi(G')$

**Definition**: Given a graph $G=(V,E)$ the clique-number $\omega(G)$ is the cardinality of the largest subset $M \subseteq V$, such that $G[K]$ is a complete graph, where $G[K]$ is the subgraph of $G$ induced by vertex set $K$

**Lemma**
- If a graph contains a $k$-clique (a $K_k$) then its chromatic number is at least $k$. Hence, $$\chi(G)\geq \omega(G)$$
**Greedy colouring algorithm**:

![[Pasted image 20240130152251.png]]

**Lemma**: GreedyColouring colours $G$ with at most $\Delta(G)+1$ colours
- Where $\Delta(G)$ denotes the maximum degree of $G$

**Proof**: Every time we want to colour a vertex from a palette of $\Delta(G)+1$ colours, we always have some colour available

**Theorem**: $\chi(G)\leq \Delta(G)+1$ for any graph $G$

For any $K\geq 3$, determining $\chi(G)$ is NP-complete
### Planar graphs
Planar if it can be drawn in a plane without edges crossing

Given a planar embedding, a face is the connected component after we deleted the vertices and edges of the drawing

**Examples**
- Every tree is planar
- Every cycle $C_n$ is planar
- $K_4$ is planar
- $K_5$ is **not** planar
- $K_{3,3}$ is **not** planar

**Euler's formula**: $|V|+|F|=2+|E|$ for planar graphs

**Proof**: By induction
- If $G$ is acyclic, then $|F|=1$, and the theorem holds since $G$ is a tree and $|E|=|V|-1$
- Otherwise $G$ has a cycle
	- Let $e$ be an edge in a cycle. Delete $e$ from $G$ and let $G*$ be the resulting graph
	- No $G$ has one less edge, one less face and same vertices
	- By induction, we have that $|V|+(|F|-1)=2+(|E|-1)\rightarrow |V|+|F|=2+|E|$

**Theorem**: For any simple connected planar graph $G$ with $n>2$ it holds that $|E|\leq 3n-6$ (where $n=|V|$)

**Planar graphs are sparse**

**Proof**:
- Every face has at least 3 edges bounding it
- Every edge bounds at most two faces
- Therefore, $2|E| \geq 3|F|$
- Apply Euler's formula

![[Pasted image 20240130154608.png]]

**Corollary**: $K_5$ is not planar
**Proof**: 
- $K_5$ has 5 vertices, each vertex has 4 neighbours $\rightarrow n=5, m=10$
- If $K_5$ was planar then we would have $10=|E|\leq 3\times 5-6=9$

**Lemma**: Every simple planar graph has a vertex of degree at most 5
**Proof**: By contradiction
- Assume all vertices have degree $>5$
- ![[Pasted image 20240130155153.png]]
**Corollary**: Every simple planar graph is 6-colourable
**Proof**:
- Since $G$ has a vertex of degree $\leq5$, then recursively:
- Find a vertex $v$ with $d(v)\leq 5$
- Recursively, colour $G-v$ using 6 colours
- Extend the 6-colouring of $G-v$ by colouring $v$ in a colour distinct from the colours of its $d(v)\leq 5$

**Theorem**: Every simple planar graph is 5-colourable
**Proof**: 
- If $G$ has a vertex of degree $\leq 4$, then we are done by induction as in the previous proof
- If not, $G$ has a vertex $v$ of degree 5
- Remove $v$ from $G$ and colour the obtained graph. Bring $v$ back.
	- If among the 5 neighbours of $v$ in $g$ not all 5 colours are used, then we may colour $v$ the missing colour and we are done
- Otherwise, $v$ has 5 neighbours $u_{1},u_{2},...,u_{5}$. Wlog, vertex $u_i$ with colour $i$. Wlog they're arranged in clockwise order.
- We will try to replace the colour of $u_1$ with colour 3
- Consider the subgraph $H$ of $G$ induced by vertices with colours 1,3
	- If $u_{1}$ is disconnected from $u_{3}$ in $H$, then consider the component of $H$ containing $u_3$ and swap colours 1 and 3 in that component
- Otherwise, take path $\pi$ from $u_1$ to $u_3$ in $H$
	- If we add edges $\{v,u_{1}\}$ and $\{v,u_3\}$ to $\pi$ then we obtain a closed curve separating $u_2$ from $u_4$
	- Therefore the graph induced by vertices coloured 2 and 4 cannot have $u_2$ and $u_4$ connected. Therefore, in that graph, in the component containing $u_4$ swap colours 2 and 4
- As the result, we assigned only 4 colours to the neighbours of $v$, and therefore we can colour $v$ with colour 4

![[Pasted image 20240130160923.png]]

**Duals**
The **dual graph** $G^{*}=(V^{*},E^{*})$ is defined as follows:
- each vertex $u\in V^{*}$ corresponds to a face in $G$
- two vertices in $G^{*}$ are connected by an edge if the corresponding faces in $G$ have a boundary edge in common

Basic properties:
- $|V^{*}|=|F|,|E^{*}|,|F^{*}|=|V|$
- $G^{*}$ is planar

Colouring the vertices in $G$ corresponds to colours the faces of its dual $G^{*}$

**Kuratowski's theorem**
- A graph is **planar** iff it has **no** subgraph of $K_5$ or $K_{3,3}$
