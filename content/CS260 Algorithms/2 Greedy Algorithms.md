Picks the **next** thing to do that looks like the **best** option
Greedy is optimal for several scheduling problems
### Interval Scheduling
##### Arrange a set of tasks, given constraints
- Job $j$ starts at $s_j$ and finishes at $f_j$
- Two jobs are **compatible** if they don't overlap
- **Goal**: find a maximum subset of mutually compatible jobs

![[Pasted image 20231009143048.png]]

**Greedy template** considerations:
Consider Jobs in some natural order
Take each job if it's compatible with the ones already taken
- [Earliest start time] Consider jobs in ascending order of $s_j$
- [Earliest finish time] Consider jobs in ascending order of $f_j$
- [Shorted duration] Consider jobs in ascending order of $f_j - s_j$
- [Fewest conflicts] For each job $j$, count the number of conflicting jobs $c_j$. Schedule in ascending order of $c_j$

![[Pasted image 20231009143447.png]]

Blue task overlaps with the black tasks, meaning you only end up doing the blue task - doesn't get goal done since maximum number of tasks not completed
###### That just leaves the [Earliest finish time] first algorithm

```js
Sort jobs by finish times so that f1 <= f2 <= ... <= fn 
A = I
for j = 1 to n { 
	if (job j compatible with A) 
	A = A[j] 
} 
return A
```
### Interval Partitioning
##### Schedule a set of lectures into classrooms
- Lecture $j$ starts at $s_j$ and  finishes at $f_j$
- **Goal**: Find least number of classrooms to schedule all lectures so that no two occur at the same time in the same room

![[Pasted image 20231009144619.png]]

- **Depth** of a set of open intervals is the maximum number that contain any given time
- Number of classrooms needed $\geq$ depth
###### Greedy algorithm
Consider lectures in increasing order of start time: assign lecture to any compatible classroom

```js
Sort intervals by starting time so that s1 <= s2 <= ... <= sn
d = 0;
for j = 1 to n{
	if (lecture j is compatible with classroom k)
		schedule lecture j in classroom k
	else
		allocate a new classroom d + 1
		schedule leture j in classroom d + 1
		d = d + 1
}
```

##### Implementation
$O(nlogn)$ time because of sorting
- For each classroom $k$, maintain the finish time of last job added
- Keep the classrooms in a **priority queue** to find first free time
---
**Observation**: Greedy algorithm never schedules two incompatible lectures (**Correctness**)

**Theorem**: Greedy algorithm is optimal
**Proof**:
- Let $d$ = number of classrooms that the greedy algorithm allocated
- Classroom $d$ is opened because we needed to schedule a job, say $j$, that is incompatible with all $d-1$ other classrooms
- These $d$ jobs each end after $s_j$ (otherwise we could have used one)
- Since we sorted by start time, all these incompatibilities are caused by lectures that start no later than $s_j$
- So we have $d$ lectures overlapping at time $s_j + \epsilon$ (just after $s_j$)
- Therefore all correct schedules must use $\geq d$ classrooms
### Minimizing Lateness
- Single resource processes one job at a time
- Job $j$ required $t_j$ units of processing time and is due at time $d_j$
- If $j$ starts at time $s_j$, it finishes at time $f_j = s_j + t_j$
- **Lateness**: $L-J = max${$0, f_j - d_j$}
- **Goal**: Schedule all jobs to minimize **maximum** lateness = $max L_j$

![[Pasted image 20231009171429.png]]

**Greedy template** considerations:
- [Shorted processing time first] Consider jobs in ascending order of processing time $t_j$
- [Earliest deadline first] Consider jobs in ascending order of deadline $d_j$
- [Smallest slack] Consider jobs in ascending order of slack $d_j-t_j$

![[Pasted image 20231009172125.png]]

###### Greedy algorithm - [Earliest deadline first] is the only one left

```js
Sort n jobs by deadline so that d1 <= d2 <= ... <= dn
t = 0
for j = 1 to n
	Assign job j to interval [t, t + tj]
	sj = t
	fj = t + tj
	t = t + tj
output intervals [sj, fj]
```
---
- **Observation 1** - There exists an optimal schedule with no idle time

![[Pasted image 20231009180720.png]]

- **Observation 2** - The earliest-deadline-first greedy schedule has no idle time
###### Definition: Inversion - a pair of jobs $i, j$ such that $i<j$ but $j$ scheduled before $i$

![[Pasted image 20231009180934.png]]

