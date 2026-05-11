Our current (regression) **loss function** looks like this:

![[Pasted image 20251101092728.png]]

- a.k.a. the **objective function**
	- What is our **objective?** - Fit a **linear model** on $X\rightarrow \frac{1}{2}||Xw-y||^2$
	- **Square of errors**
		- **No negative errors** in summation
		- Large errors squared$^2$ **increase the loss** - **more emphasis** on *reducing* **large errors**
	- Can we use **other metrics?**
		- Yes - define **our own loss function** according to our needs
		- What *should* the model **learn**?

---
# Absolute error (L1 norm)
Example:
Say there are some outliers in $y$, as such, then the model gets **biased** heavily by the **large errors**, meaning it gets skewed by the **outlier** *much more* than it is influenced by the points **fitting the pattern**

![[Pasted image 20251101093135.png]]

One idea to reduce this effect could be to use **absolute values** for the errors rather than their squares

Loss function:
$$f(w)=\sum\limits_{i=1}^{n}|w^{T}x_{i}-y_{i}$$
- This loss function means that decreasing *small* and *large* errors is **equally** important
- Instead of minimising the square of the L2 norm, we minimise the L1 norm

However, minimising the **absolute error** is **harder**:
- The L1 norm function is **not differentiable** at 0
- The **gradient** is **constant** at all points other than 0, where it changes **abruptly**

![[Pasted image 20251101093512.png]]

There is a technique for making it **differentiable** around 0, called **Huber loss** - a **differentiable approximation** of the L1 norm: $h(\cdot)$

![[Pasted image 20251101093621.png]]

This makes the loss function **convex**
- And the function $h(\cdot)$ is **differentiable**

![[Pasted image 20251101093658.png]]

---

# Standardising features
With some ML model types, it may be useful to **convert values** to a **standard unit**
- *not* important for Naive Bayes or standard Least Squares
	- Naive Bayes only looks at one feature at a time
	- For Least Squares, $w_{j}(100)$ gives the same model as $w_{j}(0.1)$ but with a different $w_{j}$ (multiplied by x1000 for example)
- **Important** for $K-NN$ and **Regularised Least Squares**
	- For $KNN$, the value of **distance** is affected by **large features**
	- For RLS, Penalising $w_{j}^{2}$ **differs** if features $j$ are on **different scales**

*Usually*, we **standardise** **continuous features**:
- For each **feature** $j$, i.e. the $j$th **column** of $X$:
	- We compute the **mean** and **standard deviation**
	- ![[Pasted image 20251101094257.png]]
	- And then we **subtract the mean** and **divide by standard deviation**
	- ![[Pasted image 20251101094324.png]]
When **standardising the test data**, **do not** use mean and standard deviation of **test data** - *always use* mean and standard deviation of **training data**
- Our model is **trained** with standardised training data
- Training and test means and standard deviations may be **different**

In regression it is common practice the **standardise the targets** in **ground truth vector $y$**
- Move targets to the **same standard scale** as *standardised features*

![[Pasted image 20251101094642.png]]

If **targets are standardised**, forcing $w=0$ during optimisation **forces** the model to **predict the average** of $y$
- e.g. with high L2-regularisation, **prediction value** is *closer* to the **average value** of $y$
- Since the model will be forced to predict the average value as it is the **best prediction given very small weights**

---

# Feature selection and Regularisation
Now, rather than just predicting the label given some features, we would like to decide **which features** are the **most relevant** for predicting the label

From the point of view of least-squares fitted linear models:
- Given our training data $X$, we would like to find the features, i.e. **columns** of $X$ that are **most important** for predicting $y$

![[Pasted image 20251101095022.png]]

**Narrowing down** the number of features we look at can **help us** by reducing the **dimension** of the feature vector input to the model

Various methods for solving this:
## Pearson's correlation coefficient
We can compute the **correlation** between feature values in each $x_{j}$ and $y$

Uses the following equation:

![[Pasted image 20251101104648.png]]

This turns feature selection into **hypothesis testing** for each feature
- Essentially, for **each feature vector**, it computes the **correlation** to the **label** using the equation above. (a relation between the mean and standard deviation of both the feature and the label)

However, it has many downsides, so is not used widely today. This is because it usually gives **unsatisfactory results** as it ignores certain **feature interactions**:
- It might *include* **irrelevant features**:
	- If 'Fish and Chips' gives an allergy, and they are often eaten on Fridays, this approach may determine that Friday is relevant - while the relevant feature in this case is actually the food that is consumed
- It may *exclude* a **combination of relevant features**:
	- E.g. Diet Coke + Mentos may not give an allergy / reaction on their own, but **together** they do
## Regression-weight approach
**Fit** regression weights $w$ based on **all features**
- e.g. by using least squares
**Take** all features $j$ where weight $|w_j|$ is *greater* than a **threshold**

For example, here use the features that have $|w_j|>1$ (features 3, 4, 6)

![[Pasted image 20251101105321.png]]

This **solves** the previous issues with Pearson correlation coefficient

However, this method also has **issues**:
- Does not work well with **collinearity**
	- Example: If the 'Friday' feature **always equals** the 'fish and chips' feature (in the training set)
	- The solution may tell us that Fridays are relevant, but 'fish and chips' are not
	- ![[Pasted image 20251101105553.png]]
	- Which may not translate to the testing/real data
- Or, if there are two copies of an **irrelevant feature**, the model may give them both high proportions of weights which **cancel out**
	- ![[Pasted image 20251101105651.png]]
## Search and score approach
- Define a **score function** $f(s)$ that measures the **quality** of a **subset** of features $s$
- Search (*enumerate*) for the $s$ (*subset*) with the **best score**

If the score is only based on the training error, this may result in **overfitting** on the training data

A more appropriate score function would be based on the **validation error**.
- Find a subset of features that gives the lowest validation error
- Helps also to minimise test error by reducing overfitting

However, we **high dimensional** feature vectors:
- There are $2^{d}$ subsets of features
- We're prone to **optimisation bias** and **false positives** - irrelevant features may help by chance

To reduce false positives, we can introduce a **complexity penalty**
- For example, use the squared error **and** the **size of the subset**

![[Pasted image 20251101110201.png]]

This is essentially the equivalent of **adding the L0 norm** as a feature selector
- The L0 norm gives the **number of non-zero values** in a vector

Now, we have an loss equation which resembles what we had from L2-regularisation

![[Pasted image 20251101110328.png]]

Except we're using the L0 norm instead of the squared L2 norm

**However**, this approach has a **disadvantage** - It is hard to find the **minimiser** for this objective function, since it is a **non-convex** function

It's actually possible to use the L1 norm instead, which approximates this error for our needs.
- Similar to the L2 norm, the L1 norm is **convex** and **improves our test error**
- Similar to the L0 norm, it **encourages elements** of $w$ to be **exactly zero**
- A.k.a. LASSO regression

---

# Sparsity and regularisation
