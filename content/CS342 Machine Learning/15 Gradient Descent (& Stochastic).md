**Minimisers**
Let's take the case of least squares:
- We want to optimise the following **objective function** or **loss**
$$f(w)=\frac{1}{2}\sum\limits_{i=1}^{n}(w^{T}x_{i}-y_{i})^{2}$$
Recall: we are in fact interest in finding a **minimiser** $w^{*}$
- A **set of parameters** $w$ that achieves the **minimum value** of $f(w)$
- The **set** over which we *search for an optimum* is called the **domain**
$$\underset{w\in \mathbb{R}^{d}}{\text{argmin}} \frac{1}{2}\sum\limits_{i=1}^{n}(w^{T}x_{i}-y_{i})^{2}$$
A set of parameters $w$ with $df(w)=0$ is a **critical point** or stationary point
- The slope is zero, hence the tangent plane is flat i.e. no inclination
- These points are *either* a local **minimum** or **maximum**
- If *minimisation* - find global **minimum**

![[Pasted image 20260420104629.png]]

Here, we plot a **loss landscape** of the $f(w)$ function
- However, in some challenging cases (i.e. non-convex functions), *any* stationary point (global or local minimum) where $df(w)=0$ suffices

Finding a **global minimum** is *extremely hard* (NP-hard!), so instead we focus on finding **local minima**

---

