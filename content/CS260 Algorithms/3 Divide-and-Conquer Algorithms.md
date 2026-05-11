Divide and Conquer algorithms break problems into **smaller pieces**
- They follow the *"Divide, conquer, combine"* pattern
Often **split** the input into **equal sized** pieces
- e.g. Merge sort - gets a comparison-optimal sorting algorithm
Some **split**, and look at **interactions between pieces**
- e.g. Inversion counting
A key step is analyzing the running time using **recurrence relations**
- Use the *"master theorem"* to help solve these
We can use divide and conquer to speed up maths
- Faster algorithms for **integer** and **matrix** multiplication
## Divide and Conquer
- **Divide**: Break problem into several parts
- **Conquer**: Solve each part **recursively**
- **Combine**: Join solutions to sub-problems into overall solution

Most commonly used to obtain **fast** algorithms by **splitting**
- Break up problem of size $n$ into **two equal parts** of size $n/2$
- Solve two parts **recursively**
- Combine two solutions into overall solution in **linear time**
## Merge sort and proving recurrences
Given $n$ elements, **rearrange** in **ascending order**
### Merge sort
**Divide** array into two halves
**Recursively** sort each half
**Merge** two halves to make sorted whole
#### Merging - combine two pre-sorted list into a sorted whole
Perform only a **linear** number of operations
Use **temporary** array to build the merged output
- Go through each **pair** of characters in the list and **compare**, if one is '*smaller*' than the other, insert this into **temporary** list
#### Analysis
- Set $T(n)$ = number of **comparisons** to merge-sort an input of size $n$
- We can work out the merge-sort recurrence to define $T(n)$
	- **Base case**: $T(n) = 0$ if $n=1$ (a single element list is already sorted)
	- $T(n) \leq T(\lceil \frac{n}{2} \rceil)+ T(\lfloor \frac{n}{2}\rfloor))+n$
	- Time to sort left half + time to sort right half + n comparisons to merge together
	- Ceiling and floor accounting for odd length inputs
**Solution to the merge sort recurrence** is $T(n) = O(nlog(n))$
- Initially, we assume $n$ is a power of 2 for simplicity
	- Means we don't have to deal with ceiling and floor notation
##### 1. Proof by Recursion Tree

![[Pasted image 20231016142215.png]]

##### 2. Proof by Telescoping
**Claim**: If $T(n)$ satisfies this recurrence, then $T(n) = nlog_2(n)$
- (Still assuming $n$ is a power of 2)

![[Pasted image 20231016142434.png]]

##### 3. Proof by Induction
## Counting Inversions
Music site tries to **match** your song preferences with others
- You rank $n$ songs
- Music site consults database to find people with **similar tastes**
**Similarity metric**: number of inversions between two rankings:
- My rank: $1, 2, ..., n$
- Your rank: $a_{1}, a_{2}, ..., a_n$
- Songs $i$ and $j$ **inverted** if $i<j$, but $a_{i}>a_j$

![[Pasted image 20231016144106.png]]

If they **completely disagree**, there can be $nC(2)$ or $\frac{n(n-1)}{2}$ **comparisons**
### Divide and conquer approach
**Divide**: Separate list into two pieces
**Conquer**: Recursively count inversions in each half
**Combine**: Count inversions where $a_i$ and $a_j$ are in different halves, and return sum of three quantities
#### Divide and Conquer
![[Pasted image 20231016144457.png]]
#### Combine
Count left-right inversions without listing them all
- Assume each half is **sorted** and we will produce sorted output
- Count inversions where $a_i$ and $a_j$ are in different halves
- **Merge** two sorted halves into sorted whole

![[Pasted image 20231016144751.png]]
#### Implementation
**Pre-condition**: [Merge-and-Count] Inputs $L$ and $R$ are sorted
**Post-condition**: [Sort-and-Count] Output $X$ is sorted
```js
Sort-and-Count(X) { 
	if list X has one element 
		return 0 and the list X 
		
	Divide the list into two halves L and R 
	(rL, L) <- Sort-and-Count(L) 
	(rR, R) <- Sort-and-Count(R) 
	(rX, X) <- Merge-and-Count(L, R) 
	
	return r = rL + rR + rX and the sorted list X 
}
```

## Closest Pair of Points
**Closest pair** - Given $n$ points in the plane, find a pair with smallest **Euclidian distance** between them
- **Brute force** - check all pairs of points $p, q$ with $\Theta (n^2)$ comparisons
- Sometimes (not always) trying a **simplification** can give insights
	- 1-D version ($O(nlogn)$) easy if points are on a line - doesn't help us
- **Assumption** - No two points have same $x$ coordinate
	- Simplifies the presentation, while capturing difficulty
