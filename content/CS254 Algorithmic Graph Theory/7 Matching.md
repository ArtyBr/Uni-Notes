**Matching Problem**:
- **Input**: Undirected graph $G=(V,E)$
- A **matching** is a subset $H$ of the edges such that **no two edges share an end-point**

![[Pasted image 20240219171009.png]]

**Goal 1**
- Find a matching of **maximum cardinality**
**Goal 2**
- In the case that edges are **weighted** (each edge $e\in E$ has a weight $w_e\geq0$ ) we sometimes aim at finding a matching of **maximum weight**, where the weight of a matching is the sum of the weights of its edges

### Bipartite Matching
- **Input**: undirected, bipartite graph 
- $M\subseteq E$  is a matching if each node appears in at most one edge in $M$
- Max matching: find a max cardinality matching
- A matching is **perfect** if $|M|=|L|=|R|$

![[Pasted image 20240219171344.png]]

**Max-flow formulation**
- Max-flow problem:
	- Given a directed graph $G=(V,E)$
	- With two special vertices $s$ (source) and $t$ (target/sink)
	- With the capacity on every edge, $c:E\rightarrow R_{>0}$,
	- Find an $s-t$ flow $f$ of maximum value

![[Pasted image 20240219171500.png]]

**Max flow formulation**
- Create digraph $G'=(L\cup R\cup \{s,t\},E')$ 
- Direct all edges from $L$ to $R$, and assign infinite (or unit) capacity
- **Add source** $s$, and unit capacity edges from $s$ to each node in $L$
- **Add sink** $t$, and unit capacity edges from each node in $R$ to $t$ 

![[Pasted image 20240219172352.png]]

**Theorem**: Size of a maximum matching in $G$ is **at most** the value of a max flow in $G'$
- Given max matching $M$ of cardinality $k$
- Consider flow $f$ that sends 1 unit along each of $k$ paths
- $f$ is a feasible flow, and has cardinality $k$

![[Pasted image 20240219173000.png]]

**Theorem**: Size of a maximum matching in $G$ is **at least** value of max flow in $G'$
- Let $f$ be a max flow in $G'$ of value $k$
- Integrality theorem $\implies$ $k$ is integral and we can assume $f$ is 0 or 1
- Consider $M$ := set of edges from $L$ to $R$ with $f(e)=1$
- Each node in $L$ and $R$ participates in at most one edge in $M$
- $|M|\geq k$, since the flow must use at least $k$ edges

![[Pasted image 20240219173511.png]]

### Matching

**Augmenting Path**:
- Given a matching $M$ in a graph $G$, a vertex that is not incident to any edge of $M$ is called a **free vertex** with respect to $M$
- For a matching $M$ a path $P$ in $G$ is called an **alternating path** if edges in $M$ alternate with edges not in $M$
- An alternating path is called an **augmenting** path for matching $M$ if it starts and ends at distinct free vertices

![[Pasted image 20240219174105.png]]

>[!note] Theorem (Berge)
>A matching $M$ is a maximum matching iff there is no augmenting path with respect to $M$

**Central task**: How to find an augmenting path?

![[Pasted image 20240226171525.png]]

- Construct an **alternating tree** $T$(Starting in a **free vertex**)
	- First go to vertices which you can get to by following paths **not in the matching** (necessarily, since the starting vertex is **free**)
	- Then go to a vertex by following a path that is **in the matching**
	- etc.
3 cases when encountering next vertex ($y$):
- **Case 1** - you **find** another **free vertex** not contained in $T$, an **augmenting path** is found
	- First one found will be the shortest one
- **Case 2** - $y$ is a matched vertex that is **not contained in $T$**
- **Case 3** - $y$ is **already contained** in $T$ as an **odd** vertex
	- **Ignore** successor $y$
	- ![[Pasted image 20240519163602.png]]
