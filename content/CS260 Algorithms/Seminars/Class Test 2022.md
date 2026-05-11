# 1

![[Pasted image 20231124200322.png]]

![[Pasted image 20231124200332.png]]

{1, 3, 5, 7, 8}

![[Pasted image 20231124200429.png]]

Largest size - 3
e.g. {1, 4, 6}

![[Pasted image 20231124200557.png]]

Greedy:
Start with first, look for next which is >G, look for next etc.

```
dist = d[1]
locations = [1]
numLocations = 1
for i from 2 to n:
	if D[i] > D[locations[numLocations-1]]+G:
		numLocations+1
		locations.append(i)
		dist = d[i]
```

![[Pasted image 20231124231956.png]]

Contrapositive: If there is a poly-time algorithm for `SPARSE-SET` then there is one for `VERTEX-COVER`
Reduction from `VERTEX-COVER` to `SPARSE-SET`

Use $|V|$ and $|E$| as inputs to `SPARSE-SET` and use the oracle with this as the answer