### Divide and Conquer approach
**Divide**: Draw a vertical line $L$ so that roughly $\frac{n}{2}$ points on each side
**Conquer**: Find closest pair in each side recursively
**Combine**: Find closest pair with one point in each side
Return best of 3 solutions

![[Pasted image 20231016164317.png]]

$\delta$ is the distance between the closest two points (both on either side)
Find a closest pair with a point in each side, **assuming distance** $< \delta$ 
- **Observation**: Only need to consider points within $\delta$ of line $L$
- Sort points in $2\delta$-strip by their $y$ coordinate
- Only check distances of those within **7** positions in sorted list

![[Pasted image 20231016165054.png]]
### Proof
**Definition** - Let $s_i$ be the point in the $2\delta$-strip with the $i^{th}$ **smallest** coordinate
**Claim** - If $|i-j|\geq 7$ then the distance between $s_i$ and $s_j$ is at least $\delta$
- Consider the $2 \delta$ x $\delta$ rectangle $R$ whose min $y$ coordinate is that of $s_i$
- Any point **outside** $R$ is more than $\delta$ away
- Divide $R$ into 8 **equal** sized squares
- No two points lie in the same $\frac{\delta}{2}$-by-$\frac{\delta}{2}$ box
	- Otherwise there are 2 points closer than $\delta$
- So there can be at most 7 other points in $R$

![[Pasted image 20231016182013.png]]

![[Pasted image 20231016182458.png]]

### Analysis
- For the running time we can write the recurrence relation $T(n)=2T\left(\frac{n}{2}\right)+ O(nlogn)$ for the divide and conquer step $T(1)=O(1)$
	- We can solve this by recursion tree or telescoping to get $T(n)=O(nlog^{2}n)$
## The Master Method
The master method automates solving certain recurrences
#### Recurrence recipe
We often see **recurrences** in the **form** $$T(n)=a T(\frac{n}{b})+f(n)$$
- Break the instance into $a$ **copies** each of **size** $\frac{n}{b}$
- Perform $f(n)$ **work** to **combine** the **results**
	- e.g. Merge sort: $a=2, b=2, f(n)=O(n)$
If we show the **recursion** tree:
- $a$ - **Branching factor**
- There are $a^i$ **subproblems** at **level** $i$
- There are $1+log_b(n)$ **levels**
- The **subproblems** are of **size** $n/b^{i}$ at **level** $i$

![[Pasted image 20231020182311.png]]
#### Recurrence tree for $f(n)=n^c$
Supposed $T(n)=aT(n/b)+n^{c}$ with $T(1)=1$ (and $n$ is a power of $b$)

![[Pasted image 20231020183022.png]]

- Work **increases** by a factor of $r=a/b^{c}$ **each** level
- Total work is $T(N)=n^{c}*\Sigma_{i=0}^{log_{b}(n)}r^{i}$ 

There are **three cases** that can arise from $T(n)$ depending on $r$
- If $r<1$, cost **decreases** from the root, and $T(n) = \Theta(n^c)$
	- Since $1+r+r^{2}+...+r^{k}<\frac{1}{1-r}$ which is constant (in $a, b, c$)
- If $r=1$, cost is the **same** at each level of the tree, $T(n)=\Theta (n^{c}log(n))$
	- Since $1+r+r^{2}+...+r^{k}=k+1$
- If $r<1$, cost **increases** as we go down the tree, $T(n)=\Theta(n^{log_{b}(a)})$
	- Since $1+r+r^{2}+...+r^{k}=\frac{r^{k+1}-1}{r-1}$ Therefore $r^{k}$ dominates 
#### The Master Method
**Generalises** for any $n$
- $n$ does **not** have to be a **power** of $b$ and it can handle ceil/floors
##### Statement of the master theorem
Let $a\geq1, b\geq2, c\geq0$
- Suppose that $T(n)$ satisfies the recurrent $aT(n/b)+n^{c}$
- With $T(0)=0$ and $T(1)=\Theta(1)$
- Where $\frac{n}{b}$ can be replaced by $\lceil \frac{n}{b}\rceil$ or $\lfloor \frac{n}{b}\rfloor$
Then 3 cases:
- If $c>log_{b}(a)$ then $T(n)=\Theta(n^{c})$
- If $c=log_{b}(a)$ then $T(n)=\Theta(n^{c}log(n))$
- If $c<log_{b}(a)$ then $T(n)=\Theta(n^{log_{b}(a)})$

Can be extended with $O()$ and $\Omega()$ throughout instead of $\Theta()$ 
## Integer Multiplication
**Addition** is easy: $\Theta (n)$ 
**Multiplication**:
- Using long multiplication, $\Theta (n^{2})$ bit operations
## Matrix Multiplication
#### Dot product
$c = a.b = \sum_{i=1}^{n}a_{i}b_{i}$  