- **Case 4** - $y$ is **already contained** in $T$ as an **even** vertex
	- More complex case - we can go here but with special cases
	- The issue here is that once we go here, we will have a **cycle** of **odd length**
		- This can't happen in a bipartite graph since each side has an even number of edges
		- ![[Pasted image 20240520205847.png]]
	- Another example that **doesn't include the root of the tree**
		- ![[Pasted image 20240520210524.png]]
	- These are called **blossoms** 
	- $w$ is the least common ancestor of $x$ and $y$ - **base** of the blossom
		- It is necessarily at an even level
	- We want to **shrink** these blossoms
	- ![[Pasted image 20240520211244.png]]
	- ![[Pasted image 20240520211205.png]]
	- Edges that **were** in the tree are still in the tree
	- Edges that **weren't** in the tree are still **not** in the tree
	- Nodes that are connected in $G$ to at least **one node** in $B$ become connected to $b$ in $G'$
- Now, once we shrink a blossom like this, we carry on and can find an **augmenting path**
	- ![[Pasted image 20240520211541.png]]
	- This is because it's an odd cycle, so you can go from either direction in the cycle and get to the next node in the path either from an **in** vertex **or** an **out** vertex, so can treat the vertex as any of them!
	- **Any** **alternating path** starting from the blossom can become an **augmenting path**
	- ![[Pasted image 20240520212111.png]]
- 

Constructing an alternating tree takes $O(n+m)$ time
- This immediately gives an $O(n(n+m))$ time algorithm to find an augmenting path
	- (Build an alternating path starting at any vertex)
It follows that we can get an algorithm for max matching in $O(n^{2}(n+m))$ 
- (Construct an augmenting path and augment it $n$ times)
**But we want to do better**
### Faster Max Matching in bipartite graphs
Focus on **bipartite graphs** so we don't have to consider **case 4**

**Theorem**:
- Let $G$ be a graph, $M$ be a matching of $G$, and let $u$ be a **free vertex** w.r.t. $M$
- Further, let $P$ denote an **augmenting path** w.r.t $M$ and let $M' = M \oplus P$ denote the matching resulting from augmenting $M$ with $P$
	- Remember, $M \oplus P$ denotes the matching, and within the matching you **invert** the edges on the path $P$
	- i.e. for each edge in $P$, if it was **in** the matching, it is now **out** and vice versa
- If there is **no augmenting path** starting at $u$ in $M$ then there is **no augmenting path** starting at $u$ in $M'$ **also**

### Hopcroft-Karp-Karzanov - Fast max-matching in bipartite graphs

**Lemma**:
Let $M*$ be a **maximum** matching and let $M$ be **any** matching in $G$. If the length of the **shortest augmenting path** in $M$ if $k$, then $|M*|-|M|\leq \frac{|V|}{k}$

#### Proof
- Start with any matching $M$
- There is also a maximum matching $M*$
- 
- ![[Pasted image 20240519180714.png]]
- Now we form the 'symmetric union' graph $M* \oplus M$
	- We get all of the edges that are either in $M$ or $M*$
	- ![[Pasted image 20240519180722.png]]
	- Every vertex has degree up to 2
		- Since the most edges going into a vertex are 1 from $M$ and 1 from $M*$
- All the connected components are **either paths or cycles**
- For every **cycle** the number of edges from $M$ and $M*$ is the same
- However for every **path** there are more in $M*$ than $M$
- The **difference** between the number of components in which there are more **blue** edges and the number of components in which there are more **red** edges is equal to the difference in edges between $M$ and $M*$


**The algorithm**:
We want to make sure that the **length** of the paths **grows** in each phase
- In each phase we construct a **maximal** set $\Pi$ of **disjoint augmenting paths** w.r.t. $M$
	- Denote $M \oplus \Pi = M \oplus (\oplus_{P\in\Pi}P)$
	- SO this denotes the **augmentation** of $M$ by all of these augmenting paths, $\Pi$
	- Where $\Pi$ is all of the **augmenting paths** that **do not share an edge**
	- Because they are disjoint, it doesn't matter what order we augment $M$ with
**Lemma**:
- If $k$ is the **length** of the **shortest augmenting path** w.r.t. $M$
- Let $\Pi$ be the maximal set of **shortest** disjoint augment paths w.r.t. $M$
	- (Each of the paths have length $k$)
	- Therefore, we cannot add another shortest augmenting path to it without breaking the clause of **disjointness**
