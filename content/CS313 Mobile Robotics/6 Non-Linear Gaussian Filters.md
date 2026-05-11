In the real world most systems are **non-linear**
- The **process** model and **measurement** model can be described as:
	- $x_{t}=g(u_{t},x_{t-1})$
	- $z_{t}=h(x_{t})$
	- Where $g$ and $h$ are **non-linear** functions. The function $g$ has **replaced** the matrices $A$ and $B$ in the previous equation for the transition model and the function $h$ replaces the matrix $C$

![[Pasted image 20260210151353.png]]

Once you go non-linear, the 'Gaussian world' essentially **collapses**

![[Pasted image 20260210151720.png]]

**Extended Kalman Filter** (EKF)

One thing we can do is to **approximate** the function at the point we're measuring using a **Taylor Expansion**

![[Pasted image 20260210151744.png]]

Essentially, a Taylor function:
- Is a **series expansion** of a function $f(x)$ about a point $a$
	- ![[Pasted image 20260210151933.png]]
- ![[Pasted image 20260210151940.png]]
The first-order Taylor polynomial is the **linear approximation** of the function, it is the equation of the **tangent line** of a function at a fixed point.


So, when using this for our Kalman filter:
- We want to make a prediction which is based on the control input of our current state
- We then want to evaluate that based on the best guess based on our previous state
- ![[Pasted image 20260210152212.png]]

We can **capture** this using a **matrix of partial derivatives** called a **Jacobian matrix**.

![[Pasted image 20260210152244.png]]

We can then also do the **same thing** for our **correction** (measurement update)

![[Pasted image 20260210152342.png]]

So, we get to the **algorithm** that operates on this.
- Once linearisation has taken place, the **EKF algorithm** is very similar to the Kalman Filter algorithm.
	- The key **difference** is that to calculate the **means** we **evaluate** the functions $g$ and $h$ at the state deemed **most likely** at the **point of linearisation**
	- The Jacobian matrices $G$ and $H$ are used instead of $A$ and $C$

![[Pasted image 20260210152501.png]]

So, we're creating a **linear approximation** of the **non-linear function** at particular points and then plugging those approximations/points in to the Kalman filter algorithm

Now, the extent to which the approach works **depends** on two **factors**
- The **degree of uncertainty**
	- With **high uncertainty**:
		- ![[Pasted image 20260210152645.png]]
		- Gaussian is quite spread (bottom right)
		- Push it through the non-linear function
		- We get a 'tri-modal' distribution
		- Take mean and variance of those samples
		- Gets the blue line
		- Then we do linear approximation at a point on that, which gives the red line
		- We can see that the Blue and Red distributions are quite different, which isn't what we want (top left)
	- Compared with **low uncertainty**:
		- ![[Pasted image 20260210152812.png]]
		- Looks a lot better here in terms of how the blue and red distributions compare.
- The **degree of local non-linearity** of the functions being approximated
	- ![[Pasted image 20260210152955.png]]
	- When you take your linear approximation at the points with high linearity, we get a good representation of the gaussian in that case.
	- However if we were to take the mean at a **different location** (less linear), we can see the differences come in which isn't what we want:
		- ![[Pasted image 20260210153044.png]]
Overall:
- Very **efficient** (same as KF)
- Works very well in practice
- Often fail when nonlinearities are significant
	- Propogation of uncertainty
- Have to calculate the Jacobian

---

**Unscented Kalman Filter**

We use two other values than just the initial mean sample. We then reconstruct the Gaussian with all of the samples.

![[Pasted image 20260210153359.png]]

Monte Carlo Approach?
- Why not just pass lots of points through the function at each time step, and then compute the mean and covariance?
	- This gets very computationally expensive, and takes away from what the KF is trying to do
- The UKF uses a similar technique but reduces the amount of computation needed by using a **deterministic method** of choosing the sample points (sigma points)

![[Pasted image 20260210153600.png]]

Unlike the EKF, the UKF **does not** **approximate** the **non-linear process** and **observation** models, it uses the **true nonlinear models** and rather **approximates** the **resulting distribution**

Overview:
- Compute a **set of sigma points** $x_{1}, ..., x_{n}$
- Each sigma point has a weight $w_i$
- Ensure $\sum\limits^{n}_{i=1}w_{i}=1$
- Transform the points through the non-linear function - result is **not gaussian**, i.e. obtain $f(x_{1}), ... f(x_{n})$
- Compute a Gaussian from the weighted points
- Mean $\mu=\sum\limits^{n}_{i=1}w_{i}f(x_{i})$
- Covariance $\Sigma=\sum\limits^{n}_{i=1}w_{i}(f(x_{i})-\mu)(f(x_{i})-\mu)^T$

How many sigma points?

![[Pasted image 20260210153932.png]]

![[Pasted image 20260210153938.png]]

![[Pasted image 20260210153949.png]]

![[Pasted image 20260210153957.png]]

![[Pasted image 20260210154246.png]]

How do we choose the weights?

![[Pasted image 20260210154348.png]]

![[Pasted image 20260210154356.png]]

![[Pasted image 20260210154404.png]]

This results in a Gaussian:

![[Pasted image 20260210154415.png]]

In practice, the UKF ends up working a lot better for different cases of uncertainty than the EKF:

![[Pasted image 20260210154741.png]]

![[Pasted image 20260210154748.png]]

However when the uncertainty is low, the two respond quite similarly. (Maybe pointless to use UKF)

When Means are in linear area:

![[Pasted image 20260210154822.png]]

But when it's in a non-linear area:

![[Pasted image 20260210154837.png]]

Overview:
- Still **very efficient** (but slower than EKF by a constant factor - for choosing the sigma points)
- Better than EKF
	- Produces **equal** or **better** results, where the improvement depends on the uncertainty of the robot and nonlinearities in the state transition and measurement functions
	- Non need to computer Jacobians

However, it is **still not optimal**
- Like all Kalman-style filters, the UKF only tracks the mean and covariance, so if the true posterior is **highly non-Gaussian** or **multi-modal**, the **approximation** will still **break down**

