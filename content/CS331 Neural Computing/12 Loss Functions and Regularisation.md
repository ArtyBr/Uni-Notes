A **loss function** measures **how much** the **predicted output** $\hat{y}$ **differs** from **target output** $y$. 
- It evaluates **how well** a network *models* the dataset

A loss function is used to **guide the training process** to find a set of parameters that reduce the error.

![[Pasted image 20260209162140.png]]

A **desirable loss function**:
- **Minimises** ($L=0$) when predicted $\hat{y}$ is **equal** to target $y$
- **Increases** when the gap between predicted $\hat{y}$ is equal and target $y$ increases
- Globally **continuous** and **differentiable**
	- Varies smoothly with changes of input/output
- **Convexity**
	- A **local minimum** of a convex function is also a **global minimum**

There are differences between a **loss** and **cost** function
- Usually, a **loss function** is used for a **single** training example
- However, a **cost function** is the *average* of the loss function over the **entire training set**.
- Sometimes, a **cost function** may include **extra penalties** (e.g. regularisation terms) for a number of training sets

In general, a **loss function** is *part* of a **cost function** 

Types of loss functions:

Regression:

- L1 Loss (Absolute Error)
	- ![[Pasted image 20260209162830.png]]
- L2 Loss (Square error)
	- ![[Pasted image 20260209162845.png]]
We can also use the **Huber Loss** which combines the L1 and L2 norm. Between some $\delta$, the **switching point**, we use the quadratic function, and outside of it, we use the linear function. 

We have a **Norm Inequality**:
- ![[Pasted image 20260209164522.png]]

And a **Norm Equivalence**
- ![[Pasted image 20260209164537.png]]

Classification:
- Log Loss (or Cross-Entropy)
- Hinge Loss

Pictorially, we can **visualise** the magnitude of the different norms on a unit grid.

![[Pasted image 20260209164938.png]]

The $L_0$ norm of a vector $x$ is defined to be the **number of non-zero entries** in $x$.
- It is an indication of **how sparse/dense** the vectors are
- $L_0$ norm is useful in many applications, such as neural computing and compressing sensing, which aims to find the sparsest solution to an under determined set of equations

Usually, our goal is to **minimise** the **loss function**

![[Pasted image 20260209165225.png]]

However, if we train it **too well** on the **training data**, it will perform **worse** on the generalised **testing data**. *Overfitting*.

We use **regularisation** to *discourage* learning more complex features by applying a **penalty** to the input parameters with larger weights
- Regularisation **constrains** the weight estimates towards zero

![[Pasted image 20260209165625.png]]

![[Pasted image 20260209165919.png]]

---

There is also **log loss** (aka **Cross entropy loss**)
- This measures the **accuracy** of a **classification model**
- Log loss **compares** **predicted output** $\hat{y}$ (Sigmoid probability between 0 and 1) with **true class** $y$, ans **penalises** the probability logarithmically based on how far it **diverges** from the true class

![[Pasted image 20260216162421.png]]

![[Pasted image 20260216162447.png]]