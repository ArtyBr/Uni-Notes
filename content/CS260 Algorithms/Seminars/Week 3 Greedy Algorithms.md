## 1. We have to **schedule** n jobs, **one after another**, on **one machine**. 
### Instructions
- The **processing times** of jobs $1, 2, ..., n$ are positive numbers $t[1],t[2],...,t[n]$, and their **urgency factors** are positive numbers $u[1],u[2],...,u[n]$, respectively. 
- A schedule is a **permutation** of all n jobs; for example, $S = (1,3,2)$ is a schedule that first completes job $1$, then job $3$, and finally job $2$. 
- The **completion time** of a job in a schedule $S = (j_1,j_2,...,j_n)$ is the time it **waits** until it has been **processed**; in other words, the completion time of the $i'th$ job $j_i$ in schedule $S$ is: $$c_S(j_i) = t[j_1] + t[j_2] + ··· + t[j_i]$$
- The **urgency-weighted lateness** of job j in schedule S is defined to be: $$ℓ_S(j) = u[j] · c_S(j)$$
- The total urgency-weighted lateness of schedule S is defined by: $$L_S =ℓ_S(1)+ℓ_S(2)+···+ℓ_S(n)$$
- We are interested in finding a schedule that **minimises** total **urgency-weighted lateness**.
### Questions
*a)* Suppose that we have **three** jobs $1,2$, and $3$ with the following processing times and urgency factors: 

![[Pasted image 20231018123346.png]]

What are the completion times $c_S(1),c_S(2),$ and $c_S(3)$ of jobs $1, 2,$ and $3$ in schedule $S = (1,3,2)$?
- $c_S(1)=5$
- $c_S(3)=8$
- $c_s(2)=10$
*b)* What is the urgency-weighted lateness $ℓS(1), ℓS(2),$ and $ℓS(3)$ of jobs $1, 2,$ and $3$, respectively, in schedule $S$ from part (a)? 
- $ℓS(1)=15$
- $ℓS(3)=16$
- $ℓS(2)=10$
What is the total urgency-weighted lateness $L_S$ of schedule $S$?
- $L_S=41$
*c)* If there are just two jobs $1$ and $2$, then there are just two possible schedules: $(1,2)$ and $(2,1)$. 
Write the expression $L(1,2) −L(2,1)$ as a function of $t[1], t[2], u[1],$ and $u[2]$, and analyse its sign to characterise when schedule $(1,2)$ has smaller total urgency-weighted lateness than schedule $(2,1)$.
- $L(1,2) −L(2,1)=$
$$(t[1]u[1]+(t[1]+t[2])u[2])-(t[2]u[2]+(t[2]+t[1])u[1])$$ = $$t[1]u[1]+t[1]u[2]+t[2]u[2]-t[2]u[2]-t[2]u[1]-t[1]u[1]$$=$$t[1]u[2]-t[2]u[1]$$
- $S(1, 2)$ has smaller lateness when  $t[1]u[2]<t[2]u[1]$
*d)* Design a polynomial-time algorithm that - given the tables $t[1..n]$ and $u[1..n]$ of processing times and urgency factors of jobs $1,2,...,n$ - computes the schedule that minimises total urgency-weighted lateness. Give a proof that it is optimal, and analyze the running time cost. 
*Hint*: define an appropriate notion of inversions, based on your observations about how pairs are ordered in an optimal schedule from the previous part.
- An inversion is where one 2 jobs $i$ and $j$ are scheduled in $S$ such that $S[..., j, ..., i, ...]$ and $t[i]u[j]>t[j]u[i]$
- Optimal algorithm must contain no such inversions
```js
input t[n] = array of processing times
input u[n] = array of urgency factors
define s[n] = array containing job schedules
S[0] = 1

for i=2; i<len(t) i++:
	for j=1; j<i; j++;
		if t[i]u[S[j]] <= t[S[j]]u[i]
			insert i in position j in S, moving all elements up by 1

return S
```
This runs in $O(n^{3})$ time

Better algorithm:
Generate ratio of $$\frac{u[2]}{t[2]}>\frac{u[1]}{t[1]}$$
And order the list by this ratio in order to get the schedule
## 2. Here, we consider undirected graphs that are undirected rooted trees, represented by a parent array P
### Instructions

![[Pasted image 20231019125445.png]]
### Questions

![[Pasted image 20231019125510.png]]

Set 1 **is** isolated, Set 2 is **not** isolated
$\{\{3, 4\}, \{5, 6\}, \{1, 2\}, \{8, 9\}\}$

![[Pasted image 20231019184613.png]]

The largest set is of size 5 - no. parents with odd height

e.g. {{4, 3}, {5, 6}, {2, 1}, {8, 9}, {10, 11}}

![[Pasted image 20231019185431.png]]

### Algorithm:
- Label all nodes with how many children they have - $O(n)$
- Add all nodes with children count of $0$ to a set $S$
- Iterate through set $S$ of children, adding the child-parent pair as edge to $m$ if parent not already in $m$..
- After adding, update child count of first ancestor of parent by -1
- Continue until no nodes with 0 children
- Return $m$

