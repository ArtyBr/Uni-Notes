There might be more than one possible 'perfect classifier' for a given dataset
- But are they all **equally good?**

![[Pasted image 20260417140251.png]]

We want a **maximum margin classifier** 
- A hyperplane that separates two classes and **maximises the distances** to the closest point from either class, i.e. maximises the margin of the classifier

Ensuring that a decision boundary is **not too close** to any data point leads to a **better generalisation** on the test data
- If the test data are **close to the training data**, then our max-margin classifier is expected to perform well (of course)
- However, if the test data is **not close** to the training data, the margin leaves **more room** before making an error

![[Pasted image 20260417140445.png]]

The data point **closest** to the decision boundary are the ones that **determine** where the decision boundary is
- We only need these data points to find that boundary
- aka *Support vectors*

Numerical predictions **along the boundary** are 0
- Predictions **away from** the boundary **increase linearly** towards $+\infty$ and $-\infty$
- Let us ensure the **support vectors** are assigned a prediction of **exactly** $+1$ or $-1$

Recall: the decision hyperplane is **orthogonal** (perpendicular) to $w$
- $w^*=\frac{w}{||w||}$ is a **unit vector** pointing in the **same direction** as $w$
- i.e. the normal vector to the hyperplane; $d$ is the perpendicular distance from $x_{i}$ to $x_{j}$

![[Pasted image 20260417141016.png]]

Now we want to find the margin from the decision boundary to the support vectors
$$w^{T}\left( x_{j}+d \frac{w}{||w||} \right)=1$$
If we follow the maths, we get:
$$d||w||=2$$
$$d=\frac{2}{||w||}$$
So, in order to **maximise $d$**, what can you do?
- We can **minimise** $||w||$ - the **length of $w$**

Making the model **simpler** makes it **better**!

SVM **maximises** the margin $\frac{2}{|w||}$ s.t. two constraints:
$$w^{T}x_{i}\geq_{1}~\text{for}~y_{i}=+1$$ $$w^{T}x_{i}\leq-1~\text{for}~y_{i}=-1$$
Minimise violation of constraints:

![[Pasted image 20260417141607.png]]

Basically, all point between the boundary and the positive support vector should be 0-1, same for negative (0- (-1)), and all points further should 1 - infinity etc.

Minimising these violations is the **hinge loss**!
- Classifying sample $i$ correctly is equivalent to $y_{i}w^{T}x_{i}>0$
- replace $y_{i}w^{T}x_{i}>0$ with $y_{i}w^{T}x_{i}\geq_{1}$ for the hinge loss

We have two competing objectives
- Let us define a minimisation problem for the margin:
- **Maximising** $\frac{1}{||w||}$ is equivalent to **maximising** $||w||^{-1}$ which is equivalent to **minimising** $||w||$
- Is $||w||$ a smooth function?

![[Pasted image 20260417142137.png]]

We would like to use $||w||^w$ since we can still find a boundary defined by $w$ that classifies all points correctly for linearly separable data - using $||w||^2$ just **emphasises** even more the **minimisation** of $||w||$

Minimise the **following objective function** for all examples:
$$f(w)=\sum\limits_{i=1}^{n}max\{0,1-y_{i}w^{T}x_{i}\}+\frac{1}{2}||w||^2$$
However, we also want to do **regularisation** - centre the data and find the **smallest one in size** which will most likely behave better in practice

The margin/violation trade-off is usually controlled with parameter $\lambda$
$$f(w)=\sum\limits_{i=1}^{n}max\{0,1-y_{i}w^{T}x_{i}\}+\frac{\lambda}{2}||w||^2$$
aka the **L2-regularised hinge loss**

---

Multi-class classification with SVMs

Again, can do the same naiive way as with linear classifiers - make separate SVM models for each class ('one vs all')

So, we want to define a loss that **encourages the largest $w_{c}^{T}x_{i}$** to be the **correct prediction** $w^{T}_{y_{i}}x_{i}$
- This can be defined as:
$$w^{T}_{y_{i}}x_{i}>w_{c }^{T}x_{i}$$
- However, this loss can become degenerate since we can just make a 0 model which would by definition be correct (which we don't want)
Instead, we use:
$$w_{y_{i}}^{T}x_{i}\geq w_{c}^{T}x_{i}+1$$
Meaning that we want the **distance** to be *at least 1*

So now we have multiple models, how do we aggregate them together to get an overall answer and measure the violation of our constraint?
- First, we could add them all together, and we want the sum of them to be small.
	- '*sum*' rule
- Another school of thought is to make every single relationship to be small (rather than just the sum of all of them)
	- '*max*' rule

![[Pasted image 20260417143444.png]]