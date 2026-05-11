# Planar Separator Theorem (Lipton-Tarjan)

---

**Key Idea:** Efficiently partition planar graphs into roughly balanced pieces by removing few vertices.

**Theorem (Formal):** Every planar graph G=(V,E) with ∣V∣=n has a partition V=A∪B∪S, where:
- **Separator:** ∣S∣=O($\sqrt n​$)
- **Balance:** ∣A∣,∣B∣≤$\frac{2}{3}$​n
- **No Cross-Edges:** No edges exist between A and B.
## Significance

- **Divide-and-Conquer:** Foundation for efficient algorithms on planar graphs. Recursively solve subproblems on smaller pieces, combine solutions.
- **Theoretical Insight:** Reveals deep structural property of planar graphs, not shared by general graphs.
- **Practical Applications:**
    - **VLSI layout:** Minimize wire length, congestion.
    - **Geographic Information Systems (GIS):** Spatial partitioning, clustering.
    - **Graph drawing:** Minimize crossings, area.

# Max Matching using Planar Separator

**Key Idea:** Leverage the divide-and-conquer nature of the Planar Separator Theorem to efficiently compute maximum matchings in planar graphs

**Problem:** Given a planar graph $G=(V,E)$, find a maximum matching, i.e., a largest set of edges with no shared endpoints

## Algorithm Sketch

1. **Base Case:** If G is small (e.g., fewer than $k$ vertices for some constant $k$), find the maximum matching directly using any standard algorithm
2. **Divide:** Use the Planar Separator Theorem to find a separator $S$ that divides $G$ into pieces $A$, $B$, and $S$ as described earlier
3. **Conquer (Recursive Calls):**
    - Recursively find maximum matchings $M_A​$​ and $M_B​$​ in the subgraphs induced by $A$ and $B$, respectively
4. **Combine:**
    - Consider each edge $e∈S$
    - If $e$ **does not** connect two vertices already matched in $M_A​$ or $M_B$​, **add** $e$ to the matching
    - If e **connects** two vertices already matched, explore the following options:
        - **Augmenting Path:** If there exists an augmenting path (alternating unmatched and matched edges) starting from one of e's endpoints, modify $M_A$ or $M_B​$​ to include this path, increasing the matching size by 1.
        - **Alternative Matching:** If no augmenting path exists, try removing e and exploring other matching possibilities for vertices in $S$
5. **Return:** The combined matching is a maximum matching in $G$

## Analysis

- **Correctness:** The algorithm guarantees a maximum matching because it explores all possible matching options across the separator $S$ and the subgraphs $A$ and $B$
    
- **Time Complexity:** The recursion depth is $O(logn)$ due to the balanced nature of the separator. At each level, we perform work proportional to the size of the separator (O($\sqrt n​$), and potentially augmenting path searches, which can be done in linear time. Therefore, the overall time complexity is $O(nlogn)$

![[Pasted image 20240520231636.png]]

# Minimum Vertex Cover

**Key Idea:** Utilize the Planar Separator Theorem's divide-and-conquer approach to efficiently compute minimum vertex covers in planar graphs.

**Problem:** Given a planar graph G=(V,E), find a minimum vertex cover, i.e., a smallest set of vertices such that every edge in G is incident to at least one vertex in the set.

## Algorithm Sketch

![[Pasted image 20240520233819.png]]

![[Pasted image 20240520234040.png]]

![[Pasted image 20240520234051.png]]

![[Pasted image 20240520234136.png]]

![[Pasted image 20240520234210.png]]

![[Pasted image 20240520234306.png]]

![[Pasted image 20240520234325.png]]

![[Pasted image 20240520234342.png]]

1. **Base Case:** If G is small (e.g., fewer than k vertices for some constant k), find the minimum vertex cover directly using any standard algorithm.
2. **Divide:** Use the Planar Separator Theorem to find a separator S that divides G into pieces A, B, and S as described earlier.
3. **Conquer (Recursive Calls):**
    - Recursively find minimum vertex covers $C_A$​ and $C_B​$ in the subgraphs induced by A and B, respectively.
4. **Combine:**
    - Initialize the vertex cover C as $C_A​∪C_B​∪S$. This is a valid vertex cover, but it may not be minimal.
    - Consider each vertex $v∈S$.
    - If removing v from C still leaves a valid vertex cover (i.e., all edges incident to v are covered by other vertices in C), remove v from C.
    - Repeat for all vertices in S.
5. **Return:** The resulting set C is a minimum vertex cover in G.
## Analysis

- **Correctness:** The algorithm guarantees a minimum vertex cover because it starts with a valid vertex cover and iteratively removes vertices from the separator S that are redundant. Since the separator is small, the number of redundant vertices is limited, ensuring the resulting vertex cover is minimal.
    
- **Time Complexity:** Similar to the maximum matching algorithm, the recursion depth is O(logn). At each level, we perform work proportional to the size of the separator (O($\sqrt n​$)) to check for redundancy.