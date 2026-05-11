Here, we would like to learn to make a **regression** model which can make **binary classifications.** 

However, the issue we initially have is that regression models give **real**-**value** outputs, which don't exactly map to binary classifications
- We can instead use the regression model as a **proxy** or **first step** and then use the output of the regression model to inform our classification
- e.g. Use a threshold, where the natural threshold should probably be 0:

![[Pasted image 20260416152030.png]]

So what we want is a case where we create a model which can define for us a decision boundary in 1D.

![[Pasted image 20260416155226.png]]

(imagine the points are centred at the origin and the perpendicular line goes through the origin)

Say we want to classify some data in 2D and create such a model which can distinguish the classes
- If we just create a regression model, it will just follow the points, as is done by the red line
- However the red line doesn't separate the classes at all - it just tries to fit the points.
- Instead, we take the decision boundary as being the **perpendicular line** to the red one, passing through the origin.
- This is defined by the way we get positive/negative results after multiplying the weight line $w$ by the input points $x_{i}$
- Since points under the decision boundary result in $w^Tx_{i}>0$ and over as $w^tx_{i}<0$, the decision boundary itself is located at $w^tx_{i}=0$

Now we take an example decision boundary - the Boolean function
- We do this using 2d feature vectors
$$\hat{y}_{i}=w^tx_{i}$$
$$\hat{y}_{i}=w_{1}x_{i_{1}}+w_{2}x_{i_{2}}$$
What should $w$ be to correctly classify the two examples?
- One possible solution is $w_{1}=1,w_{2}=-2$

Gives us this:

![[Pasted image 20260416172830.png]]

Predictions given by $w$ can be represented as two **half-spaces**
- $H_{+}=\{x_{i}:w^Tx_{i}>0\},H_{-}=\{x_{i}:w^Tx_{i}<0\}$
	- The boundary of these half-spaces pass through the origin - **Decision boundary**
- If the training examples can be separated by a **linear decision rule**, they are **linearly separable**

So, we can visualise this as such:
![[Pasted image 20260416173011.png]]

Model $w$ can also be plotted in a 'model' space
- Each training example $x_{i}$ is associated with a half-space, they must lie in the correct half-space to be correctly classified
- The region satisfying all the constraints is the **feasible region**; if this region is non-empty, the problem is **feasible**

---

Now we investigate loss functions for such as model
- Let us attempt to minimise the square error with out ground truth as +1 or -1:
$$f(w)=\frac{1}{2}||Xw-y||^2$$
- Recall, we use $sign(w^Tx_i)$ but first we must optimise $f(w)$
- If we predict a number close to 1, we get a small error
- but if we predict a number like 100 and the ground truth is exactly 1, then we get a huge error despite our model being correct
	- However, least squares may penalise such a prediction when it gives the correct sign 'too harshly'

Instead, we define a '0-1' loss function, which tries to minimise the number of **classification errors**
- Allows for this:
	- Either the classification is wrong (loss = 1) or right (loss = 0)
	- Expressed using the L0-norm as $||sign(Xw)-y||_{0}$
		- Recall: The L0-norm is the **number of non-zero entries** in the vector

![[Pasted image 20260416180925.png]]

This shows the 'loss space' of such as model:

![[Pasted image 20260416180952.png]]

It would still give a large error for correct classifications which we don't want

Finding $w$ by minimising the 0-1 loss function is a hard problem
- The 0-1 loss function is non-convex in $w$, and the gradient is 0 everywhere
- This means that we can't tell how fast the function changes and in which direction

The solution is to find a **convex approximation** of the 0-1 loss function
- If $y_{i}=+1$, we get the label right if prediction $w^Tx_{i}>0$
- Otherwise, correct if $w^Tx_{i}<0$, or equivalently if $-w^Tx_{i}>0$

Classifying example $x_{i}$ correctly is equivalent to having $y_{i}w^Tx_{i}>0$!
- Error is given as $max\{0,-y_{i}w^Tx_{i}\}$

So, we want to **minimise** our convex approximation for the 0-1 loss function over all samples:
$$f(w)=\sum\limits^n_{i=1}max\{0,-y_{i}w^Tx_{i}\}$$
What happens if $f(w=0)=0?$
- This gives the lowest possible value of $f(w)$
- It minimises the convex approximation, but it does not seem correct when $w=0$
	- i.e. if $w=0$, the loss is zero, but we are not learning a model!
- aka this is a **degenerate solution**

Solutions:

**Hinge loss**
Provides an *upper bound* on the loss

![[Pasted image 20260416190040.png]]

Here replace the $y_{i}w^Tx_{i}>0$ function with $y_{i}w^{T}x_{i}\geq1$

