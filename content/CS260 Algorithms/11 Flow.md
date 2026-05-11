>[!note] Flow Networks
>A **Flow Network** is a **5-tuple** $(V,E,s,t,c)$ where:
>- $V$ is a set of **vertices**
>- $E$ is a set of **edges** (order *ordered pairs*)
>- $s\in V$ is the **source** vertex
>- $t\in V$ is the **sink** vertex
>- $c:E\rightarrow \mathbb{R}^+$ is the **capacity function**

![[Pasted image 20231123172005.png]]

**Intuition**: $s$ is a **water tap**, $t$ is a **drain**, and the **edges** are **pipes** of varying cross-sectional area

A **flow** is an assignment of a nonnegative **real number** $f(e)$ to **each edge** $e\in E$, such that:
- $f(e)\leq c(e)$ for all $e \in E$; **and**
- for all $v\in V \ \{s,t\}$, the **flow conservation** condition holds:

![[Pasted image 20231123172256.png]]
#### Minimum Cut

>[!note] Minimum Cut
>An $s-t$ **cut** is a partition $(A,B)$ of $V$ such that $s\in A$ and $t\in B$.
>The **capacity** of an $s-t$ cut $(A,B)$ is the sum of the capacities of the edges from $A$ to $B$:
>
>![[Pasted image 20231123172719.png]]

>[!note] The `MIN-CUT` problem
>Given a flow network $N=(V,E,s,t,c)$, find an $s-t$ cut $C$ such that the **capacity** of $C$ is **minimal** for $N$
### Maximum-Flow
The **value** of a flow $f$ is the **sum** of the **net flow** out of $s$:
- (= flow **into $t$**)

![[Pasted image 20231123173459.png]]

>[!note] The `MAX-FLOW` problem
>Given a flow network $N=(V,E,s,t,c)$, find an $s-t$ cut $C$ such that the **capacity** of $C$ is **maximal** for $N$

How to solve `MAX-FLOW`?
#### Greedy Algorithm
- Start with $f(e)=0$ for each edge $e\in E$
- Find an $s\rightarrow t$ path $P$ where each edge has $f(e)<c(e)$
	- Choose any path with **remaining capacity** on **every edge**
- **Augment** flow along path $P$
	- **Increase** the flow along this path by the **minimum remaining capacity**
- **Repeat** until you get stuck

![[Pasted image 20231127141300.png]]

The **problem**:
- We might choose bad paths
#### Residual Network

![[WhatsApp Image 2023-11-23 at 17.51.54_81535eaf.jpg]]

**Idea**: Track used capacity so that we can **undo** bad choices while increasing overall flow
- The residual network contains, for **each edge** in the flow network:
	- A **forward edge** with **remaining** capacity
	- A **reverse edge** with **used** capacity

![[Pasted image 20231127141928.png]]

![[Pasted image 20231127141937.png]]

#### The **Ford-Fulkerson** algorithm:
- Start with a flow 0 everywhere in $N$
- While there exists some augmenting path $p$ from $s$ to $t$ in the **residual network** of $N$
	- Let $c$ be the **minimum edge weight** along $p$
	- For each **forward edge** taken in $p$, **increase** the flow along that edge in $N$ by $c$
	- For each **reverse edge** taken in $p$, **decrease** the flow along that edge in $N$ by $c$
	- **Update** the residual network accordingly

**Correctness**:
If there is an **augmenting path** from $s$ to $t$ in the **residual network**, then either:
- It uses **no** reverse edge, so directly corresponds to augmentation of the flow network
- It uses at least **one** reverse edge, which means that we **reduce** flow along some edge(s) and reallocate it to other edges
	- Since the chosen path was augmenting, it **must** still increase flow overall

**How long** does it take?
- Finding an augmenting path is $O(|E|)$ (via BFS or DFS)
- If weights are integers, then the maximum flow is at most the sum of the capacities, and all augmenting paths **increase** flow by at least 1
- So the algorithm **terminates** in $O(|E|\times \Sigma c)$
If weights are **not integral** (real valued) - algorithm might never terminate!
### Net Flow

>[!note] Net Flow
>The **net flow** across an $s-t$ cut $(A,B)$ is the sum of the flow values of all edges from $A$ to $B$, **minus** the sum of the flow values of all edges from $B$ to $A$

**Lemma**: Let $f$ be any flow for $N$ and let $(A,B)$ be any $s-t$ cut in $N$. Then, the value of the flow $f$ equals the net flow across the cut $(A,B)$

What is the net flow across the black and white cut?

![[Pasted image 20231127143728.png]]

20 - 8 - 4 - 4 + 22 = 26

Proof of the **lemma**:

![[Pasted image 20231127144001.png]]

Max-Flow-Min-Cut Theorem

![[Pasted image 20231127144615.png]]



## Bipartite Matching

>[!note] Matching
>A **subset** of edges such that every vertex is paired with another vertex in the graph

>[!note] Bipartite Matching
>A **max-cardinality** matching in a graph that is **bipartite**

We can **solve** the bipartite matching problem using **Flow**
### Max-flow formulation

- Create **digraph** $G' = (L\cup R\cup\{s,t\},E')$
- Direct all **edges** from $L$ to $R$ and assign **infinite** (or **unit**) capacity
- Add unit-capacity edges from $s$ **to** **each node** in $L$
- Add unit-capacity edges from **each node** in $R$ **to** $t$

![[Pasted image 20231221195520.png]]

**Theorem** - 1-1 correspondence between matchings of cardinality $k$ in $G$ and integral flows of value $k$ in $G'$

**Proof** $\rightarrow$
- Let $M$ be a matching in $G$ of cardinality $k$
- Consider flow $f$ that sends 1 unit on each of the $k$ corresponding paths
- $f$ is a flow of value $k$

**Proof** $\leftarrow$
- Let $f$ be an integral flow in $G'$ of value $k$
- Consider $M$ = set of edges from $L$ to $R$ with $f(e)=1$
	- Each node in $L$ and $R$ participates in **at most** one edge in $M$
	- $|M|=k$: apply flow-value lemma to cut $(L\cup \{s\},R\cup \{t\})$

![[Pasted image 20231221195716.png]]

### Perfect matching

>[!note] Perfect matching
>A subset of edges is a **perfect matching** if each node appears in **exactly one edge** in $M$

In a bipartite graph $|L|=|R|$
- Which other conditions are necessary?

let $N(S)$ be the set of nodes **adjacent** to nodes in $S$

![[Pasted image 20231221200500.png]]

#### Halls's marriage theorem
$G$ has a perfect matching **iff** $|N(S)|\geq |S|$ for all subsets $S\subseteq L$

![[Pasted image 20231221200624.png]]


## Multi source and multi target to single source and single target reduction

![[Pasted image 20231221203419.png]]

![[Pasted image 20231221203429.png]]

![[Pasted image 20231221203438.png]]

![[Pasted image 20231221203453.png]]

![[Pasted image 20231221203501.png]]

![[Pasted image 20231221203506.png]]
