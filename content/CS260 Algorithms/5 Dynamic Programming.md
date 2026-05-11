- Breaks up the problem into a series of **overlapping sub-problems**
- Build up solutions to the whole
	- We require the '*optimal substructure*' property
		- The **optimal solution** can be **built up** from **optimal solutions** to **subproblems**
		- We want that the **number of subproblems** does **not** grow too **big**
	- Also requires that **future** decisions **depend** on **earlier** decisions
		- Deciding to do something at one step may affect the ability to do something in a later step
		- Makes **greedy algorithms** **invalid** for DP problems

Use **bellman equation** to set up algorithm:
$$Opt(j)=\begin{cases}

0 & \text{if } j = 0\\

min_{i1\leq i\leq j}(e(i, j)+c+Opt(i-1)) & \text{otherwise }
\end{cases}$$
### Top-Down (memoization)
Implemented with **recursion** and made efficient with **memoization**
- Work out $OPT(i)$ by working out $OPT(i-1)$ etc.
- For problems where this $(i-1)$ is a **repeated** action, we can **store** the result of the function in a hashmap to be **found** again when the operation is re-done
### Bottom Up (tabulation)
Implemented with **iteration** and starts with the **base case**
- Solve the problem by filling in/solving values from smallest to biggest
### Top-Down vs Bottom Up
- **Bottom-up** is better:
	- Runtime usually faster as **iteration** does **not** have **overhead** like **recursion** does
	- However, **ordering** of subproblems **matters**
- **Top-down** is better:
	- Much **easier** to **write**
	- **Ordering** of subproblems **doesn't matter**
## Weighted Interval Scheduling
#### The problem
- Job $j$ starts at $s_j$, finishes at $f_j$ and has weight or value $v_j$ 
- Two jobs are **compatible** if they don't overlap
**Goal** - Find maximum **weight** subset of mutually compatible jobs

![[Pasted image 20231030143406.png]]

**Greedy fails** to obtain solution to this modified problem

**Notation**: Label jobs by finishing time: $f_{1}\leq f_{2}\leq ... \leq f_n$
**Define**: $p(j)$ = largest index $i<j$ such that job $i$ is compatible with $j$
- ![[Pasted image 20231030143846.png]]
$OPT(j)$ is the **value** of **optimal solution** up to index $j$
- **Case 1** - $OPT$ **selects** job $j$
	- Collect profit $v_j$
	- **Can't** pick **incompatible** jobs $\{(j) + 1, p(j) + 2, ..., j-1 \}$
	- Must include **optimal** solution to the WIS problem consisting of remaining compatible jobs $1, 2, ..., p(j)$
		- **Optimal substructure**
- **Case 2** - $OPT$ does **not select** job $j$
	- Must include optimal solution to the WIS problem consisting of remaining compatible $1, 2, ..., j-1$
		- **Optimal substructure**
#### **Brute force algorithm**:

![[Pasted image 20231103152001.png]]

![[Pasted image 20231103152012.png]]

Recursive algorithm above can **fail** spectacularly because of **redundant** sub-problems - **exponential growth**

![[Pasted image 20231103152205.png]]
#### Memoization
**Memoization** - **Store** the results of each sub-problem in a **table** and **look up** the values whenever needed

![[Pasted image 20231103152911.png]]

**Claim** - Memoized version of algorithm takes $O(nlogn)$ time
- Sort by **finish time**: $O(nlogn)$
- Computing $p(.):O(nlogn)$ via sorting by **start time**
$\text{M-Compute-Opt}(j)$ - each invocation takes $O(1)$ time and either:
1. Returns an **existing value** $M[j]$
2. Fills in one **new entry** $M[j]$ and makes two recursive calls
Progress measure $\Phi$ = # non-empty entries of $M[]$
- Initially $\Phi=0$, throughout $\Phi\leq n$
- Increases $\Phi$ by 1 $\rightarrow$ at most $2n$ recursive calls **in total**
Overall **running time** is $O(n)$
- **If** jobs are **pre-sorted** by start and finish time

![[Pasted image 20231103162915.png]]
#### Bottom-Up
**Bottom-up dynamic programming** - Unwind the recursion
- Solve the problem by filling in the table from smallest to biggest 

- **No** **recursive** calls, because every needed value is already there
- **Same** computational complexity, but slightly **simpler code**

