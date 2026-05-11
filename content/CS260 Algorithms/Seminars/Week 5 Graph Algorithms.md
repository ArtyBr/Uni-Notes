1 - No, paths with more edges have their shortest paths swayed much more than those with less edges as the compound of the constant added to all edges adds up for all these edges

2 - 
a) nothing else matters but the ordering, so no matter what you do to the weights, as long as the order of the weights stays the same the MST will stay the same after.
b) 

3

![[Pasted image 20231103171946.png]]

Look at:
Is there a vertex with < 2 neighbours? Drop them
Is there a vertex with > n-2 neighbours? Drop them

$f(V)$ - largest good party
$u$ in $V$ - an extreme node (knows $>2$ people or $> n-2$ people)
$f(V) - f(V/u)$

Now to make it optimal:
- Make a bucket for every degree for each node possible
- Count the degree for each node and add this node to the corresponding number of bucket
- Delete the vertices that are in buckets $0, 1, n-1, n-2$
- How to delete?
	- Upon initialisation, every vertex has a pointer to its location in its bucket
	- Say using a linked list
	- Go through all neighbours of the node you're deleting
	- For each one move it to bucket number $i-1$
4
![[Pasted image 20231103173506.png]]

| iteration  | $s$ | $A$ | $B$    | $C$    | $D$ | $E$   | $F$    | $G$    |
| --- | --- | --- | ------ | ------ | --- | --- | ------ | ------ |
| 1   | 0   | 1   | $\inf$ | $\inf$ | 4   | 8   | $\inf$ | $\inf$ |
| 2   | -   | 1   | 3      | $\inf$ | 4   | 7   | 7      | $\inf$ |
| 3   | -   | -   | 3      | 4      | 4   | 7   | 5      | $\inf$ |
| 4   | -   | -   | -      | 4      | 4   | 7   | 5      | 8      |
| 5   | -   | -   | -      | -      | 4   | 7   | 5      | 8      |
| 6   | -   | -   | -      | -      | -   | 6   | 5      | 6      |
| 7   | -   | -   | -      | -      | -   | 6   | -      | 6      |
| 8   | -   | -   | -      | -      | -   | -   | -      | 6      |
|     |     |     |        |        |     |     |            |     |     |        |        |     |     |        |        |

