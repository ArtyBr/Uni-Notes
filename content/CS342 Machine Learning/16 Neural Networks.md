Neural networks use **layers**, which separates them from **linear models** (no layers)

![[Pasted image 20260420142921.png]]

- Each layer introduces a **bias** to the corresponding nodes
- We added a bias (intercept) in **linear regression** by adding a **column of ones** to the matrix:

![[Pasted image 20260420142637.png]]

- In NN's, the bias is usually **explicitly used** in the **output**
	- A bias in **each feature** could also be added

![[Pasted image 20260420142721.png]]

- We see that we can add a '1' to each layer of the NN

Let's use a 1-layer NN for a regression task
- If we use the **mean** of the **squared errors**, our objective function is:
$$f(W,v)=\frac{1}{n}\sum\limits_{i=1}^{n}(v^{T}h(Wx_{i})-y_{i})^2$$
**Stochastic gradient descent** (GSD) is commonly used for training
- The **gradient** of a **random sample** $i$ is computed to **update** both $W$ and $v$

Tuning the algorithm is difficult because the objective **function** is **not convex**
- Due to **non-linear activation**

Computing gradients in NNs with SGD is called **backpropagation**

---

**More than one** random example can be used to refine a solution $w^t$
$$w^{t+1}=w^{t}-a^{t}\frac{1}{|B^{t}|}\sum\limits_{i\in B^t}df_{i}(w^{t})$$
Where $B^{t}$ is the **random batch** of examples at iteration $t$ (aka **mini-batch**)

The mini-batch **size** has an effect on the **ball** representing the **challenging region**
- *Doubling* mini-batch size $\approx$ *halving* the radius of the ball
- Large **mini-batch size** + large **learning rate** $\rightarrow$ Getting to the ball **faster**

If we use mini-batches, we have the following two processes:
- **Epoch** - a **full pass** through the training set
- **Iteration** - No. of **times** we need to run SGD to **complete an epoch**
	- Examples: 2,000 training examples divided into **random mini-batches** of 500 $\rightarrow$ 4 iterations to complete 1 epoch

(Question - one iteration of SGD contains many iterations/steps of gradient-finding/tuning, right?)

---

It's common to use **more than one epoch**
- NNs have **highly non-convex functions**; it is always recommended to update the parameters over several epochs
- (very wavy - many local minima / plateaus)

Why is this?
- Neural networks are **universal function approximators**
	- The functions they *approximate* are:
		1. Functions to map **feature vectors** to a **new feature representation**
		2. Functions to map **new feature representations** to the **desired output**
	- With enough **neurons** and **layers**, they can **learn to approximate** *any function* very well

This means that they need to be able to approximate **non-convex** functions as well as convex ones
- Convex functions **can't approximate** non-convex ones very well
- We then need **non-convexity** itself within our NN to approximate both convex and non-convex ones

---

Computing gradients in NNs with SGD involves **backpropagation**
- Based on the **chain rule**

This computes the gradient function with respect to all parameters
- Computes the gradient at **each neuron** with respect to the overall output (overall objective function)
- Take a small step in the negative gradient direction to update all of the weights

In Deep Neural Networks (DNNs), the **learning** **rate** is an important factor
- SGD is very sensitive to the step size

Common practice is to **manually-tune** the step size
- Fix step-size and run training
- Monitor the loss function (error) to see if it decreases
- If error does not decrease, decrease step size
- Keep monitoring error

![[Pasted image 20260420144912.png]]

SGD can be **further improved** by **adding momentum**
- Solution is usually found **relatively slowly** with a **small learning rate**
- Momentum is the equivalent of adding a 'heavy ball' to the search
	- Adding 'inertia' acts as a smother and accelerator: ignoring local minima and 'humps'
	- Add a **term** that moves in the **previous direction**
$$w^{t+1}=w^{t}-\alpha^{t}df_{i}(w^{t})+\beta^{t}(w^{t}-w^{t-1})$$ with $\beta \in [0,1]$

![[Pasted image 20260420145142.png]]

We get a **vanishing gradient** with small gradients - they keep decreasing and decreasing (sigmoid function applied to small numbers, e.g. - activation function) exponentially and eventually get very close to 0
- We can replace sigmoid function with something like ReLU to mitigate this

---

There is a **fundamental trade-off** in **deep learning**

If we recall:
- $E_{train}$ is how small can we make the training error
- $E_{approx}$ is how well the training error approximates the test error

NNs are also subject to this fundamental trade-off:
- As **depth increases**, training error **decreases**
- As **depth increases**, Training error **no longer approximates test error**

Deeper NNs can approximate complex functions
- However such high depth also leads to **overfitting**

**Regularisation** is used in DNNs to avoid overfitting

![[Pasted image 20260420150427.png]]

We see that we can add the **sizes** of the weights themselves as parameters to the objective function in order to motivate simpler models