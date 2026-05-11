# 1

![[Pasted image 20231201171253.png]]

Reduce 3-SAT to 2LIT-3SAT which transitively means 4-D-IND-SET is also NP-Hard
Inductive Proof:
# 2

![[Pasted image 20231201170915.png]]

Show in NP
Given a MST $T$ -
- Verify that $T$ a spanning tree
	- Linear time - DFS
- $C(T)=B$

Show SUBSET-SUM Reduces to EXACT-SPANNING-TREE in poly time
So $S-S\leq _{p}E-S-T$

![[Pasted image 20231201172613.png]]

# 3

![[Pasted image 20231201172630.png]]

![[Pasted image 20231201172746.png]]

Certifier: 
- MST - check if it fulfils properties of MST and every node has degree $\leq k$

![[Pasted image 20231201173226.png]]

- Just Hamiltonian path problem - so NP-complete

![[Pasted image 20231201173248.png]]

Reduce 2-S-T to 3-S-T 
2-S-T $\leq _p$ 3-S-T

Suppose we have graph $G$
Add a dummy node and edge to every node in the graph - $G'$
So every node must connect to each of these dummy nodes in MST
All other nodes can have degree at most 2 (not counting dummy nodes) so this is 2-S-T if not including $G'$ so equivalent.

