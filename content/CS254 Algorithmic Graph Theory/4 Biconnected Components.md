- An undirected graph $G$ is **biconnected** if $G \backslash \{v\}$ is connected for all $v\in V$
- A **cut-vertex** or **articulation point** is a vertex whose removal **disconnects** the graph
- A **maximal** biconnected subgraph of a graph $G$ is called a **biconnected component** of $G$

Equivalence relation:
- CCs and SCCs are the equivalence classes of certain equivalence relations
- Biconnected components can't be - they overlap with each other

However we can use edges:
- Let ~ be the relation on $E$ s.t. $e_{1}$~$e_2$ iff $e_{1}$ and $e_{2}$ are contained in a **simple cycle** in $G$ **or** $e_{1} =e_2$
- ~ is an equivalence relation

![[Pasted image 20240129172248.png]]

![[Pasted image 20240129172353.png]]

When is a **leaf node** an articulation point?
- A leaf can **never** be an articulation point

When is the **root node** of the DFS-tree an articulation point?
- If the **root node** has **at least 2 children** then it will always be an articulation point
	- Since **cross edges** cannot exist (between a vertex in a left subtree and right subtree) in an undirected graph's DFS tree

When is an **internal vertex** $v$ an articulation point?
- If there is a **child** $u$ of $v$ such that no edges connect a **node** from the sub-tree $T_u$ rooted at $u$ to an **ancestor** of $V$

The **low-point** of a node $v$ in a DFS-tree is the **lowest level** among the neighbours of nodes in the sub-tree $T_v$ rooted at $v$
- Low point of node 4 is 2, because node 7 is in the sub-tree of 4 and it is connected to node 3 which is at level 2

Algorithmic idea for finding all articulation points:
- Compute a **DFS-tree**
- Compute for **each node** its **level** in the DFS-tree
- Compute for **each node** its **low-point**
- Check for **each internal node** $v$ in the DFS-tree, whether the **low-point** of one of its **children** is **larger** or **equal** to $v$’s level
	- If this is true, $v$ is an articulation point; otherwise not
