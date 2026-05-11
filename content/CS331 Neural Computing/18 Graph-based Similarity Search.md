Some applications:
- Recommending similar items using a **co-purchasing graph** (e.g. Amazon)
- Retrieving related documents based on **co-citation graph** (e.g. Google Scholar)
- Find close authors on **collaboration graph**

There are **two types** of similarity search:

**Text-based similarity**
- Similarity relies on **document content**
	- e.g. *Jaccard string similarity*
	- ![[Pasted image 20260311123839.png]]
- ![[Pasted image 20260311123847.png]]

**Graph-based similarity**
- Similarity hinges on **graph structure**
	- e.g. *Jaccard node-pair similarity*
	- ![[Pasted image 20260311123933.png]]
- ![[Pasted image 20260311123940.png]]

**Jaccard Similarity** has a few interesting and useful properties:

![[Pasted image 20260311124225.png]]

However, there are also two **limitations** of the Jaccard similarity:

![[Pasted image 20260311124401.png]]

A visualisation of **Limitation 2:**

![[Pasted image 20260311124437.png]]

Instead, we could use **SimRank similarity**

The **basic intuition** is as such:
- Two nodes are **similar** if they are pointed to by **similar nodes**
	- This is a *recursive definition*
- Every node is **most similar** to **itself**
	- This is the *base case*

![[Pasted image 20260311124547.png]]

![[Pasted image 20260311125141.png]]

![[Pasted image 20260311125147.png]]

Google uses a **PageRank** model, which is modeled after sim-rank
- Both SimRank and PageRank can **recursively** capture **global information** from multi-hop neighbourhoods

![[Pasted image 20260311125357.png]]

So, next we will look at the **properties** of SimRank Similarity:

![[Pasted image 20260311125442.png]]

Next, we prove **why** this value $S(a,b)$ is bounded by 0 and 1

![[Pasted image 20260311125509.png]]

The **distance induced** by SimRank measure can be computed, and it has its own properties:

![[Pasted image 20260316160752.png]]

There are **two method** to compute SimRank similarity:

**Single-Pair** SimRank Search:

![[Pasted image 20260316161109.png]]

**All-Pairs** SimRank Search:

![[Pasted image 20260316161123.png]]

Along with this, there are some **optimisation** techniques that can further accelerate the **computation** of SimRank

First, there can be **duplication computations** in simrank:

![[Pasted image 20260316161226.png]]

We can solve this by computing the **partial sums**, using **memoisation**

![[Pasted image 20260316161525.png]]

We can compute this using the **Matrix Form of SimRank**

![[Pasted image 20260316161643.png]]

After representing S using $C, Q$, $S$ and $Q$, we use an identity matrix in order to make sure (of something)

![[Pasted image 20260316161650.png]]

Finally, we want to find the **error estimate** with our situation:

![[Pasted image 20260316162248.png]]

![[Pasted image 20260316162301.png]]

![[Pasted image 20260316162307.png]]

