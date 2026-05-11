We can improve on uninformed search using **problem specific knowledge** - informed search

**Important for exam!!!!**
- Make sure to learn **"A*"** and **"Depth-first Branch-and-Bound"**!!
## Heuristic Search
- **Idea**: Don't ignore the goal when selecting paths
- **Heuristics**: Extra knowledge that can be used to guide the search
- $h(n)$ is an **estimate** of the cost of the shortest path from node $n$ to a goal node
- $h(n)$ needs to be be **efficient** to compute
- $h$ can be extended to **paths**: $h(<n_0, ..., n_k>)=h(n_k)$
- $h(n)$ is an **underestimate** if there is no path from $n$ to a goal with cost strictly less than $h(n)$
- An **admissible heuristic** is a **non-negative** heuristic function that is an **underestimate** of the actual cost of a path to a goal
	- $h(n)$ is admissible if it is **always** *less than or equal* to the **actual** cost of a lowest-path cost from $n$ to a goal

We can use the heuristic function to determine the **order** of the stack/queue representing the frontier

A search algorithm is **admissible** if, whenever a solution **exists**, it returns an **optimal** solution
### Best-first Search
Uses **just** heuristic values of nodes on the frontier

**Idea**: Select the path or node that is closest to a goal according to the heuristic function
- **Heuristic depth-first search**: Selects a **neighbour** so that the **best** neighbour is selected first (that which appears closest to the goal)
	- Selects the **locally** best path, and explores all paths from the selected path before exploring elsewhere
	- Has the **same** problems as depth-first search (may not find solution and may not find optimal solution)
- **Greedy best-first search**: Selects a **path** on the frontier with the **lowest** heuristic value 
	- Selects a path, and then **doesn't backtrack** from this path later down the line
	- Doesn't re-order nodes after choosing path from frontier
	- Follows paths that seem promising, but paths may get long/costly quickly
- Best-first search treats the frontier as a **priority queue** ordered by $h$
#### Complexity
- Space complexity is **exponential** in path length: $b^n$
- Time complexity is **exponential**: $b^n$
- **Not** guaranteed to find a **solution**, even if one exists
- Does **not** always find the **shortest** path
### A* Search
A* search uses **both** path cost **and** heuristic values
- $cost(p)$ is the cost of path $p$ (also referred to a $g(p)$)
- $h(p)$ estimates the cost from the end of $p$ to a goal
Mix of **lowest-cost-first** and **best-first** search
- It treats the frontier as a priority queue ordered by $f(p)$
$$f(p) = cost(p) + h(p)$$
- It always selects the path on the frontier with the lowest estimated distance from the start to a goal node constrained to go via that node

A* is admissible if:
- The branching factor is **finite**
- Arc costs are **bounded** above zero (there is some $\epsilon>0$ such that all of the arc costs are greater than $\epsilon$)
- $h(n)$ is **admissible**, i.e. nonnegative and an underestimate of the cost of the shortest path from $n$ to a goal node

A* can **always** find a solution if there is one:
- The frontier always contains the **initial** part of a path to a goal, before that goal is **selected**
- A* **halts**, since the costs of the paths on the frontier keep **increasing**, and will eventually **exceed** any finite number
##### Good heuristics matter
Suppose $c$ is the cost of an **optimal solution**
- A∗ expands **all** paths from the start in the set $\{p : cost(p) + h(p) < c\}$
- A∗ also likely expands **some** paths from the set $\{p : cost(p) + h(p) = c\}$
**Increasing** $h$ while keeping it admissible **reduces** the **size** of the first set
If the second set is **large** there can be significant **variability** in the space and time of A*
#### Complexity
**Time** - Exponential in relative error in $h \times$length of solution
**Space** - Exponential: keeps all nodes in memory (A*'s main problem')
### Pruning
**Cycle** pruning prunes for cycles **within a path** whereas **multiple path** pruning prunes for **duplicate paths** to the **same node**
#### Cycle Pruning
When searching, we can **rune** a path that ends in a node already on the path, without removing an **optimal solution**.
- Paths $⟨n_0,...,n_k,n⟩$ where n ∈ ⟨n_0,...,n_k⟩ are **not added** to the frontier when expanding the path.
- In **depth-first methods**, checking for cycles can be done in **constant time** in path length (e.g., using a hash function).
- For other methods, checking for cycles can be done in **linear time** in path length.

![[Pasted image 20231018100642.png]]
#### Multiple-Path Pruning
**Multiple path pruning**: prune a path to node n if the search has already found a path to n.
- The search maintains an explored set, called the **closed list**, of nodes at the end of **expanded** paths.
- The closed list is initially **empty**.
- When a path $⟨n_0,...,n_k⟩$ is selected, if $n_k$ is in the closed list, the path is **discarded**; otherwise, $n_k$ is added to the closed list, and the algorithm continues as normal.

