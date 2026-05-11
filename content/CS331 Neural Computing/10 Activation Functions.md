![[Pasted image 20260427183458.png]]

An **activation function** (or *transfer* function) is a mathematical function attached to each neuron, which **decides** if a neuron should be activated/fired
- It often **adds non-linearity** to the neural networks, and **normalises** the **output** of each neuron

![[Pasted image 20260202164828.png]]

The **linear** activation is simply $L(x)=c\cdot x$ - multiply $x$ by some value $c$.

![[Pasted image 20260202165238.png]]

- **Adv:** Better than a step function since it outputs multiple values (proportional to inputs), not just two values
- **Disadv**:
	- There is a constant gradient descent occurring as the derivative of the linear function is a constant $c$, showing no relation to the input $x$
	- When multiple layers use a linear activation function, the **entire network** will be equivalent to a **single-layer** model.
		- This is because **linear combinations** of **linear functions** are **still** **linear functions**

The **sigmoid** function has a **curved, non-linear** shape.

![[Pasted image 20260202165259.png]]

- **Adv**:
	- S-Shaped sigmoid function has a **smooth** gradient, unlike the step function (which jumps in output values)
	- **Output** is **normalised** between 0 and 1, unlike unbounded output of linear functions
- **Disadv**:
	- **Vanishing gradient**: For large or small inputs, there is almost no change to the prediction. This can result in the network being *very slow* to learn further
	- Sigmoid outputs are **not zero-centred**
	- **Computationally expensive**, due to presence of the exponential function

The **gradient** can be calculated:

![[Pasted image 20260202165504.png]]

![[Pasted image 20260202165510.png]]

![[Pasted image 20260209151214.png]]

---

An alternative function is the **tanh** function:

![[Pasted image 20260209151236.png]]

![[Pasted image 20260209151243.png]]

![[Pasted image 20260209151253.png]]

---

**ReLU** (Rectified Linear Unit)

**ReLU** is simply defined as the maximum between 0 and the input
- $ReLU(x)=max(0,x)$

![[Pasted image 20260209151719.png]]

- ReLU is a **piecewise** **linear** function, but **globally non-linear**. 
- It has a **derivative function** allowing for **backpropogation**
	- ![[Pasted image 20260209151838.png]]

**Adv**:
- Simple and easy to compute, thus **converges** very quickly
- For **positive inputs**, there is **no vanishing gradient** as its derivative becomes 1

**Disadv**:
- 'Dying ReLU' refers to the problem where ReLU neurons become **inavtive** and only output 0 for any input. This occurs when ReLU gets **negative input**
- Also, it is **not** a **zero-centered function**

ReLU is **not differentiable at $x=0$** as it is not smooth at this point.
- The left-hand slope at $x=0$ is 0 whereas the right-hand slope is 1.
- Typically, ReLU'(0) is defined to be *either* 0, 1, or 0.5

We introduce two **variations** of ReLU.

**Leaky ReLU**:
- Aims to address the 'Dying ReLU' problem

$LReLU(x)=max(ax,x)$
- Multiplies the negative values by a small constant to keep the gradient from being 0

![[Pasted image 20260209152300.png]]

- $\alpha$ is predifined by the user. Often set between 0.01 and 0.3
- Can also **learn** $\alpha$ during training
	- In this case, we call it **parametric ReLU** or PReLU

- Solves the 'Dying ReLU problem'
- No Vanishing gradient
- However, it is still not differentiable at 0

**Exponential Linear Unit (ELU)**
- Aims to alleviate the differentiability problem at $x=0$

ELU uses a **non-linear exponential curve** when input is negative.

![[Pasted image 20260209152531.png]]

- **Smoother** at $x=0$ than LReLU
- ![[Pasted image 20260209152622.png]]

No 'Dying ReLU' problem, and no vanishing gradient problem.
- Also, converges quicker than other ReLU

**However**, it is **more computationally expensive** than the others

---

**Softmax**

Takes as input a **vector $z$** of $N$ real values, and **normalises** it into a **probability distribution** consisting of $N$ probabilities **proportional** to the exponentials of the input values

![[Pasted image 20260209152825.png]]

- The **sum** of all $y_i$ is equal to 1

 Softmax is the **generalisation** of the **Sigmoid** function to $N$ classses
 - Whereas Sigmoid applies to just **2** classes.
 - Therefore, when $N=2$, Softmax **reduces to Sigmoid**

---

**Maxout**

A **piecewise** linear function that returnst he **maximal** value of the input data

![[Pasted image 20260209153602.png]]

- $k$ is the number of linear pieces (affine feature maps) given by the user

![[Pasted image 20260209153737.png]]

Example:

![[Pasted image 20260209153942.png]]

Maxout has special cases that **degrade** to the other activation functions:

![[Pasted image 20260209154220.png]]

Therefore ReLU, LReLU and Abs are **special cases** of Maxout.
- Therefore, gets all the **benefits** of ReLU and LReLU

When we **increase** the number of **linear pieces** $k$ **Maxout** can **approximate** non-linear functions to a great accuracy

![[Pasted image 20260209154359.png]]

When $k$ is larger, despite its high accuracy, the function becomes **computationally expensive** as the number of parameters for each neuron increases.

A Maxount unit can learn a piecewise linear function with up to $k$ pieces, which is a **convex function**
- Thus, two maxout units can **learn any continuous function**, since they can be **expressed by the difference of two convex functions**

![[Pasted image 20260209154555.png]]

