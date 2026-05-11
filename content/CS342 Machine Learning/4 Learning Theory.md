Example:
- Data from **248 homes**
- For each data point, have x features:
	- Elevation
	- Year
	- Bathrooms
	- Bedrooms
	- Price
	- Square Feet
- Given these features, want to classify homes as being in **San Fransisco** (SF) or **New York** (NY)

Using feature elevation:
- ![[Pasted image 20251014134646.png]]
 Now using Price/Square feet:
- ![[Pasted image 20251014134708.png]]
Want to balance the **depth** of the tree vs **accuracy**

![[Pasted image 20251014134826.png]]

Eventually, you will have enough branches / sub-trees to classify the data with 100% accuracy

However:
- Our decision tree gets a **training accuracy** of 100%
- Assuming we have some data for 200 more homes, this model may not be as accurate for that data
- This effect is **overfitting**
	- Lower accuracy on **new data**, high accuracy on **training data**
- It is also possible to do **underfitting**
	- Rules don't perform accurately on either the new data **or** the training data

**Notation**:
For **training** data (we know the labels)
- We describe this as:
- $X$ as the *inputs* and $y$ as the *outputs*
For **testing data** (we want to classify this)
- $\tilde{X}$ for the *inputs* and $\tilde{y}$ as the **generated/predicted** *outputs*

Typical supervised learning steps:
1. Build a model based on training data $X$ and $y$ (**training phase**)
2. Use the model to make **predictions** $\tilde{y}$ on test data $\tilde{X}$ (**testing phase**)

Apart from considering the **training error**, we should also consider the **test error**:
- Are predictions $\tilde{y}$ similar to the **true** unseen labels $\tilde{y}$
- These unseen labels represent the **ground truth**

We are *mainly* concerned with the **test error**
and importantly:
- The **test data** *cannot influence* the **training phase** in any way

In order to learn, we need **assumptions**
- The training and test data need to be *related* in some way
- The most **common** assumption: **independent and identically distributed** (IID) data

### IID assumption
Trained/test data are **independent** and **identically distributed** if:
- All example come from the **same distribution**
	- (same data source)
- The examples are sampled **independently** from the source (sampling order does not matter)
	- Need to make sure to sample and put the samples **back** in the drawing pile 

This assumption makes *learning* possible as:
- patterns in **training** examples are likely to be *the same* in **test** sample

It's rarely true, but it's a good approximation

### Learning Theory
**Learning theory** explores how **training error** is *related* to **test error**
- $E_{train}$ = error on training data
- $E_{test}$ = error on testing data 
$$E_{test}= (E_{test}-E_{train})+E_{train}$$
$$E_{test}=E_{approx}+E_{train}$$
- $E_{approx}$ captures **how well** the training dataset *represents* the test dataset
	- $E_{approx}=\dfrac{|\text{Model Complexity}|}{\sqrt{n}}$
	- Where $n$ is the number of samples/amount of data
- If $E_{approx}$ is small, then $E_{train}$ is a good **approximation** of $E_{test}$
	- If $E_{approx}<\epsilon$ , there is **NO** model for which  $E_{train}, E_{test} \geq \epsilon$ 
- $E_{approx}$ depends on:
	- Tends to get *smaller* as $n$ gets *larger* $\rightarrow$ More training examples, large training dataset
	- Tends to *grow* as the model gets *more complicated*

### Fundamental trade-off
This strategy has a **fundamental trade-off**
- $E_{train}$ vs $E_approx$
- $E_{Train}$: How *small* can we make the **training error**
- $E_{approx}$: How well the **training error** *approximates* the **test error**

Examples:
- **Simple** models (like decision stumps)
	- $E_{approx}$ can be *low* (not very sensitive to training dataset)
	- $E_{train}$ may be *high*
- **Complex models** (like deep decision trees)
	- $E_{approx}$ may be *high* (very sensitive training dataset)
	- $E_{train}$ can be *low*
**Training error** vs **test error** for choosing depth:
- **Training error** is usually *high* for **low depth** - **underfitting**
	- Training error *reduces* as the depth *increases* 
	- e.g. as a decision tree gets deeper
- **Test error** *decreases* as **depth** *increases*, 
	- Test error *eventually* *increases* with high depth - **overfitting**
	- i.e. $E_{train}$ becomes a poor approximation of $E_{test}$

![[Pasted image 20251014175203.png]]
### Validation error
The main concern is the test error
- However, we cannot use the test data during training

One solution use *part* of the training data to *approximate* the test error
- Split training samples into a **training set** and a **validation set**
	- **Train** model based on training data
	- **Validate** model based on the validation data to understand how well the model will do with the test data

![[Pasted image 20251014164246.png]]

With IID data: validation error provides an **unbiased approximation** of test error

$$\mathbb{E}[E_{validation}] \approx \mathbb{E}[E_{test}]$$
- $\mathbb{E}[x]$ represents the **expectation** over IID samples $x$

Validation error is commonly used to select the **hyper-parameters** of the model
### Parameters and Hyper-parameters
**Parameters** control how *well* we fit a dataset
- We train a model by trying to *find* the *best* parameters on the training data
- By training our model, we *learn* the **value** of these parameters
- e.g. rules in a decision tree

**Hyper-parameters** control how *complex* our model is
- We *cannot learn* a hyper-parameter
- We usually *select* a hyper-parameter using a **validation error**
- e.g. depth of a decision tree

To try to choose a good value for depth, we could:
- Try a depth-$n$ decision tree for $n$ possible depths (we choose)
- Return the depth with **lowest validation error**

### Optimisation bias
Another name for **overfitting**
- How **biased** is an error that we *optimised* over **many possibilities**

Optimisation bias of **parameter learning**:
- During learning, we could search over several different decision trees
- We find a tree with a low training error - **overfitting**
	- We have over-used the **training data** - **rules** are *too specific* for this data

Optimisation bias of **hyper-parameter tuning**
- Optimise the validation error over 1000 values of depth
- We find a tree with very low validation error - **overfitting**
- We have overused the **validation data** - **depth** is *too specific* for this data

**Validation error** tends to have a **lower** optimisation bias (i.e. less overfitting) than training
- For example, optimising over 20 values of depth vs trying thousands of possible trees

Optimisation bias *decreases* as the number of **validation samples** *increases*
- Solutions:
	- Increase validation set
	- Not overuse validation set

### Cross validation
Validation *assumes* that data is **plentiful**
- Do we have *enough* training data to create **both** a training *and* validation set?
	- Data may be **scarce** - we do not want to **waste** data on validation
	- We may not have the *ability* to **sample** a fresh validation set

$k$-fold cross validation gives an accurate estimate of the test error *without wasting* too much data.
- 5-fold cross-validation:
	- **Train** on 80% of the data, **validate** on the other 20%
	- *Repeat* this $k=5$ times with different splits, and *average* the accuracy (or other metric used)

![[Pasted image 20251020180917.png]]

- 10-fold would be to train on 90% of data and validate on 10%, repeating 10 times in total

**Leave-one-out** (LOO) cross-validation - train on **all but one** training sample
- Repeat $n$ times and average
- Corresponds to special case when $k=n$

$E_{test}$  gets **more accurately** represented with more folds, but **more expensive** to compute

![[Pasted image 20251020181059.png]]

Central challenge of ML is that our learning algorithm, or model, must perform well on **new, unseen data**

The ability to perform well on unseen data is called **generalization** - alternative measure of performance
- How well does our model perform with **unseen feature vectors?**

