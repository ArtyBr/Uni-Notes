We want to discover relationships between **numerical variables**

**Linear regression** is a statistical tool for modelling the **relationship** between some **explanatory variables** and some **real-valued outcome**

**Objective**: Find a simple pattern in the data; a **functional relationship** between $X$ and $y$

Example:

![[Pasted image 20251023101204.png]]

We would like to find a **function** that **predicts** a baby's birth weight based on ultrasound measures of the baby's head circumference
- $X\in \mathbb{R}^d$
- $y$ is the weight in grams

Hypothesis: the relationship between $X$ and $y$ is **linear**
- Our **hypothesis space** is **all possible lines**
- ![[Pasted image 20251023101344.png]]
Different **parameters** (this is a parametric model) give us different lines

![[Pasted image 20251023101410.png]]

We need to find the **right $w$** by measuring how well our line **fits** the training data

We define a **loss function**
- We would like to find a line as **close as possible** to **all the points**

We can use a **squared loss** function for all training data:

![[Pasted image 20251023101523.png]]

![[Pasted image 20251023101532.png]]

The algorithm below finds the **best linear predictor** with respect to the squared loss:

![[Pasted image 20251023101601.png]]

To **minimise** a function, we find the **derivative**
- We find values where $f`(w)$ is 0
- Choose the $w$ that results in smallest value for $f(w)$ and check that $f''(w)$ is positive for that $w$

![[Pasted image 20251023101709.png]]

We can not that the problem:

![[Pasted image 20251023101806.png]]

Has the **same minimizer** as these problems:

![[Pasted image 20251023101823.png]]

We can multiply $f(w)$ by **any positive constant** and **not change the solution**
- Slope, or **gradient** will still be 0 at the **same locations**

So, to find the $w$ that minimizes the sum of squared errors:

![[Pasted image 20251023102232.png]]

![[Pasted image 20251023102244.png]]

This will give us the model that fits our training data

![[Pasted image 20251023102633.png]]

The predicted line is the red dashed one
Our new predicted value based on new data $\tilde{x_{i}}$ is in green

However, our model doesn't account for the $y$-intercept

Our linear model is $\hat{y_{i}}=wx_{i}$, instead of 
- $\hat{y_{i}}=w_{0}+w_{1}x_{i}$ with y-intercept $w_{0}$

![[Pasted image 20251023102822.png]]

There is a way to **adda a bias** by changing the features:
- Make a **new matrix $Z$** with an **extra feature** that is always = 1

![[Pasted image 20251023102907.png]]

- Use $Z$ as the **features** for linear regression
$$\hat{y_{i}}=v_{1}z_{i1}+v_{2}z_{i2}=w_{0}+w_{1}x_{i1}$$
- This 'trick' allows us to 'ignore' the bias in the calculations

We may also need to account for **even more features** $d$

For **two features**, we get a **2d linear function**
$$\hat{y_{i}}=w_{1}x_{i1}+w_{2}x_{i2}$$
We now fit a 2D hyper plane

![[Pasted image 20251023103148.png]]

Generally, for $d>1$ features, the $d$-dimensional linear model is:
$$\hat{y_{i}}=w_{1}x_{i1}+w_{2}x_{i2}+...+w_{d}x_{id}$$
$$=\sum\limits_{j=1}^{d}w_{j}x_{ij}$$
Or, alternatively in **vector notation** (all vectors are **column vectors**)
$$\hat{y_{i}}=\text{w}^{T}\text{x}_{i}$$
Therefore, the linear least squares model in $d$-dimensions **minimizes** the **following loss function**:
$$L(w)=f(w)=\frac{1}{2}\sum\limits_{i=1}^{n}(w^{T}x_{i}-y_{i})^{2}$$
How do we find the **best vector $w$** in $d$ dimensions?
- Could set the derivative for each variable (partial derivative) to 0? too complex!
- Instead, we can use **matrix and vector operations**

Our **ground truth**, $y$, is an $n\times1$ vector containing ground truth $y_{i}$ in position $i$
- Our training example $x_{i}$ is a $d\times1$ vector containing $d$ features - note these are 1-column vectors
- Our training dataset, $X$, is an $n\times d$ matrix with $x_{i}^T$ in row $i$

![[Pasted image 20251023103909.png]]

The prediction of example $i$ is given be the **scalar**
$$\hat{y_{i}}=w^{T}x_{i}$$
The prediction for **all $n$ examples** in $X$ is the $n\times1$ vector $\hat{y}$

![[Pasted image 20251023104020.png]]

The residual vector $r$ is an $n\times1$ vector containing the **difference** between **predictions** and **ground truth values**
$$r=\hat{y}-y = Xw-y$$
Least squares can be written as the **squared L2-norm** of the **residual**

![[Pasted image 20251023104336.png]]

We can therefore find the solution:

![[Pasted image 20251023104359.png]]

![[Pasted image 20251023105103.png]]

So our least square objective function, or **loss function**, is
$$f(w)=\frac{1}{2}||Xw-y||^{2}$$
The minimizers of this objective function are the **solutions** to the system:
$$X^{T}Xw=X^{T}y$$
$$w=(X^{T}X)^{-1}(X^{T}y)$$
if $X^{T}X$ is invertible

There also exist some **issues** with **least squares**:
- Sensitivity to **outliers**
- Data can be big to store: $X^{T}X$ may be large
- May be costly - requires $O(nd^{2}+d^{3})$ operations
	- creating a matrix $X^{T}X$ costs $O(nd^{2})$
	- Solving a $d\times d$ system of equations costs $O(d^{3})$
		- Since $X^{T}Xw=X^{T}y$ is a set of $d$ linear equations
- It might predict values **outside the range** of $y$ values
- It assumes a **linear relationship** between $X$ and $y$
- The solution might **not be unique**

![[Pasted image 20251023132833.png]]

A function is **convex** if the **area above the function** is a **convex set**
- Any line segment we draw between two points lies **above the curve**
- When finding the minimum values, a local minimum suffices $\rightarrow$ global minimum
	- This is what makes convex optimisation easy - it suffices to find a **local minimum**

![[Pasted image 20251023133948.png]]

The solution found from linear regression is **convex** *because* the loss function seeks to find the least **square error**, which gives a **quadratic function**, which will always be convex. 

---

Ok, now can we do this non-linearly?

How to use linear least squares to fit a quadratic model to 1d features?
$$\hat{y_{i}}=w_{0}+w_{1}x_{i}+w_{2}x_{i}^{2}$$
- By changing the features (change of basis):

![[Pasted image 20251023134254.png]]

- Fit new parameters $v$ under change of basis: solve for $Z^{T}Zv=Z^{T}y$
- We get a linear function of $v$ (our solution) but a quadratic function of $x_{i}$:
$$\hat{y_{i}}=v_{1}z_{i1}+v_{2}z_{i2}+v_{3}z_{i3}$$
To predict **new data** $\tilde{X}$, we compute $\tilde{Z}$ from $\tilde{X}$ and then $\hat{y}=\tilde{Z}v$

![[Pasted image 20251023134533.png]]

This change of basis is a **non-linear feature transform**
- The $z_{i}$ are the coordinates in the new basis (or feature space) for the training example $i$
We can fit **any polynomial** of degree $p$ by using the transformed features:

![[Pasted image 20251023134813.png]]

We now have two cases:
- Linear regression **with original features**
	- Use $X$ as the $n\times d$ **data matrix** and $w$ as the **parameters to be learned**
	- Find a $d$-dimensional $w$ by minimising the squared error:
$$f(w)=\frac{1}{2}||Xw-y||^{2}$$
- Linear regression with **non-linear feature transormation**
	- Use $Z$ as the $n\times k$ **data matrix**, and $v$ as the **parameters to be learned**
	- Find a $k$-dimensional $v$ by minimising the squared error:
$$f(v)=\frac{1}{2}||Zv-y||^{2}$$


For non-linear data, as the **polynomial degree** used to change the bases **increases**, the **training error goes down**

![[Pasted image 20251023135141.png]]

However, **approximation error** tends to go **up** - **Overfitting** with large $p$
- **Validation** or **cross-validation** is commonly used to select the **degree** $p$

With $p=7$, the weights in the solution $v$ are very large in this example
- This change of basis is very sensitive to the data
- We can regularise the weights in $v$, so they are smaller, less sensitivity to the data $\rightarrow$ reduce overfitting

---

## L2 Regularisation

- Standard **regularisation strategy**
- **Objective function** of least-squares for $d$-dimensions

![[Pasted image 20251023135534.png]]

Essentially, *augment* the loss function *by* the **sum of the squares** of the **coefficients** themselves
- **Large values** $w_{j}$ tend to lead to **overfitting**
- Balances getting **low training error** vs. having **small values** $w_j$
	- e.g. increase the training error by making $w$ small
- Helps *reduce* **overfitting**
	- Increasing training error vs. decreasing approximation error
How to choose $\lambda >0$?
- Use **validation** or **cross-validation**
	- Maybe choose a small step-size to check different values of $\lambda$, experiment to find what step sizes give good cross-validations
- Large $\lambda? \rightarrow$ more penalty added

![[Pasted image 20251028134937.png]]

