# Latent Factor Models (LFMs)

Learn a **new basis** from the data
- *Change of basis* means mapping $x_{i}$ to $z_{i}$

Advantages:
- **Outlier detection** - an outlier cannot be represented by a combination of these parts
- **Dimensionality reduction** - represent the data using a **limited number** of parts
- **Visualisation** - If we only have 2 or 3 parts, we can view it on a graph
- **Interpretation** - we can figure out intuitively what different parts represent and which are most important

For example, *vector quantization* (VQ) - K-means can be considered a latent factor model
- Replace examples (feature vectors) with the **mean** of their cluster

![[Pasted image 20251105093916.png]]

Here is how this works:
- Assume we have a feature vector $x_{i}$ with $3$ dimensions. We can therefore **represent** it with 3 **parts**

![[Pasted image 20251105094007.png]]

- Where weights $z_{i}=[x_{i1}x_{i2}x_{i3}]$ is the new feature vector - nothing interesting yet, $z_{i}^{T}=x_{i}$
- Now if we use VQ - we can assume 4 clusters with $x_{i}$ in cluster 3 with centroid $m$
- $x_{i}\approx m_{3}=0\times m_{1}+ 0\times m_{2} + 1\times m_{3} + 0 \times m_{4}$
- Sort of like a binary version of PCA - where you put all your weight on one of the bases

Using VQ as our *latent-factor model*: $z_{i}=[0~0~1~0]$
- Our '**parts**' are now the **means** of the clusters
- $z_{i}=[0~0~1~0]$ is a new **feature representation** of $x_{i}$
- note that in this case, VQ uses only **one part** to represent $x_{i}$

Now, assuming that we want a model for classification:
- It's an easy task using our new basis $Z$
	- 1-hot encoded vectors point to the class
- New features are the $\{z_{i}\}$ - we are using only one part to define these feature vectors
	- What if a new data point is *not* well-represented by only one part?
- Can we make this new representation, $\{z_{i}\}$. more **continuous?**
- Can we use a **combination** of parts?
# Principal Component Analysis
PCA is a **generalisation** of VQ that allows for **continuous** $Z$
- Allows for more than 1 non-zero contribution
- Can use **fractional** or **negative** weights

Takes a matrix $X$ and outputs two matrices, $Z$ and $W$, where:
$$X=ZW$$

![[Pasted image 20251105095152.png]]

- Each row $c$ of $W$ is a **part** ($w_{c}$) a.k.a a **factor** or **principal component**
	- We have $k$ PCs
- Each row $i$ of $Z$ is a set of weights ($z_{i}$) aka new features
- Column $k$ of $W$ ($w^{j}$) represents the values of the $j$ th dimension across all parts or PCs

![[Pasted image 20260415235944.png]]

Notation:
- $w^j$ = $j$'th column of $W$
- $z_{i}$ = $i$'th row of $Z$

Using matrices $Z$ and $W$ as given by PCA, we can reconstruct $x_{ij}$ as:
$$\hat{x}_{ij}=z_{i1}w_{1j}+z_{i2}w_{2j}+\dots+z_{ik}w_{kj}=\sum\limits_{c=1}^{k}z_{ic}w_{cj}=(w^j)^Tz_{i}=<w^j,z_{i}>$$
- Where $<w^j,z_{i}>$ is the **inner product** of the column vector $w^j$ and row vector $z_{i}$

PCA uses a **weighted combination** of the $j$th dimension of all PCs to *approximate* feature $x_{ij}$, which is the $j$th dimension of $x_{i}$
- The weights are given by $z_{i}$, which is the new **feature vector** for $x_{i}$
PCA approximates the complete feature vector $x_{i}$ by $\hat{x}_{i}=W^Tz_{i}$

**Dimensionality reduction**
PCA is a **linear model** with a closed-form solution, which can map the data to a **lower dimensional space** $k\ll d$
- If we only use the first two columns of $Z$ then we only need the first 2 rows of $W$
- If $x_{i}$ has 20 dimensions, then $\hat{x}_{i}=W^Tz_{i}$ still allows me to reconstruct $\hat{x}_{i}$ as a one-column feature vector of 20 dimensions by using the 2d feature vector $z_{i}$
	- The dimensions match: $(20\times 1)=(20\times 2)(2\times 1)$

Change of basis from $X$ to $Z$ - the basis vectors are the **rows** of $W$. i.e the $w_{c}$'s
- The **coordinates** in the new basis for the $x_{i}$ are the $z_{i}$
PCA is a matrix **factorization** model - provides a *better approximation* than VQ
- Useful for understanding other algorithms (e.g. autoencoders)
- Allows visualising features in 2d (or 3d) if the top two (or three) PCs are used

Example: Linear models (linear regression)

![[Pasted image 20260416111601.png]]

PCA is a **data projection technique**, where our $k$-dimensional subspace $S$ has $k\ll d$ dimensions
- Given a dataset $X=\{x_{1},\dots , x_{n}\}$
- Compute the **mean** of the data:
$$\mu=\frac{1}{n}\sum\limits_{i=1}^nx_{i}$$
- Goal is to *find* a $k$-dimensional **subspace** $S$, such that the **centred data** $\{x_{i}-\mu\}$ is *well-represented* by its **projection** onto the bases of $S$
	- The projection of a centred point $(x_{i}-\mu)$ onto the bases of $S$ gives us the coordinates of $(x_{i}-\mu)$ in $S$ such that its new representation $z_{i}$ is the most similar to $(x_{i}-\mu)$

