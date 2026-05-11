# Unsupervised learning
We only have $\{x_{i}\}$ and no explicit labels or targets
we want meaningful information about $\{x_i\}$
- Infer the causal structure underlying the data
- This structure is **latent** which means it cannot be **easily observed**

## Clustering
Data may form **clusters**
- Points within a cluster are **similar** to each other
	- Vice versa - different clusters are dissimilar
- Such a distribution is **multi-modal** - has multiple modes, or regions of high probability mass (multiple clusters)

![[Pasted image 20251104131245.png]]

**Assumptions**:
- $X = \{x_{1}, x_{2}...x_{n}\}$ lives in a **euclidean space**
- $X$ can be grouped into $K$ groups or clusters
- points within the same group are similar

### K-means clustering
Assumes there are $K$ clusters and each point is close to its **cluster centre** - the **mean** of all points in the cluster, a.k.a a **centroid**
- If we knew the cluster assignment, we could easily compute the means
- If we knew the means, we could easily computer the cluster assignment
- chicken and egg problem
- $NP$ - hard

Very simple heuristic for the algorithm
- Start **randomly** and **alternate** between **cluster assignment** and **computing means**

Input: the **number of clusters $K$** (hyper-parameter)
Initially, **guess** the centres (or mean) of each cluster
Algorithm
- Assign each $x_{i}$ to the **closest mean** (closest centre)
- **Update** the means based on the assignment
- **Repeat** until convergence

![[Pasted image 20251104132046.png]]

---

K-means **objective**
- Find cluster centres, $m$ and assignments $r$ that minimise the sum of squared distance of data points $\{x_{1}...x_{n}\}$ to their assigned cluster centres
- ![[Pasted image 20251104132617.png]]
![[Pasted image 20251104132623.png]]