- **Observation 3** - The earliest deadline first schedule is the unique schedule with no idle time that has **no inversions**
- **Observation 4** - If an **idle-free** schedule has an inversion, then it has an adjacent inversion (two inverted jobs next to each other)
---
**Key claim**: Swapping two consecutive, inverted jobs, reduces the number of inversions by one and does not increase the maximum lateness ("exchange argument")
- Let $L$ be the lateness before the swap, and $L'$ be the lateness after
- $L'_k = L_k$ for all $k \neq i, j$
- $L'_i \leq L_i$ : moving $i$ earlier can't make it later
- If job $j$ is late, 
	 $L' = f'-d_j$ (By definition)
	     $= f_i-d_j$ (now $j$ finishes at $f_i$)
	     $\leq f_i-d_i$ ($i<j$ implies $d_i\leq d_j$)
	      $= L_i$ (by definition)
- So $L'_j \leq L_i$ - Hence, the maximum lateness cannot have increased
**Theorem**: Greedy schedule $S$ is optimal
Basically, greedy schedule essentially does these swaps until there are no inversions left, which means for each swap the lateness at least decreases or stays the same. Therefore either $S$ was already optimal or greedy removed as much lateness as possible.
### Optimal Offline Caching
**Caching** is a widely used concept in computer science
- We have a **cache** with capacity to store $k$ items
- Sequence of $m$ item requests $d_1, d_2, ..., d_m$
- **Cache hit**: Item already in cache when requested
- **Cache miss**: Item is not in the cache when requested: must fetch requested item into cache, and evict some existing item if it's full
**Goal** - find an eviction schedule that minimizes number of cache misses
**Offline caching** - The sequence of requests is known in advance

**Furthest-in-future eviction strategy** - Evict the item in the cache that is not requested until farthest in the future
##### Reduced eviction schedules
- Define a **reduced schedule** to be a schedule that only inserts an item into the cache in a step in which that item is requested
- **Intuition** - can transform an unreduced schedule into a reduced one with no more cache evictions
#### Analysis
**Theorem**: FF is optimal eviction algorithm
**Proof** by induction on number of requests $j$
**Invariant** - There exists an optimal reduced schedule $S$ that makes the same eviction schedule as $S_{FF}$ through the first $j+1$ requests
- Let $S$ be **reduced schedule** that satisfies invariant up to $j$ requests
- We produce $S'$ that **satisfies invariant** after $j+1$ requests
	- Consider $(j+1)^{st}$ request $d=d_{j+1}$
	- Since $S$ and $S_{FF}$ have **agreed** so far, they have the **same cache contents** before request $j+1$ 
##### Case 1:
- $d$ is **already in the cache**
- $S'=S$ satisfies invariant
##### Case 2:
- $d$ is **not in the cache** 
- $S$ and $S_{FF}$ **evict the same element**
- $S'=S$ satisfies invariant
##### Case 3:
- $d$ is not in the cache
- $S$ and $S_{FF}$ **evict different elements** 
	- $S_{FF}$ evicts $e$, $S$ evicts $f$ ($f \neq e$)
- Begin construction of $S'$ from $S$ by evicting $e$ instead of $f$

![[Pasted image 20231012182430.png]]

- Now $S'$ **agrees** with $S_{FF}$ on first $j+1$ requests
	- We've shown that having element $f$ in cache is **no worse** than having element $e$
- Let $j'$ be the **first** time after $j+1$ that $S$ and $S'$ take **different actions** and let $g$ be **item requested** at time $j'$
###### Case 3a
- $g=e$ 
- Can't happen with FIF since there must be a request for $f$ before $e$
###### Case 3b
- $g=f$
- Element $f$ **can't be in cache** of $S$, so let $e'$ be the element that $S$ evicts
	- If $e'=e, S'$ accesses $f$ from cache
		- Now $S$ and $S'$ have the **same** cache
	- If $e' \neq e,S'$ evicts $e'$ and brings $e$ into the cache
		- Now $S$ and $S'$ have the **same** cache
###### Case 3c
- $g \neq e, g \neq f$
- $S$ **must** evict $e$
- Make $S'$ evict $f$, now $S$ and $S'$ have the **same** cache
### Coin Changing
**Goal**: Given currency denominations 1, 5, 10, 25, 100, devise a method to pay amount to customer using least number of coins
**Cashier's algorithm** - At each iteration add coin of the largest value that does not take us past the amount to be paid