Another could be the **Sigmoid function**, which is robust to having *many* outliers and *extreme* outliers

However, it is NP-hard to minimise

![[Pasted image 20260416190255.png]]

**Logistic loss**:

An alternative to the approximation of the 0-1 loss, which was degenerate, is as such:
- We can **smooth** the approximation with a log-sum-exp:
$$max\{0,-y_{i}w^Tx_{i}\}\approx \log(\exp(0)+\exp(-y_{i}w^Tx_{i}))$$
Which gives the logistic loss:
$$f(w)=\sum\limits^n_{i=1}\log(1+\exp(-y_{i}w^Tx_{i}))$$

![[Pasted image 20260416190410.png]]

---

**The Perceptron algorithm**

One of the first learning algorithms was the perceptron, which can learn to classify **linearly separable data**

![[Pasted image 20260417115946.png]]

It **searches** for a $w$ such that predictions (as defined) $\hat{y}_{i}=y_{i}\forall i$
**Algorithm**:
- $w^0=0$ at iteration $t=0$
- Predict $y_{i}$ for all $i$ in any order until $\hat{y}_{i}\neq y_{i}$ is encountered
	- If $\hat{y}_{i}\neq y_{i}:w^{t+1}\leftarrow w^{t}+y_{i}x_{i}$
- Continue predicting $y_{i}$ for all all $i$ in any order **until** **no errors**

Why $w^{t+1}\leftarrow w^{t}+y_{i}x_{i}$?
- If $y_{i}=+1$ and prediction is $\hat{y}_{i}=-1$, we can add $x_{i}$ to $w$ so that $w^{T}x_{i}$ is larger:
- $(w^{t+1})^{T}x_{i}=(w^{t}+y_{i}x_{i})^{T}x_{i}=(w^{t}+x_{i})^{T}x_{i}+x_{i}^{T}x_{i}$
- $=(\text{prediction at t})+||x_{i}||^2$

The algorithm can find a **perfect classifier** in a finite number of steps for linearly separable data

---

**Multi-Class linear classification**
If we want to be able to identify something as being one of a few things:
- Image of a vehicle as being a car, truck or bike? Rather than just one or the other?

The naive way of doing it would be to create a '**One vs All**' scheme
- Train multiple linear models for binary classification under the premise 'Bus/No bus', 'Car/No car' etc.
- For $k$ classes, this requires $k$ binary classifiers
- Apply the $k$ classifiers to the example $x_{i}$ to get a numerical value for each class $c$
- Return the class $c$ with the **highest numerical value**

![[Pasted image 20260417121029.png]]

For each class $c$ we compute $w_{c}^Tx_{i}$
- For all classes, we get a set of results $\hat{y}_{i}=Wx_{i}$. For an image of class 1, we would get $\hat{y}_{i^T=[+1,-1,-1]}$
- How do we select the class if we do not get $+1$ and $-1$ values exactly?
- Use the maximum value, then $\hat{y}_{i_{1}}$ is the highest value in and the label to select is class 1
- Training does not guarantee that the largest value in $\hat{y}_{i}$ comes from the classifier that should correctly assign the label to the image - each classifier is just attempting to get $+1/-1$

So how can we define a loss that encourages the largest $w_{c}^Tx_{i}$ value to be the correct prediction?

In essence, we want our **loss** to be:
$$w^{T}_{y_{i}}x_{i} \geq \underset{c}{max}\{w^T_{c}x_{i}\}$$
OR
$$0\geq -w^T_{y_{i}}x_{i}+\underset{c}{max}\{w^T_{c}x_{i}\}$$
This could be the constraint?
- However it could be degenerate
- We can apply log-sum-exp to smooth it:

$$-w^{T}_{y_{i}}x_{i}+\log(\sum\limits_{c=1}^{k}e^{(w_{c}^{T}x_{i})})$$
- if $w_{c}=0$, this now gives a loss of $\log(k)$
Aka **softmax loss**: The loss for **multi-class logistic regression**

Recall the Softmax function:

![[Pasted image 20260417123354.png]]

- $z_{i}$ is converted to a **vector of probability values** $\in [0,1]$
- Softmax loss function **maximises** the **predicted value** for the correct class, $y_{i}^{correct}=w^{t}_{y_{i}}x_{i}$, after mapping all predicted values for all classes to probability values $\in[0,1]$

![[Pasted image 20260417123516.png]]

We **sum** the loss over all examples and (optionally) add L2-regularisation:

![[Pasted image 20260417123549.png]]

This is **convex** and **differentiable** in $W$, so we can then find a solution

This multi-class linear classifier then provides the **intersection** of all the **half-spaces**:
- Divides $x_{i}$ space into **convex regions** like K-means:

![[Pasted image 20260417123642.png]]