- Then the length of the **shortest augmenting path** w.r.t. $M\oplus \Pi$ is $> k$

![[Pasted image 20240519182742.png]]

How to **efficiently** find a maximal set of shortest augmenting paths?
- We combine BFS-like and DFS-like approaches

For a bipartite graph $G=(L\cup R,R)$ and for a matching $M$, define a **directed graph**:

![[Pasted image 20240519194631.png]]

Where all edges **in the matching** go from **right to left**
- And the **other edges** from **left to right**

We will use this procedure to the **layered graph** $G^*_M$ that is constructed out of $G_M$
- Let $L^*$ be the set of **free vertices** in $L$
- Let $d:V \rightarrow N$ be the **distance** $d(v)$ from $v$ to the vertices in $L^*$
- Layered graph: ![[Pasted image 20240519194845.png]]

**Matching M**:

![[Pasted image 20240519194913.png]]

**Construct the directed graph and note the free vertices and their distances**

![[Pasted image 20240519195006.png]]

Follow the edges from them to the right - these all have distance = 1

![[Pasted image 20240519195030.png]]

Now we go back from them to the left - the new vertices we can reach have distance 2

![[Pasted image 20240519195049.png]]

Now carry on from these to the vertices on the right which have distance 3, and back to the left where only $v_1$ has distance 4

![[Pasted image 20240519195110.png]]

The other vertices have infinite distance as they are unreachable from here
(This has essentially been a BFS)

**Lemma**:
- Every path in $G^*_{M}$ that **starts** in $L^*$ is a **shortest path** in $G_M$

Remove the edge that connects the vertices with infinite distance
Now we use DFS approach
- Run DFS from a vertex in $L^*$ until you find a **first vertex** from $R^*$
	- Where $R^*$ are the free vertices on layer $k$
- **Remove** all visited vertices during DFS from $G^*_M$
- **Return** the path found

![[Pasted image 20240519195847.png]]
### Bipartite Perfect Matching
**Hall's (Marriage) Theorem**:
- A bipartite graph has a **perfect matching** iff for all sets $S \subseteq L$, the **size** of the **neighbours** of $S$ is greater than or equal to the **size of $S$**
Proved in Lecture:

![[Pasted image 20240519120945.png]]

### Vertex Cover
*Covers all the edges*
A **vertex cover** is a set $C$ of vertices such that all edges $e$ of $E$ are incident to at least one vertex of $C$
- In other words, there is no edge completely contained in $V/C$

The size of any **matching** is $\leq$ the size of any **vertex cover**
- Given any matching $M$, a vertex cover $C$ must contain at least one of the endpoints of each edge in $M$
- **Not possible** to have a vertex cover that is **smaller** than a matching
**Weak Duality**:
- Any **vertex cover** is **at least as large** as the **maximum size matching**

**Konig's Theorem**:
- For any **bipartite graph**, the **maximum** size of a **matching** is equal to the **minimum** size of a **vertex cover** 
### Weighted Bipartite Matching
Input: Undirected, **bipartite** graph $G=(L\cup R,E)$
- An edge $e=(l,r)$ has weight $w(e)\geq 0$
Find a matching of **maximum weight**

**Simplifying assumptions:**
- Assume that $|L|=|R|=n$
- Assume that there is an edge between **every pair of nodes** $(l,r)\in L\times R$
	- This can be assumed since the edges that don't *actually appear* can be considered to have weight $w(e)=0$

Sometimes called the **assignment problem**, can minimise or maximise the total cost

**Idea**:
- Introduce the notion of **node-weighting** $\bar{x}$
- For any edge $e$, the sum of the weights of the vertices on each end of it must be at least the weight of the edge
	- For an edge $e=(u,v)$:
	- $x_{u}+x_{v}\geq w(e)$
		- Known as the node weights **dominate** the edge weights
	- **Tight edges**:
	- $x_{u}+x_{v}= w(e)$
- Let $H(\bar{x})$ denote the subgraph of $G$ that only contains edges that are **tight** (node weights add to **exactly** edge weight) w.t.r. the node weighting $\bar{x}$
- Try to compute a perfect matching in the subgraph $H(\bar{x})$
	- This will be a maximum weight matching in the graph $G$
	- If you succeed, the weight of your matching is equal to the total node weight $X = \Sigma_{v\in V} x_v$
