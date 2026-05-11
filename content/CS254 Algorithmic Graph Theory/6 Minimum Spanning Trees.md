A spanning tree of $G=(V,E)$ is a **spanning subgraph** of $G$ that is a **tree**

![[Pasted image 20240205170922.png]]

- A spanning tree is a connected subgraph with $|V|-1$ edges

A **minimum spanning tree** is a spanning tree of a **directed graph $G$** that has the **minimum weight**
### **Meta-algorithm** for MST
Can be described as an edge colouring algorithm:
- **Included** edges are coloured *blue*
- **Excluded** edges are coloured *red*

Builds the spanning tree edge by edge:
- Either by including a **blue edge**
- or by excluding an appropriate **red** one
until a **tree** is built

Key 'colouring' invariant of the algorithm:
- There exists an MST containing all of the blue edges and none of the red ones
- Must apply at all points during the run of the algorithm

Once we have an algorithm satisfying the property above, after colouring all the edges of $G$, the **blue** edges form an MST
#### Blue Rule

![[Pasted image 20240205171901.png]]
#### Red Rule

![[Pasted image 20240205172229.png]]
#### Examples of Meta Algorithms
OG Meta algorithm:

![[Pasted image 20240205172617.png]]

**Theorem**
- The meta-algorithm is correct. That is, if $G$ is a connect undirected graph, then the meta-algorithm returns a minimum spanning tree of $G$

**Kruskal's algorithm**

![[Pasted image 20240206152917.png]]

Theorem about meta-algorithm ensures that this algorithm is correct
#### Proof of correctness of Meta-algorithm:
It is sufficient to show:
- Meta-algorithm satisfies our **colouring invariant**
- Meta-algorithm terminates (it colours all edges)

>[!note] Colouring Invariant
>There exists an MST containing **all** of the **blue** edges and **none** of the **red** ones

Initially **no edge is coloured** and any connected undirected has an MST
- Invariant is **satisfied** at the beginning of algorithm

![[Pasted image 20240205173254.png]]

![[Pasted image 20240205173303.png]]
### Union-Find
What do we need to implement the meta-algorithm?

Useful **data structure**: `UNION-FIND`

**Goal**: Maintain a collection $S=\{s_{1},...,s_k\}$ of disjoint dynamic sets supporting the following three operations
- **MAKE-SET($x$):** Creates a new set whose only member is $x$
- **UNION($x,y$):** Unites the dynamic sets that contain $x$ and $y$ into a new set that is the union of these two sets
- **FIND($x$):** Returns a pointer/representative of the set containing $x$

We'll typically need $O(n)$ **UNION** and $O(m)$ **FIND** operations

**Applied to our meta-algorithm**:
- Each set $s_{i}$ contains vertices from the same “**blue tree**” 
- Two vertices are in the same blue tree if their representatives are the same 
- Updating the sets: 
	- **Blue** rule: **merge** two sets (**trees**) into one 
	- **Red** rule: do **nothing**

For Kruskal's:

![[Pasted image 20240206155138.png]]

#### Characteristic Vectors
Simplest implementation of union-find data structure uses **characteristic vectors**

![[Pasted image 20240206155213.png]]

**Theorem**: Characteristic vectors implementation requires:
- $O(n)$ time for **initialisation**
- $O(1)$ time for each operation **MAKE-SET($i$)**
- $O(1)$ time for each operation **FIND($i$)**
- $O(n)$ time for each operation **UNION($i,j$)**

![[Pasted image 20240206155412.png]]

#### Tree Structure
More common/better implementation uses a **tree structure**:
We will have a collection of (abstract) **trees** such that:
- Each set corresponds to a tree:
- $x$ and $y$ belong to the same tree iff $x,y$ belong to the same set $s_{i}$
- **Representative** of a set = the **root** of the corresponding tree
- Each node has a link to its **parent** in the tree
- Parent of a root is the node itself

![[Pasted image 20240206160126.png]]

![[Pasted image 20240206161145.png]]

Even thought this implementation may look very promising, it's **inefficient**

![[Pasted image 20240206161540.png]]

![[Pasted image 20240206161628.png]]
#### Heuristics

**Path compression**
- Each time we perform **FIND($x$)**, we change the **parent** link for all nodes on the path from $x$ to the root to point to the root of the tree

![[Pasted image 20240206162524.png]]

2 heuristics for **unions**
- **Weight union rule**
	- In **UNION($x,y$)**, let the **number of nodes** in the tree containing $x$ be larger than or equal to the number of nodes in the tree containing $y$;
	- Set **PARENT(FIND($y$))=FIND($x$)**
- **Height union rule**
	- In **UNION($x,y$)** let the **height of the tree** containing $x$ be larger than or equal to the height of the tree containing $y$
	- Set **PARENT(FIND($y$))=FIND($x$)**

**Ackermann function**

![[Pasted image 20240206163755.png]]

**Theorem:**

![[Pasted image 20240206164037.png]]

So running time of Kruskal's:

![[Pasted image 20240206164909.png]]

### Prim's algorithm
Meta-algorithm:

![[Pasted image 20240212172051.png]]

Implementation using **adjacency list**:
- Easy to implement in $O(n(n+m))$ time
- Can we do better?

**Tool**:
- For any vertex $v\in V\backslash T$ we define:
	- $d(v)=min\{w(v,u):u\in T,(v,u)\in E\}$
		- **Cost** of the **lightest edge** between $v$ and $T$
	- $\pi(v)=u$ such that $w(v,u)=d(v),u\in T,(v,u)\in E$
		- **Endpoint** of the **lightest edge** between $v$ and $T$

![[Pasted image 20240212172534.png]]

Update function which updates values for $\pi(u)$ and $d(u)$ - but only when **needed**

![[Pasted image 20240212173409.png]]

$O(n^2)$

### Round Robin Algorithm

![[Pasted image 20240213150955.png]]

**Stage 1**
- Ends when the last element from the **original set $Q$** is deleted
**Stage $i$**
- Ends when the last element from $Q$ from the beginning of the stage is deleted

**Lemma**: Set entering stake $k$ have size $\geq 2^{k-1}$, produced in that stage have size $\geq 2^k$
**Corollary**: There are at most $log(n)$ stages

![[Pasted image 20240213153053.png]]

Can be improved to $O(mlog(log(n)))$
- Joked that it is essentially constant - even for n in the billions/trillions, loglogn is around 1-5
However, **prim's** is still better with Fibonacci heaps

However, for **sparse graphs** ($m \approx n$), we can do **better** with Round Robin

**Key properties**:
- We know that any simple graph has **at most** $3n$ edges
- Graph **contraction**
	- We contract every **blue tree** into a single "*Supervertex*"
	- Delete all edges between two vertices in the same tree
	- Multiple edges between trees $\rightarrow$ delete all but the lightest
**Claim**: A contraction of a simple planar graph gives a simple planar graph

**Modified round robin**: 
- **After each stage**, contract the graph

![[Pasted image 20240213154238.png]]

![[Pasted image 20240213154521.png]]

![[Pasted image 20240213154246.png]]