![[Pasted image 20231103152437.png]]
## Segmented Least Squares
### **Least squares problem** (already solved mathematically):
- Given $n$ **points** in the **plane**: $(x_{1}, y_{1}), ..., (x_{n}, y_{n})$
- **Find** a **line** $y=ax+b$ that **minimises** the **sum** of the **squared error**

![[Pasted image 20231103163631.png]]
### **Segmented** least squares problem:
- Instead of **one** line, points lie roughly on a sequence of **several** line segments
- Find a sequence of lines that **minimises** some $f(x)$
We **choose** $f(x)$ to balance **accuracy** and **complexity** of solution
- Allow $n$ lines: perfect fit, but no useful explanation of the data

- Combine **SSE** for each segment $E$, with total number of lines $L$
- Choose $f(x)=E+cL$ for constant $c>0$

$O(n^3)$ **algorithm**:
- Set up notation:
  $OPT(j)$ = **minimum** **cost** for points $p_{1}, p_{2}, ..., p_j$
  $e(i, j)$ = **minimum** **sum of squares** for points $p_{i}, p_{i+1}, ..., p_j$
- User previous equations to write $e(i, j)$ in terms of $SX_{i,j}, SXY_i,j$ etc.
- For points $p_i… p_j$, we take the optimal **least squares** solution 
- To compute $OPT(j)$, find the cost of extending a prefix solution:
	- If the **last** segment uses points $p_i , p_{i+1} , . . . , p_j$for some i, then the cost is given by $e(i, j) + c + OPT(i-1)$

![[Pasted image 20231105135651.png]]

**Running time** $O(n^3)$ 
- The **bottleneck** is computing $e(i,j)$ for $O(n^2)$ pairs
- $O(n)$ per $(i,j)$ pair using the previous formular for sum squared error

$O(n^2)$ algorithm

![[Pasted image 20231105140211.png]]


## Knapsack Problem
**Maximise value** subject to **capacity**
- Given $n$ objects and a 'knapsack' of **fixed size**
- Item $i$ weighs $w_{i}>0$ weight and has value $v_{i}>0$
- Knapsack has **capacity** of $W$
- **Goal**: Fill knapsack to **maximise total value**

Define optimal over **indices** and **weights**
- $OPT(i, w)$ = max profit subset of items $1, ..., i$ with **weight limit** $w$
- **Case 1**: $OPT$ does **not** select item $i$
	- $OPT$ selects best of $\{1, 2, ..., i-1\}$ using weight limit $w$
- **Case 2**: $OPT$ **selects** item $i$
	- New **weight limit** $w-w_i$
	- $OPT$ selects best of $\{1, 2, ..., i-1\}$ using new weight limit
$OPT(i, w)=$
$$\begin{cases}

0 & \text{if } j = 0\\  

OPT(i-1, w) & \text{if } w_i>w\\

max(OPT(i-1, w), v_i+Opt(i-1, w-w_i)) & \text{otherwise }
\end{cases}$$
**Implementation**
- Fill up an $n\times W$ array
- Takes $O(1)$ time per entry, $O(nW)$ time total

**![[Pasted image 20231105152048.png]]
![[Pasted image 20231105152102.png]]
## RNA secondary structure
String $B=b_{1}, b_{2}, ..., b_n$ over the alphabet $\{A, C, G, U\}$
- $A$ **pairs up** with $U$
- $G$ **pairs up** with $C$
### Secondary Structure

![[Pasted image 20231105155738.png]]

![[Pasted image 20231105155755.png]]

![[Pasted image 20231105155806.png]]

![[Pasted image 20231105155820.png]]

### Dynamic Programming Over Intervals

**Notation**: $OPT(i,j)=$ maximum number of base pairs in a secondary structure of the substring $b_{i}b_{i+1}...b_{j}$
- **Case 1** - If $i\geq j-4$
	- $OPT(i,j)=0$ by **no-sharp-turns** condition
- **Case 2** - Base $b_{j}$ is not involved in a pair
	- $OPT(i,j)=OPT(i,j-1)$
- **Case 3** - Base $b_{j}$ pairs with $b_{t}$ for some $i\leq t<j-4$
	- **Non-crossing** constraint decouples resulting sub-problems
	- $OPT(i,j)=1+max_{t}\{OPT(i,t-1)+OPT(t+1,j-1)\}$
		- Take max over $t$ such that $i\leq t<j-4$ and $b_t$ and $b_j$ are **Watson-Crick** complements
- 