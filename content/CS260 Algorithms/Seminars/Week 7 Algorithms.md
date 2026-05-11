# 1
![[Pasted image 20231117160230.png]]

![[Pasted image 20231117160240.png]]

- {1, 3, 4} is **not** an independent set since there is an edge {3, 4}
- Largest independent set is {1, 3, 5}
- Since 2 nodes that aren't connected in this graph (independent) means that they **are** compatible (since if incompatible connected by a node):
	- No larger set of pairwise compatible intervals can exist since then there would be another 2 nodes in the largest independent set as they would not be connected to each other or any other node already in the set.

![[Pasted image 20231117162209.png]]

- No, {1, 3, 4} is not a vertex cover is the edge {2, 5} does not have an endpoint at any of these edges
- The smallest vertex cover is {2, 4}

![[Pasted image 20231117162419.png]]

- Create an undirected graph with nodes for the intervals and edges between **incompatible** intervals
- Find the smallest **vertex cover** for the graph
- Return the nodes of the graph - the vertex cover
# 2
![[Pasted image 20231117170325.png]]

Make an algorithm `MAX-INDEPENDENT-SET (Value)` which returns the **value** of the **maximum size** independent set of the graph, by keeping on adding vertices and checking if an independent set exists by running `INDEPENDENT-SET`
- Start with whole graph
- Initially calc. max independent set size using oracle
- Go through the vertices and check:
	- If you exclude it, does the size of the max independent set change?
		- If yes, it is **in** the max independent set
		- If no, throw it out - it is **not** in the max independent set
- (only $n$ checks, so clearly polynomial time reduction)