One solution to this is an algorithm called **gradient descent**
- We *take* the gradient/**derivative** and *move down* the **derivative** to the **next point** (improve quality of solution)

![[Pasted image 20260420105101.png]]

Gradient descent **can converge** to the **global minimum** *if* $f(w)$ is **convex**
- For non-convex functions, it can help us find a **local minimum** where $df(w)=0$

How does gradient descent work?

Given parameters $w^0$, $df(w^{0})$ provides information about how to get **closer to the minimum value** of $f(w)$ from $w^{0}$
- i.e. by using:
$$w^{1}=(w^{0}-df(w^{0}))$$

![[Pasted image 20260420105349.png]]

We keep iterating by performing this step over and over until we get to the other side (positive slope)
- We'll start 'bouncing' (going back and forth from a positive to negative gradient) at the bottom
 
![[Pasted image 20260420105550.png]]

In order to optimise this further, we can **control** the **step size** (*learning rate*) $\alpha$ 
- This $\alpha$ can also change over time
- So it would be good to **decrease** this alpha over time
$$w^{t+1}=w^{t}-\alpha^{t}df(w^{t})$$
Further, we would like to **stop** if *no progress* is being made.
- Stop if:
- $w^{t+1}=w^{t}$ 
- or $||df(w^{t})||\leq \epsilon$, where $\epsilon$ is a small constant
	- Very small gradient - basically not moving every step

---

Our objective function looks like so in **matrix notation**:
$$f(w)=\frac{1}{2}||Xw-y||^{2}$$
This **can be solved** *analytically* by finding the **minimisers** of $X^{T}Xw=X^{T}y$
- However this has complexity $O(nd^{2}+d^{3})$ which is **prohibitively expensive** in most cases

So **gradient descent** is a much more efficient way of solving it:

![[Pasted image 20260420110505.png]]

However the next question is **how many time-steps** do we need?
- Since that case $O(nd)$ is **per-iteration**
- In real life, the number of times we need to run the iterative step is much smaller than the time taken for the analytical solution.

---

At iteration $t$, gradient descent computes:
$$w^{t+1}=w^{t}-\alpha^{t}df(w^{t})$$
Using **least squares** as an example, we have:
$$df(w)=X^{T}(Xw-y)$$
$$df(w)=\sum\limits_{i=1}^{n}(w^Tx_i-y_i)x_i$$
The **cost** of computing the gradient is **linear** in $n$
- For a **large training dataset**, gradient descent can be **very expensive**

**Stochastic gradient descent** (SGD) is an *alternative* to gradient descent for large dataset
- It is similar to regular GD, however it uses the gradient of the objective function defined by using only **one randomly-selected training sample** $i$

The cost of computing a gradient is now **independent of $n$**
- Iterations are $n$ times **faster** than those using gradient descent

Well-suited for minimising loss functions that involve **summing** over **all training samples**
- e.g. *least squares* or *logistic regression*

![[Pasted image 20260420111907.png]]

Essentially, where we have **multiple dimensions** in which we would have to calculate the gradient of **all directions** at each step (all data points), we can instead choose just **one direction** at each step and work it out for that one:

![[Pasted image 20260420112119.png]]

In the maths, it's essentially the same as GD but we replace $f(w)$ with $f_{i}(w)$, meaning we only work out the **gradient** in **one direction** (for *one feature*):
$$w^{t+1}=w^{t}-\alpha^{t}df_{i}(w_{t})$$
However, while it's much more efficient, SGD has a very important **drawback** in that we are using a **different objective function** to compute the gradient
- The gradient information may points in the **wrong direction**
- It may not lead us to the minimiser $w^*$ of the actual objective function
	- i.e. the one defined for all training examples
However SGD is **expected** to change $w$ in the right direction, **on average**, if used over a **large set of examples**
- However this is not guaranteed - only expected

![[Pasted image 20260420112621.png]]
Visualisation of difference between GD and SGD

This is because **each iteration**, due to the random nature of the objective function being picked, we are calculating the gradient of a **different function** every time
- So we are choosing a **different function** to follow every time, and switch between them at each step

The **whole picture**:

![[Pasted image 20260420113137.png]]

Out of **all functions**, this displays where:
1. All functions are pointing in the **same direction** 
	- This is the **ideal case** - they all **help us** in a sense
2. Functions are pointing in opposite / different directions
	- This causes **chaotic behaviour** (challenging region) - we don't know if we're going to go towards the optimal $w^*$ or away from it

---

Now we look at how the **step size** affects the **chaotic region**
- By **setting different** $\alpha$, we can **control** the **radius** of this chaotic region

![[Pasted image 20260420113504.png]]

By reducing $alpha$ at each iteration, we **shrink** the radius of the **chaotic region** as we get closer to our optimised weights

![[Pasted image 20260420113702.png]]

However the cost of doing this is that the rate of moving down the gradient becomes **slower**, so this needs to be *controlled* effectively
- Decreasing the learning rate too quickly would mean that SGD is **not fast enough** to even *reach* the 'ball'

We can mathematically prove that SGD **converges** to a **stationary point** (global or local minimum) *if*:
$$\dfrac{\sum\limits^{\infty}_{t=1}(a^{t})^{2}}{\sum\limits^{\infty}_{t=1}\alpha^{t}}\approx0$$
Where the **numerator** ($\sum\limits^{\infty}_{t=1}(a^{t})^{2}$) is the '*effect of variance*'
And the **denominator** ($\sum\limits^{\infty}_{t=1}\alpha^{t}$) is '*how far we can move*'
- This all **requires** that $\alpha<1$

To **satisfy** this equation, a common practice is to use a sequence of values, $\alpha^{t}=O(\frac{1}{t})$ for $t>0$
$$\sum\limits^{\infty}_{t=1}(a^{t})^{2}=\sum\limits^{\infty}_{t=1}\dfrac{1}{t^{2}}=\text{small value}$$
$$\sum\limits^{\infty}_{t=1}\alpha^{t}=\sum\limits^{\infty}_{t=1}\dfrac{1}{t}=\text{large value}$$
For example: $\alpha^{t}=\frac{0.001}{t}$

Unfortunately, in practice, this is *not* a *strong strategy*; step sizes become **very small very fast**
- A stronger strategy is to set $\alpha^{t}=\frac{\alpha}{\sqrt{t}}$ where $\alpha<<1$
- For example, $\alpha^{t}=\frac{0.001}{\sqrt{t}}$

---

Now how can this be applied to **recommender systems** to solve for $W$ and $Z$?

![[Pasted image 20260420114840.png]]

