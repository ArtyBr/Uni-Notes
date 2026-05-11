The **gradient** is the **derivative** of a multi-variable function

![[Pasted image 20260216162157.png]]

**Gradient descent** is an **iterative algorithm** that finds a **minimal value $v$** of the function $f$ near a given initial point $x_{0}$, such that:
$$v=\underset{x}{\text{min}}f(x)$$
The **key idea** is taking **repeated steps** in the **opposite direction** of the **gradient** $\nabla f$ of $f$ at the current point, because this is the  direction of **steepest descent**

![[Pasted image 20260216163847.png]]

![[Pasted image 20260216163900.png]]

$\alpha$ is the **learning rate** (between 0 and 1)
- Controls **how much** we **adjust** the **weights** with respect to the **loss gradient**

![[Pasted image 20260216163957.png]]

How do we adjust the weights and biases of the neural network using gradient descent in order to minimise our loss?

---

We can use a **dependency graph** to represent how **variables** in an expression are **dependent on each other**
- Useful for visualising **chain rules** for partial derivatives of multivariable functions
- In a dependency graph, each **node** is a **variable** in an expression.
- Each **edge** $(x,y)$ denotes '$x$ *affects* $y$', carrying a **value** labelled as $y_x$ (i.e. the partial derivative of $y$ with respect to $x$)

![[Pasted image 20260216164805.png]]

We use the **chain rule** to work out the effects of one variable on another:

**Univariate**:
- ![[Pasted image 20260216164905.png]]
- Example:
	- ![[Pasted image 20260216165238.png]]

**Multivariate**:
- ![[Pasted image 20260216164922.png]]
- Example:
	- ![[Pasted image 20260216165257.png]]- 

**Backpropogation** is an effective way of training a neural network, with the aim of **minimising the cost function** by **adjusting the weights and biases** of the network
- The level of adjustment is determined by the **gradient** of the **cost function** with respect to the weights and biases through a 'chain rule' method

![[Pasted image 20260216165839.png]]

Basic steps for training a NN:

**Repeat**:
- Use **forward propagation compute predicted output $\hat{y}$**
- Use **loss function** to measure the error between $\hat{y}$ and target (truth) $y$
- Use **backpropagation** to minimise the loss function $L(\hat{y}, y)$ by adjusting the weights and bias through **gradient descent**
**Until** $L(\hat{y}, y)$ is minimised

Next, we explain how backpropagation works based on a **simple example**

![[Pasted image 20260306104746.png]]

To train this network, our goal is to minimise the loss function $L(w,b)$, by iteratively adjusting $w$ and $b$ as follows:
- $w \leftarrow w-\alpha * L_{w}$
- $b \leftarrow b - \alpha * L_b$

We notice that in the learning formula, we need to obtain the **gradient** of the loss function with respect to the weight and bias
- First, let's get the **expression** of the **loss function** **in terms** of $w$ and $b$
- ![[Pasted image 20260306105038.png]]
- Now that the dependency graph has been depicted, we next compute the **local partial derivatives** for each edge in the graph
- ![[Pasted image 20260306105221.png]]
- We write the results of each partial derivative on the corresponding edge:
- ![[Pasted image 20260306105242.png]]
- Now we want to find the partial derivative of $L$ with respect to $w$ ($L_w$)
	- We notice that there is just **one path** going from $L$ to $w$ in the dependency graph
	- Therefore, we **multiply** all the local partial derivatives on that path together
	- ![[Pasted image 20260306105346.png]]
	- ![[Pasted image 20260306105402.png]]
	- Similarly, we can do the same for $L_b$
	- ![[Pasted image 20260306105423.png]]
	- Next, we plug this into the gradient descent formula and get the following **learning rules**:
		- ![[Pasted image 20260306105452.png]]