- Any other matching $M$ has the following bound for its weight:
	- $\Sigma_{(u,v)\in M} w(u,v) \leq \Sigma_{(u,v)\in M}(x_{u}+x_{v})\leq X$ - **Optimal!**
	
**Example of a possible node-weighting**:

![[Pasted image 20240519152404.png]]

**Better weighting**:

![[Pasted image 20240519152415.png]]

**How to find** the **best** node weighting?
- Have some assignment of node weights and keep improving this assignment
Following diagram represents all **tight** edges connected to each other between $L$ and $R$

![[Pasted image 20240519202338.png]]

**Because** we don't have a perfect matching, we know by Hall's Theorem that the size of $|\Gamma(S)| < |S|$
- This means that for every node in $L$, **decrease** the node-weight by $\delta$ and for every node on the right, **increase** the node-weight by $\delta$
- We know that since $|\Gamma(S)| < |S|$, the total of all of the node weights will **decrease**

**But** what do we choose for $\delta$?
- We want it to be as large as possible, but within some limitations
- $\delta$ is limited by:
	- The **minimum** node-weight in $L$
		- We don't want any of the node-weights to be **negative**
	- The **dominance condition**
		- i.e. since our graph currently only shows **tight** edges, we can increase and decrease the node weight up to a point - but if at a point we may break this dominance for some other edge that becomes tight within the graph

Suppose we have the following bipartite graph with node weightings:

![[Pasted image 20240520160200.png]]

How big can $\delta$ be?
- 1, since cannot be 2 due to if we choose 2 then we will break dominance between 5 and 5'

![[Pasted image 20240520160429.png]]

Tight edges remain tight, but now we have some new tight edges also

Now can't choose 2 as there are two nodes of weight 1 on the left:

![[Pasted image 20240520160553.png]]

Now we have a **perfect matching!**
- By including the edges of weight 0 in the graph

Since **perfect matching of** **tight edges**, we have a matching of largest weight

How to **find** $S$?
- Make the graph with a matching directed by making all edges in matching go left, and other edges go right

![[Pasted image 20240520172940.png]]

Using a matrix example:

![[Pasted image 20240520173216.png]]

Initialise all 0's for nodes on the Right hand side
Initialise all Max-values for the Left hand side

![[Pasted image 20240520173343.png]]

Identify tight edges - where edge weight is equal to the sum of the value in row and column

![[Pasted image 20240520173519.png]]

Do we have a perfect matching consisting only of tight edges?
- No - find a set $S$ and $\Gamma(S)$ where $|S| > |\Gamma(S)|$

![[Pasted image 20240520173716.png]]

Here, look at columns where the number of tight edges corresponding to a number of columns is larger than that number of columns
- Tight edges in both 1 and 2 correspond to just on column - B

Now, **decrease** the weights of the **rows** in $S$ and **increase** the weights of the **columns** in $\Gamma(S)$
- By **how much** can we change this?
- 7

![[Pasted image 20240520174318.png]]

Notice that after changing this, the edge from 4 to B is no longer tight!

We again don't have a perfect matching - no tight edge going out of 4
- Now we can take as $S$ just vertex 4 
- $\Gamma(S)$ is empty

By how much can we reduce the weight of this node?
- 7 - since 7 + 11 = 18 so this edge will once again be tight

![[Pasted image 20240520174756.png]]

Now we once again don't have a perfect matching

Nodes 2 and 4 only have 1 neighbour
Nodes 2, 3 and 4 only have 2 neighbours

How much can we reduce weights by
- 2 - Any more would make 16 dominate the edge 3 - D
- This makes that edge tight

![[Pasted image 20240520175125.png]]

Now once again no perfect matching
- 2, 4 only have 1 neighbour - B

How much to reduce by?
- 9

![[Pasted image 20240520175612.png]]

Now we have a perfect matching!

1-A, 2-B, 3-C, 4-E, 5-D

Remember that originally the graph has been transformed

From each edge take 22 and reverse sign - so 22 + 22 + 2 + 4 + 7