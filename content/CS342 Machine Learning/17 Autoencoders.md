How can we learn the **meaningful representation** of the huge amounts of data that we have?
- **Representation learning**

We would like to **automatically identify** the **underlying structure** or **representation** of the data
- Similar to PCA
	- Found the reduced **linear components** that reproduce the data to a high degree (with less dimensions)

In Neural Networks, the **number of neurons** in each layer tells you how many dimensions we store at that layer
- So in essence, we are already reducing the number of dimensions representing the data in Neural Networks

When we have a neural network in which each layer **decreases** the number of neurons per layer, we can call this an **encoder**
- It **encodes** the data of many features into a **more compact representation** through the process of learning the **weights and biases** that best represent the data

The **reverse process**, going back and **increasing** the number of neurons, is the **decoder**
- It takes the **encoded data** and returns it back to the **original** **feature space**

![[Pasted image 20260420152445.png]]

So our goal is to **find $f:X\rightarrow Z$**, which is **easy** if $Z$ (the representation) is known, but often $Z$ is **not known**

If $Z$ **is known**:
$$Z=f(X)$$
$$X=f^{-1}(Z)=f^{-1}(f(x))$$
$$X\rightarrow f \rightarrow f^{-1} \rightarrow X$$
If you can somehow **learn** this $f$ and $f^{-1}$ pair, you can plug it into the pipeline and it will give you $Z$
- However, you need to **learn** $f$ and $f^{-1}$

![[Pasted image 20260420153300.png]]

So that's what a NN is doing:
- $f$ is the **encoder** part
- $f^{-1}$ is the **decoder part**

And a NN **learns both** at the **same time**
- So we **don't need to know $Z$ itself**
- We can learn the functions themselves (by training an NN)

This gives way for two applications, based on your starting point:
1. Start with a **large piece of data**, **encode** it to give a more compact representation, get a **compressed representation** of the data
2. Start with a **representation** of the data (or random noise) and put into the **decoder** to generate **new uncompressed data**

![[Pasted image 20260420153733.png]]

---

For binary outputs, the **squared error** is commonly used as the loss function
$$L(x_{i},\hat{x}_i)=||x_{i}-\hat{x}_i||^{2}$$
However for **multi-class** classification, we often use **cross-entropy loss**

![[Pasted image 20260420154845.png]]

- Take the log of the estimate, multiply it by the original data, and combine this by multiplying it with the **opposite case** as well (1-data)

![[Pasted image 20260420155023.png]]

---

**Denoising Autoencoders** (DAEs)

Sometimes our data is kinda bad - has lots of noise in it (mistakes, errors occasionally etc.)

A DAE works by **corrupting the input** by using a probabilistic process **before feeding** it to the **network**
$$p(\hat{x}_{ij}=0|x_{ij})=q$$
$$p(\hat{x}_{ij}|x_{ij})=1-q$$
- The $j$th dimension is **set** to 0 with **probability** $q$

So we train it by **not taking into account** certain features with some probability, meaning it should be **robust** to this random noise occurring in data features
- The objective is still to **reconstruct** the original (un-corrupted) $x_{i}$ as **accurately as possible**

An important aspect is that the loss should be **compared** to the **original data**, not the 'artificially noisy' data that you created.
- Otherwise this system would be no different to the traditional autoencoder

Visual representation:

![[Pasted image 20260420160245.png]]

The bottom layer (turning $x_{i}$ into $x_i$ with some features removed) is the **only difference** between a DAE and AE

Rather than using this 'flipping coin' noise (either include or exclude the feature), you can use **Gaussian noise** instead
$$\hat{x}_{ij}=x_{ij}+N(0,1)$$
Where $N(\mu, \sigma^{2})$ denotes a Normal (Gaussian) distribution with mean $\mu$ and variance $\sigma^{2}$
- In other words, each $x_i$ is **modified** by adding noisy values that follow a normal distribution

