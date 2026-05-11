Naive Bayes classifiers and Decision Trees are **parametric models**

**Parametric** models:
- **Fixed** number of **parameters**
- Once trained, the **model size** is $O(1)$ in terms of $n$ examples in the dataset
	- Decision tree - stores **rules**, no matter $n$
	- Naive Bayes classifier stores **counts**, no matter $n$
- Parameters are estimates **more accurately** with *larger* $n$

**Non-parametric** models:
- Number of **parameters** *grows* with $n$
- **Size** of model *depends* on $n$ - model grows **more complicated** as we get **more data**
- Examples:
	- $K-NN$ ($K-$Nearest Neighbours)

# K-NN: K-Nearest Neighbours

K-NN attempts to classify a samples $\tilde{x_{i}}$
1. Find the $K$ training examples {$x_{i}$} that are **nearest** to $\tilde{x_{i}}$
2. Classify $\tilde{x_{i}}$ using the **most common** label of the $K$ **nearest examples**

Based on the **Lipschitzness assumption**: If two feature vectors are **close to each other**, then their labels are **likely** to be **the same**

![[Pasted image 20251022091233.png]]

The most common **similarity metric** is **Euclidean distance**
$$||x_{i}-\tilde{x_{i}}||$$
$$=\sqrt{\Sigma_{j=1}^{d}(x_{ij}-\tilde{x_{ij}})^{2}}$$
- With **small** $n$, $K$-NN is very simple
- Model gets **more complicated** as $n$ **increases**

There is **no training phase** in $K$-NN - **lazy learning**
- Simply **store** the training data
- **Non-parametric** - the size of the model is $O(nd)$, the size of $X$
	- $n$ = number of samples
	- $d$ = number of features per sample
- Predictions are **costly** 
	- $O(nd)$ to classify 1 test sample
	- $O(ndt)$ to classify $t$ test samples

Euclidean distance is the **L2 norm** of the difference between two feature vectors
- Norms measure the **length** of vectors

There also exist other norms:

![[Pasted image 20251022093147.png]]

These are examples are **special cases** of the more general, $L_{p}$ norm, $p\geq 1$
$$||x_{p}||=(\Sigma_{j=1}^{d}x_{j}^{p})^\frac{1}{p}$$
The $L\infty$-norm is the **limit** as $p$ goes to $\infty$ 

Computing the **difference** between two vectors $r$ and $s$ is equivalent to computing the **distance** between the **locations** pointed to by the vectors

![[Pasted image 20251022093756.png]]

For the 1-NN case,
- The **predicted label** of any $\tilde{x_{i}}$ is the label of the data point **closest to it**
- Each **data point** defines a **cell** - **Voronoi Tessellation** of the feature space

![[Pasted image 20251022093859.png]]

So, if you now have a **new point**, it will **lie in one of the cells** and you know which point is its nearest neighbour in $O(1)$ time.

---

We introduce the concept of an **Optimal Bayes classifier** as a **reference** model for a binary classification task (two possible classes)
- The optimal classifier is **similar** to the naive classifier, however it **does not** use the **conditional independence** assumption
- It **knows** the **exact probability** from the training data
- Theoretical concept - not practical

The optimal Bayes classifier will therefore predict $\hat{y_{i}}=argmax[p(y_{i}|x_{i})]$
However, the classifier can **still make mistakes** because it is based on **probabilities**. Probability **error** is:
$$\epsilon_{Bayes}=1-p(\hat{y_{i}}|x_{i})$$
We can **use** this Optimal Bayes Classifier to **estimate** a **lower bound** on the error of other classifiers

---

With the 1-NN classifier, we can see that as we **increase the size** of the **training set**, the **test error** is expected to **go down**
- The more samples we have, the distance between $\tilde{x_{i}}$ and its **closest nearest neighbour** *tends to* 0, the classifier returns the label of the nearest neighbour

![[Pasted image 20251022095250.png]]

As $n$ gets **larger**, the **error** of the 1-NN classifier *tends to* get **closer** to the error of the Optimal Bayes Classifier

![[Pasted image 20251022095404.png]]

However, as $d$ increases - the **dimensionality** becomes a problem

The number of points we need for the **error** to be **close to** the Optimal Bayes Classifier becomes **exponentially large** as the number of dimensions $d$ increases:
- We need exponentially more **points** to fill a **high-dimensional region** for the $1-$NN to achieve the best possible error - $n\approx c^{d}$ where $c$ is a constant

![[Pasted image 20251022095558.png]]

We can also define an **upper bound** on $E_{test}$.
- This is a classifier we should **always beat**

This is done using the **constant classifier** - always predicts **the same class** *independent* of any feature vector

This is important for debugging
- we shold always be able ot show that **our classifier** performs **better** on the test set than the constant classifier