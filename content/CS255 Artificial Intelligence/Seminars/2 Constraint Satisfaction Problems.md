# 1 

![[Pasted image 20231030171830.png]]

**Minimum Remaining Values** - Choose the variable that has the fewest amount of legal values, meaning it is the highest likelihood to fail
**Degree heuristic** - This is a tie-breaker for MRV
- Choose the variable with the most constraints on remaining variables
- Attempts to reduce the branching factor in future
**Least constraining value** - Given a variable, choose the value that will rule out the least amount of remaining variables
# 2

![[Pasted image 20231101120358.png]]

# 3

![[Pasted Image 20231101120652_312.png]]

a)

| Arc     | Relation | Value(s) removed |
| ------- | -------- | ---------------- |
| <$A,B$> | $A>B$    | $A=1$            |
| <$B,A$> | $B<A$    | $B=4$            |
| <$A,D$> | $A<D$    | $A=4$            |
| <$D,A$> | $D>A$    | $D=1, D=2$       |
| <$B<A$> | $B<A$    | $B=3$            |
| ...     | ...      | ...                 |

![[Pasted image 20231102154612.png]]

# 4

![[Pasted image 20231102154914.png]]

![[Pasted image 20231102154901.png]]
# 5

![[Pasted image 20231102155041.png]]

![[Pasted image 20231102155049.png]]

