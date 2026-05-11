 Sometimes we want to work in a continuous state space (rather than a 'discrete filter')
- So we can use 'Gaussian filters'

---

**Gaussian Distributions**:
A Gaussian distribution can be described by $\mu$, the mean and $\sigma^2$, the variance.

There can be two cases, with one variable (univariate) and multiple variables/dimensions (multivariate)

![[Pasted image 20260209141549.png]]

The overall notations are the most important, that in **univariate** case, we have a single magnitude $\mu$ and $\sigma^2$, whereas in **multivariate** we have a **vector** $\mu$ with same dimensionality as $x$ and a **covariance matrix** $\Sigma$ 

Important to note:
- A **linear transformation** of a Gaussian random variable **yields** **another** **Gaussian**

![[Pasted image 20260209141624.png]]

Also, if **two independent random variables** $x$ and $y$ are **normally distributed**, their **sum** is **also normally distributed**

![[Pasted image 20260209141658.png]]

Also, the **product** of the same will also be normally distributed and the **mean** will be a **weighted sum** of the means of the two distributions, **weighted** by the **proportion of information** in **each**

![[Pasted image 20260209141831.png]]

These properties are important because:
- We can **stay in the Gaussian world** if we start with Gaussians, only considering linear transformations or summations of Gaussians

**However**: Since Gaussians are **unimodal**, they possess only a **single maximum**; this can be readily applied to **robot localisation**
	- E.g. where the robot posterior is focused around the true state with some amount of uncertainty, these posteriors are of little use in problems that can have **multiple** hypotheses

---

**Kalman Filter**

Operates in a **continuous state space** (but **discrete time**)

Everything is **linear**

Here, we **assume**:
- **Linear control model**
	- If I don't apply any new controls, how will my state change given the current situation?
	- ![[Pasted image 20260209142427.png]]
- **Linear measurement model**
	- ![[Pasted image 20260209142434.png]]
- **Equally important**:
	- **Start** with a Gaussian
	- ![[Pasted image 20260209142653.png]]

![[Pasted image 20260209142703.png]]

Next, we analyse the **prediction** step in the Kalman Filter:

- We can **relate** the Kalman filter to eh **prediction step** from the Bayes filter.

![[Pasted image 20260209142908.png]]

The **results** is a **new Gaussian** with **mean** $\mu_{t}=A_{t}\mu_{t-1}+B_{t}u_{t}$
and **covariance** $\Sigma_{t}=A_{t}\Sigma_{t-1}A^{T}_{t}+R_{t}$

Next, we have the **update** step:
- We **update** the **predicted belief** by **incorporating** the measurement $z_t$

![[Pasted image 20260209143240.png]]


This can all be summed up in the **algorithm**:

![[Pasted image 20260209143604.png]]

---

1D example:

We take an **initial state** $p(x_{0})$

![[Pasted image 20260209143730.png]]

We have an initial or predicted belief shown in red. This is a Gaussian with mean and variance shown in $N$. The bar signifies that this is an **estimated value** or a **value before measurement**. The total amount of information carried by this is given by $I_prior=1/\sigma^2_t$

![[Pasted image 20260209143851.png]]

We **obverve** sensor measurement $z_t$ shown by the **green distribution**.
This is a Guassian with mean and variance shown in $z_t$. The total amount of information carried by this is given by $I_{obs}=1/\sigma^{2}_{obs,t}$

![[Pasted image 20260209143954.png]]

They are combined to give a **new belief** shown in blue.

![[Pasted image 20260209144011.png]]

And to get the new variance:

![[Pasted image 20260209144302.png]]

Basically, we will move more towards the one out of (predicted vs measured) based on which one is **more certain** (aka has smaller variance).

We end up having this '**Prediction-Correction**' cycle. 
- We can extend this with multiple dimensions by simultaneously applying the weighted-average update to many coupled variables

![[Pasted image 20260209144952.png]]

![[Pasted image 20260209144932.png]]



---

![[Pasted image 20260209145231.png]]

![[Pasted image 20260209145238.png]]

![[Pasted image 20260209145244.png]]

