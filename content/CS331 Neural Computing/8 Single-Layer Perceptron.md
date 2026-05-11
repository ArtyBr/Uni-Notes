The MP neuron is **limited** in the following ways:
- Inputs and outputs are limited to **binary outputs** only
- All **inputs** are treated as **equally important**. No chance to assign **more importance** to some points.
- A **manual setting** of **threshold $\theta$** is always required

The **perceptron** was Rosenblatt's solution to this

We introduce some **learning mechanisms** to the model.

**Inputs**:
- Perceptron:
	- Real numbers
- MP Neuron
	- Binary

**Weights**:
- Perceptron:
	- Each input carries a weight
- MP neuron:
	- All inputs **equally** important

**Threshold**:
- Perceptron:
	- Can be **learned** **automatically**
- MP Neuron:
	- **Manually** set by users

The Perceptron is a **single-layer** neuron with a threshold function

We use a **bias** alongside weights for each neuron.

![[Pasted image 20260128122745.png]]

Bias allows us to **translate** the classification barrier, in addition to the rotation introduced from the weights

Two-vector form of the perceptron:

![[Pasted image 20260128123139.png]]

---

Perceptron **learning rule**

The key steps:
- Initially, chose $w, b$ at random to create a plane $w\times x+b=0$
- **Detect** if there exist any **misclassified** nodes
- **Update** the plane by adjust $w, b$ (if misclassified)
- Output $w, b$ once no misclassifications

We can use the **dot product** of two vectors to find the angle between them.
- If positive, **acute**
- Otherwise, obtuse
- if 0, **perpendicular**

In order to detect if a point is within some region, we perform the following operation:

$w\cdot x+b$, where $w$ is a normal **vector** which is **perpendicular** to the plane.

![[Pasted image 20260128124406.png]]

![[Pasted image 20260128124536.png]]

How to update $w$ to learn $w\cdot x=0$?

![[Pasted image 20260128125117.png]]

So, we get this unified **learning rule** for the perceptron:

![[Pasted image 20260128125146.png]]

We also use a **learning rate**:

![[Pasted image 20260128125209.png]]

