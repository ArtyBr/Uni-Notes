HITS (Hyperlink-Induced Topic Search) is a **PageRank-like** **ranking** algorithm
- It measures the **importance** of web-pages (nodes) based on "**Authority**" and "**Hub**" scores of each web-page

Basic intuition:
- A good web-page should:
1. **Link** to **other good web-pages**, and
	- Good *out*-neighbours
2. Be **linked** by **other good web-pages**
	- Good *in*-neighbours

In HITS, each web-page has **two scores**:
- **Authority**
	- ![[Pasted image 20260309165231.png]]
- **Hub**
	- ![[Pasted image 20260309165239.png]]

They **reinforce** each other and define a **bi-recursive relationship**

![[Pasted image 20260309165219.png]]

We can use a **bipartite graph** in order to depict this relationship

![[Pasted image 20260311120837.png]]

There are 12 equations in all in order to compute the scores.
- This can be very time-consuming, so we offer some **optimisation techniques**

---

Method 1: **HITS iterative approach**

We can compute $a(x)$ and $h(x)$ using an iterative algorithm:

![[Pasted image 20260311121043.png]]

![[Pasted image 20260311121132.png]]

---

Method 2: **Dominant Eigenvector**

We use certain **notations**:
- Authority **vector $a$** = $[a(1), a(2), ...m a(n)]^T$ ($n=|V|$)
- Hub **vector $h$** = $[h(1), h(2), ..., h(n)]^T$

![[Pasted image 20260311121756.png]]

So, we get this algorithm by using the vector forms:
- We use a similar iterative approach as the previous method, but as vectors now

![[Pasted image 20260311121849.png]]

Next, we would like to find the Eigenvector of the matrix:

![[Pasted image 20260311122037.png]]

- This tells us that if we want to compute the authoritative vector $a$ we need $h$ and vice-versa for $h$ (need $a$)
- We can plug the values in and cancel them out
- At the end of plugging in each equation to each other, we get the **same denominator** at the bottom
- Therefore, we can cancel out the denominators and just look at the nominators, telling us what the dominant eigenvectors of the relevant matrices are

![[Pasted image 20260311122820.png]]

---

Method 3: **SVD-based method**

The SVD of a matrix $X$ is the decomposition into $X=U\cdot \sum \cdot V^T$

![[Pasted image 20260311122908.png]]

Next, we show **how** HITS is **related to SVD**

![[Pasted image 20260311122919.png]]

![[Pasted image 20260311122925.png]]