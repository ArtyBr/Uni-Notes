Data which otherwise may look **non-separable** may be **separable** under a **change of basis**

![[Pasted image 20260417173556.png]]

Suppose we have data points in some 1 dimension - 1d feature vectors
- Models like this can be **fit to a higher-dimensional space** using a **change of basis**

![[Pasted image 20260417173904.png]]

However, how can we do this when we have **several features?**
- i.e. high-dimensional feature vectors in $X$

Here's an example for $d=2$ with a polynomial $p=2$

![[Pasted image 20260417173952.png]]

So every single data point is represented uniquely in the new basis.

For a dimension $d$ and polynomial degree $p$, the **total number** of **polynomial bases** is:

![[Pasted image 20260417174353.png]]

That is, this is the **dimension of the rows** in $Z$ (*number of columns*)

For **large** $d$ and $p$, storing a polynomial basis is **intractable**:
- The number of dimensions of the new feature vectors in $Z$ depends on $d$ and $p$ with a complexity $O(d^p)$
- These big data may not fit in memory

**Solution**:
**L2-regularisation** for linear regression
- Doing this, we get the following minimisation objective function:
$$\underset{v}{min} \left( \frac{1}{2}||Zv-y||^2+\frac{\lambda}{2}||v||^2 \right)$$
- The solution to this optimisation function ends up being to solve for $v$ like so:
$$v=(Z^TZ+\lambda I)^{-1}(Z^Ty)$$
The **cost** for solving for $v$ is $O(nk^2+k^3)$
- Which can be rewritten as:
$$v=Z^T(Z^TZ+\lambda I)^{-1}y$$
The **cost** of solving for $v$ is $O(n^2k+n^3)$
- If $n\ll k$, this is **much faster** to solve

So far we have just changed the way we calculate $v$ and slightly improved the calculation time
- However, here we still need to calculate $Z$ in order to work it all out. The steps we need to take are:
$$X\rightarrow Z \rightarrow v$$
- The portion of going from $X$ to $Z$ (calculating $Z$ from $X$) is **very expensive**

So, can we avoid calculating $Z$? The kernel trick:

**Kernel trick**
Note that during **training**, we get as our output this $v$ model (the weights of our model)
- During **testing**, we do the same change of basis 'trick' $\tilde{X}\rightarrow \tilde{Z}$
- we estimate $\hat{y}=\tilde{Z}v$
- So, with $v$ plugged in we get
$$\hat{y}=\tilde{Z}Z^T(Z^TZ+\lambda I)^{-1}y$$

We can rewrite $\tilde{Z}Z^T$ as $\tilde{K}$
and $ZZ^T$ as $K$
$$\hat{y}=\tilde{K}(K+\lambda I)^{-1}y$$
Now, can we calculate $\tilde{K}$ and $K$ **without calculating $Z$**?

The matrix $K=ZZ^T$ is called the **Gram matrix** $K$
- $K$ contains the **dot products** between **all training examples**

![[Pasted image 20260417182717.png]]

Additionally, $\tilde{K}=\tilde{Z}Z^T$ is called the **Gram matrix** $\tilde{K}$
- $\tilde{K}$ contains the **dot products** between **test and training** **examples**

![[Pasted image 20260417182850.png]]

For $K$ we always get an $n\times n$ matrix no matter the dimensions of each $z_{i}$:

![[Pasted image 20260417183013.png]]

- However calculating $K$ currently still requires all of the $z_{ij}$ values, which we **don't want** (requires calculating $Z$ - expensive!)
- So we would like a way to directly compute $k_{ij}$ from $x_{1}$ and $x_{n}$

For example, say we have *2d feature vectors*
$$x_{i}^T=[a~~b]$$
$$x_{j}^T=[c~~d]$$
And compute a new 2-degree basis of this form:
$$z_{i}^T=[a^2~~\sqrt{ 2 }ab~~b^2]$$
$$z_{j}^T=[c^2~~\sqrt{ 2 }cd~~d^2]$$
Can we compute $<z_{i},z_{j}>$ **directly** *without* compute first $z_{i}$ and $z_{j}$ *separately*?
$$\begin{align}
<z_{i},z_{j}>&= a^2c^2+(\sqrt{ 2 }ab)(\sqrt{ 2 }cd)+b^2d^2 \\
&=a^2c^2+2abcd+b^2d^2 \\
&=(ac+bd)^2 \\
&=(x^T_{i}x_{j})^2
\end{align}$$
Meaning you don't even know the $z_{ij}$ data! Since it all boils down to just $x_{i}$ and $x_{j}$
- This is not the same expansion as every possible change of basis, but it works the same for all of them

So how do we find this '*magic box*' $\phi(x)$ which takes the basis function and maps points $x_{i}$ and $x_{j}$ to $z_{i},z_{j}$
- This is the **kernel function $k()$** - a function which does that mapping for us!

$$<z_{i},z_{j}> = k(x_{i},x_{j})=(x^T_{i}x_{j})^2$$

There are a couple example kernel functions:
- Gaussian kernel (RBF) - **continuous space**
- Language kernel - **discrete space**

![[Pasted image 20260417213422.png]]