![[Pasted image 20231018100809.png]]
#### Multiple-Path Pruning & Optimal Solutions
- No guarantee that the search does not prune a **least-cost path**.
- The problem is that a subsequently found path to n may be **lower cost** than the first found path to n.
- Possible **solutions**:
    - Ensure this does not happen — make sure that the **shortest path** to a node is found first.
    - Remove all paths from the frontier that use the **longer path**, i.e., if there is a path $p = ⟨s,...,n,...,m⟩$ on the frontier, and a path p′ to n is found with a **lower cost** than the portion of $p$ from $s$ to $n$, then $p$ can be removed from the frontier.
    - Change the **initial segment** of the paths on the frontier to use the **shorter path**, i.e., if there is a path $p = ⟨s,...,n,...,m⟩$ on the frontier, and a path $p′$ to $n$ is found with a **lower cost** than the portion of $p$ from $s$ to $n$, then $p′$ can replace the **initial part** of $p$ to $n$.
#### Multiple-Path Pruning & A*
Recall that **admissibility** does not guarantee that every path selected from the frontier is on an **optimal path**.
- This means that A* does not guarantee that the first path found to a node is the **lowest-cost path** to that node.
- However, we can make this the case by using a **consistent heuristic**
- A consistent heuristic is one that satisfies the **monotone restriction**: $h(n) ≤ cost(n,n′) + h(n′)$ for any arc $⟨n,n′⟩$
- ![[Pasted image 20231018100848.png]]
So a consistent heuristic would be one in which the naturally longer one ( $n\rightarrow n'\rightarrow g$ ) takes longer by the logic of the heuristic too
- ![[Pasted image 20231018100857.png]]
- Suppose path $p′$ to $n′$ was selected, but there is a **lower-cost path** to $n′$
- Suppose this **lower-cost path** is via path p on the frontier and $p$ ends at node $n$
- $p′$ was selected **before** $p$, i.e., $f(p′) ≤ f(p)$, so: $$cost(p′) + h(p′) ≤ cost(p) + h(p)$$
- Suppose $cost(n,n′)$ is the **actual** cost of a **lowest cost path** from $n$ to $n′$. The path to $n′$ via $p$ is **lower cost** than via $p′$ so: $$cost(p) + cost(n,n′) < cost(p′)$$
- From these equations: $$cost(n,n′) < cost(p′) − cost(p) ≤ h(p) − h(p′) = h(n) − h(n′)$$
- **Contradiction**: the choice of $p′$ can’t happen if we ensure $h(n) − h(n′) ≤ cost(n,n′)$, i.e., the **consistency/monotone condition**
- A* with a **consistent heuristic** and **multiple path pruning** always finds the **shortest path** to a goal — this is a strengthening of the **admissibility criterion**
#### A* example with cycle and multiple-path pruning
- ![[Pasted image 20231018102524.png]]
- ![[Pasted image 20231018102511.png]]
- The script $<A>^{m+n}_{a}$ 
	- $m$ = actual cost so far
	- $n$ = heuristic function from end node
	- $a$ = $m+n$
- Paths are **pruned** if, when they are **checked**, the **end** node is one in the **closed** list
### Direction of Search
The definition of searching is **symmetric** - find a path from start nodes to goal node **or** from goal node to start node
- **Forward branching factor** - Number of arcs **out** of a node
- **Backward branching factor** - Number of arcs **into** a node
Search complexity is $b^{n}$ - so we should use **forward** search if:
- **Forward branching factor** is **less** than **backward branching factor** and vice versa
However if the graph is **dynamically** constructed the backwards graph may **not be available**
#### Bidirectional Search
**Idea**: Search **backward** from the **goal** and **forward** from the **start** **simultaneously**
- Effective since $2b^{\frac{k}{2}}<b^{k}$ and so can result in an exponential saving in **time** and **space**
	- Where $k$ is the **depth** of the goal
- Main **problem** is making sure the frontiers **meet**
- The is often used with one **breadth-first** method that builds a **set** of locations that can lead to the **goal** and in the **other** direction **another** method can be used to find a **path** to these **interesting** locations
#### Island Driven Search
**Idea**: Find a set of islands (places where the forwards and backwards searches might meet) between $s$ and $g$
$$s\rightarrow i_{1}\rightarrow i_{2}\rightarrow ...\rightarrow i_{m}\rightarrow g$$
There are now $m$ **smaller** problems rather than 1 **big** problem
- Can be **effective** since $mb^{\frac{k}{m}}<b^{k}$
- The **problem** is to **identify** the islands that the path must pass through, which may need **problem-specific knowledge** - it is also difficult to **guarantee optimality**
- The subproblems can be solved using islands $\rightarrow$ *hierarchy of abstractions*
### Dynamic Programming
**Idea**: For static graphs build a **table** of $dist(n)$, the **actual** distance of the **shortest** path from node $n$ to a goal
- This can be build **backwards** from the goal:

![[Pasted image 20231018080505.png]]

This can be used **locally** to determine what to do, defining a **policy** of which arc to take from a given node
There are two main **problems**:
- Dynamic programming requires enough **space** to store the **whole graph**
- The $dist()$ function needs to be **recomputed** for **each** goal
### Bounded Depth-first search
Does **not expand** paths that exceed the bound
- Explores part of the search graph
- Uses **space linear** in the depth of the search
#### Iterative-deepening search
- Start with a depth of bound $b=0$
- Do a bounded depth-first search with bound $b$
- If a solution is found return solution,
- Otherwise, increment $b$ and repeat
This will find the **same** solution as breadth-first search
Since it is using depth-first search, uses **linear space**
- Iterative deepening has an asymptotic **overhead** of $\frac{b}{b−1}$ **times** the **cost of expanding** the nodes at **depth** $k$ using **breadth-first** search 
- When $b =2$ there is an overhead factor of $2$, when $b = 3$ there is an overhead of $1.5$, and as $b$ gets **higher** (as in typical problems) the overhead factor **reduces**
### Depth-first Branch-and-Bound
Combines **depth-first search** with **heuristic information**
- Finds an **optimal solution**
- Most useful when there are **multiple solutions** and we want an **optimal one**
- Uses the **space** of depth-first search (linear)

Suppose we want to find a single optimal solution. Let **bound** be the cost of the lowest-cost path found to a goal so far.

- If the search encounters a path $p$ such that $cost(p) + h(p) \geq bound$, $p$ can be pruned.
- If a non-pruned path $p'$ to a goal is found, the **bound** can be set to the cost of $p'$, and $p'$ can be remembered as the best solution so far.
- Depth-first search is used to ensure it uses **linear space**.
- When the search completes, it has found an optimal solution.
#### Depth-first Branch-and-Bound: Initializing Bound
- The **bound** can be initialized to $\infty$.
- The **bound** can also be set to an estimate of the optimal path cost.
- After depth-first search terminates, either:
    - A solution **was found**
    - **No** solution was found, and **no** path was pruned
	    - Bound was set such that you still explored all of the state space - solution is **not possible**
    - **No** solution was found, and a path **was** pruned
	    - Tells you it's **possible** to go **deeper** in the state space
	    - Solution **may** be possible, but need to look at more of state space 
- It can be **combined** with **iterative deepening** to **increase** the **bound** until either a solution is found or to show there is no solution.
- Cycle pruning works **well** with depth-first branch-and-bound.
- Multiple-path pruning would **defeat** the space-saving of using depth-first search (due to storing the explored set).
## Characterising Heuristics
**Definition**: If A* tree-search expands N nodes and the solution depth is d, then the effective branching factor, $b^*$, is the branching factor a uniform tree of depth d would have to contain N nodes. $$N=1+b^∗+(b^∗)2+…+(b^∗)^d$$For example, at a depth of 5 with 52 nodes, we get $b^* = 1.91$. 
- An effective branching factor closer to 1 indicates a **larger** problem that can be **solved**.

We can estimate $b^*$ experimentally, which is usually fairly consistent over different problem instances

When experimenting with heuristics $h1$ and $h2$:
- If it's **always** true that $h2(n) \geq h1(n)$ for all n, then h2 **dominates** h1.

- On average, A* tree-search using h2(n) expands **fewer** nodes than using h1(n). 
- A* expands **all** nodes with $f(n) < f^*$, where $f^*$ is the cost of an **optimal** path. 
- So, all nodes with $h(n) < f^* - g(n)$ are expanded. 
- Since $h2 \geq h1$, all nodes expanded with h2 **are expanded** with $h1$, but $h1$ may expand **more** nodes.

**Conclusion**: A good heuristic helps, i.e., a heuristic with **higher values** is **better** for search, provided it is **admissible** (and consistent if using multiple path pruning). 
- **But** we need to **ensure** that the **computation** required to use the heuristic does **not** offset the benefit of reduced nodes.
## Deriving Heuristics
### Relaxed problems
We can derive admissible heuristics from the **exact** solution cost of a **relaxed** version of the problem
- **Relaxed problem** - a problem with **fewer restrictions on operators**
We would like to generate heuristics **automatically**
- If the problem can be **described** in a formal language then we can **generate** relaxed problems

For each **restriction** that we **remove**, we may get several amissible heuristics $h_{1}...h_m(n)$
- We want to choose the heuristic which **dominates**
- If **none** of them **dominate**, then use $h(n)=max(h_{1}(n), ..., h_{m}(n))$
	- Since the options are all admissible, then $h$ is admissible
	- $h$ dominates all of its constituent heuristics
### Subproblems
We can also derive an admissible heuristic from the **solution cost** of a **subproblem**
- Cost of **subproblem** = **lower bound** on cost of **complete** problem

e.g. For the n-puzzle this is much more accurate than Manhattan distance

![[Pasted image 20231019204439.png]]

### Pattern databases
We can store the **exact** solution costs for each **possible** **simplified** problem or **subproblem** instance
- This is done **offline**
Heuristic $h_{DB}=$ cost of solution for corresponding problem

- We can construct the database by searching backwards from goal, i.e. using **dynamic programming**
- Pattern databases can be combined as before
	- $h(n)=max(h_{1}(n), ..., h_{m}(n))$
### Other approaches
#### Statistical approach
Run search over **training** problems and **gather statistics**
- Use statistical values for $h$
- **Not admissible**, but still likely to expand fewer nodes
- Cheap to compute
#### Select *features* of state that contribute to heuristic
- Use learning to determine weightings for these factors
- **Not admissible**
- Potentially **very cheap** to computer