We would like to find such a PCA which **minimises** the **square distance** to each $x_{i}$ (from the projected points $\hat{x_{i}}$)

![[Pasted image 20260416112234.png]]

![[Pasted image 20260416112241.png]]

There are two perspectives we can interpret PCA from in order to compute the representation $Z$

## Reconstruction POV
We want to find matrices $Z$ and $W$ that **best reconstruct** matrix $X$

We want to **minimise** the following summation of **squared errors**:
$$f(W,Z)=\sum\limits_{i=1}^n||W^Tz_{i}-x_{i}||^2$$
We have $d$ sums over $n$ samples
- Equivalent to solving $d$ **regression** problems
We are learning the new features $z_{i}$ and discovering the PCs in $W$
- Each $z_{i}$ indicates how to combine the PCs
	- i.e. the rows of $W\rightarrow \{w_{c}\}$

So our PCA objection function is:
$$||ZW-X||^2_{F}$$
The Frobenius norm $F$ of an $m\times n$ matrix $A$ is $||A||_{F}=\sqrt{ \sum \limits_{i=1}^m\sum\limits^n_{j=1}A^2_{ij} }$
We say we **approximate** $X$ since usually $k\ll d$
- Even for $k=d$, small inaccuracies may be introduced due to floating point calculations

Our PCA objective function for reconstruction may **not** have a **unique global minimum**
- Non-uniqueness property of PCA's objectives function
- The components we find may provide *redundant* information about the data
- We can make the solution **unique** and **non-redundant** by *adding three constraints*:
	1. **Normalisation**: Enforce $||w_{c}||=1$
		- Without this, there would be infinite projected lines of $W$ from $X$ (scaled up or down by $\alpha$). Enforces uniqueness
	2. **Orthogonality**: Enforce ${w_{c}}^Tw_{c'}=0$ for all $c\neq c'$
		- This makes the solution non-redundant since it means all information represented by each PC is distinct from the other ones. (They aren't just combinations of other PCs).
	3. **Sequential fitting**: Sequentially find the principal components with maximum variance
		- Start with the top PC, then move to 2nd top, etc.

For sequential fitting, we move onto the maximum variance POV
## Maximum variance POV
Want to find matrix $W$ that gives the directions of **maximum variance** of the data.
- After projecting the points onto those directions, we get matrix $Z$

Components can be used as a **new set of axes** to represent the data in a new feature space - **basis vectors**
- To know where the axes are with respect to eh current axes, the **data must be centred** (subtract the mean)

So, we want PCA to find the directions of maximum variance in $X$ after centring the data
These directions are the vectors in $W$
- The first PC, $w_{1}$ is the direction of **largest variance** - first row of $W$
- The second PC, $w_{2}$, is the direction of largest variance **orthogonal** to $w_{1}$ - second row of $W$
- The $c$th PC, $w_{c}$ is the direction of largest variance **orthogonal** to $w_{1}\dots w_{c-1}$

Consider the top PC:
- Given centred $X$, PCA aims to find $w_{1}\in \mathbb{R}^d,||w_{1}||=1$ that maximises the variance
- We want to find the **largest spread** of projected points along the axis (or line) defined by $w_{1}$
	- In other words, the line that gives the **maximum range of values** after projecting the points to it
- $w_{1}$ should maximise the value of $z_{i}$ for all $n$ data points
	- i.e. $w_{1}$ should maximise the summation $\sum\limits_{i=1}^nz_{i}$
$$\sum\limits^n_{i=1}{z_{i}}^2=z^Tz=||z||^2=||Xw_{1}||^2\rightarrow(n\times d)(d\times 1)$$

Now we have some theorems:
>[!note] Theorem
>The maximum value attained by ${w_{1}}^TX^TXw_{1}$ with $||w_{1}||=1$ is the largest **eigenvalue** of $X^TX$:
>$$\underset{w_{1}}{max}\{{w_{1}}^TX^TXw_{1}\}=e_{1}$$
>The argmax is the eigenvector of $X^TX$ associated with eigenvalue $e_{1}$:
>$$\underset{w_{1}}{argmax}\{{w_{1}}^TX^TXw_{1}\}=e_{1}$$

So how do we solve our objective function by enforcing normalisation, orthogonality and sequential fitting?
- Don't want to solve several regression problems with these constraints
- We use SVD - Singular Value Decomposition

![[Pasted image 20260416122315.png]]

In PCA, training involves finding matrix $W$
- After training, the model has **two parts**:
	- $\mu_{j}$ - from the training data
	- $W$ matrix (transformed parts of the input)

Given new data $\tilde{x}$, use $\{\mu_{j}\}$ and $W$ to compute the new $\tilde{Z}$:
1. Centre data: Replace each $\tilde{x_{j}}$ with $(\tilde{x_{j}}-\mu_{j})$
2. Find $\tilde{z}=(WW^T)^{-1}(W\tilde{x})=W\tilde{x}$

Testing: the reconstruction error is how close the approximation is to $\tilde{X}$:
$$||W^{T}\tilde{z}-\tilde{x}||_{2}^2$$
