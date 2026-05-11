Each node in the neural network graph denotes a **neuron** (unit cell)
- The edges between them denote **signal transmission**
- The **strength** of the signals is captured by their **weights**
- Neurons are connected via a **network**
- These are connected through **layers**

When we count the layers, we **do not** count the **input layer**

![[Pasted image 20260126161718.png]]

An NN **must** have an input and output layer, but can have 0 or many hidden layers.

---

**Scale of feedforward NN**

If we have no hidden layers, it's called a *single-layer* NN
- no layers is 1 or 2 - *shallow* NN
- no hidden layers > 3 - *deep* NN

![[Pasted image 20260126161929.png]]

A **feed-forward** NN is an ANN where information only moves in **one direction** - **forward**
- There are **no cycles** or **loops**

The layers may identify different features of the input.
- **Training** a NN **teaches** it to identify the best features of the input.

![[Pasted image 20260126162100.png]]

Next, we can use a **Convolutional** NN (CNN).
- This applies NNs to images.
- They perform **convolutions** on the images to produce feature maps, informing the network of the structure of the image.

- Next, we perform **dimensionality reduction** with **subsampling**

This is then passed to a fully-connected NN

![[Pasted image 20260126162234.png]]

Finally, a **recurrent** NN is a type of **deep** NN.
- Deals with **temporal sequence data**.

Uses a **self-loop**, which, when expanded, means each node from behind is connected to those in front.

This is a **feed-back** NN, since it has **loops** and can **go backwards**

![[Pasted image 20260126162506.png]]

Overall, different types of NNs can be summarised as such:

![[Pasted image 20260126162528.png]]

