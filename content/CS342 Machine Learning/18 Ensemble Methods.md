Use lots of **weak classifiers** (accuracy > 50%) to learn the classifications and combine them together to get a **strong classifier overall**
- The data used to train each classifier must be **different** (independent and identically distributed) from each other in order for this to work

Set of weak models whose **individual decisions** are **combined** in some way to **classify** new example or predict targets for new examples
- aka *meta-learning*
- Models that have **other models** as **inputs**

*Simplest* approach (e.g. for classification)
- Generated **multiple classifiers**
- Each classifier classifies the **same test examples**
- Take **majority** as classification results

It still needs to be *relatively accurate*:
- **Better** than just guessing
- > 50% accuracy

They need to be **similar** in their relative accuracy
- Otherwise, it's worth it to just use the most accurate model

They need to be **diverse**
- They should get **different errors** on the **same test examples**
- **Independent errors**

Independent errors: probability that $k$ out of $N$ classifiers (with independent error rate $\epsilon$) are wrong:
$$p(\text{no. wrong classifiers}=k)=\binom{N}{k}\epsilon^k(1-\epsilon)^{N-k}$$
The **probability** that the **majority is wrong** is the **cumulative error** (product) for at least $N/2$ wrong classifiers based on the distribution
- That probability, if you add it up, becomes **very small**
- So **very high chance** that it works well
- Say we have classifiers with error rate $\epsilon=0.3$
- With $k$ models, the chance of all of them being wrong = $0.026$

---

There are **different types** of ensemble methods:

### Averaging
Inputs to the **meta-learner** are the predictions of a set of weak models:
	- Low-depth decision trees
	- Naive bayes
	- K-NN
- Meta-learner **averages results**
	- Average probabilities or predicted numerical values
	- Take the **mode** (majority vote) of predictions if models output classes
- Example: 
	- ![[Pasted image 20260420171529.png]]
- Consider weak classifiers that **overfit** (e.g. decision trees that overfit):
	- If all overfit in **exactly the same way**, averaging does nothing $\rightarrow$ **not diverse**
	- If they make **independent errors**, probability of error of the average can be **lower** than that of the individual classifiers
		- The ensemble model pays **less attention** to the **specific overfitting** of each weak classifier

### Stacking

Use **another classifier** that uses the **results computed** by a set of weak classifiers as **features** to compute new results

![[Pasted image 20260420171832.png]]

### Bagging
**B**ootrap **agg**regation

In practice, **access** to the **original data** source may *not* be *possible*
- **Expensive** to independently collect several training dataset

1. Take a **single dataset $X$** with $n$ examples
2. Generate $M$ **new datasets**, each by **sampling** $n$ training examples from $X$, with replacement
3. **Average** the **predictions** of models trained on each of these datasets

- A famous example of this is **random forests**
	- Bagged decision trees with one extra trick to **further decorrelate predictions**
	- When defining a node, **randomly sample** a small number of the $d$ **features** (usually $\sqrt{d}$), and only consider this random set of features to find the **best splitting rule**

![[Pasted image 20260420172659.png]]

- Trees may **still overfit**, but **test errors** are expected to be **independent**
- **Average** of predictions tends to result in a **much lower test error**

Probably the **best** **black-box** ML algorithm $\rightarrow$ works very well with **no tuning whatsoever**

### ADAboost
This model focuses on performing the ensemble process **adaptively**

Assumes a **weak linear classifier** that can get the correct prediction with probability $(0,5+\epsilon)$, where $\epsilon$ is a **very small constant**

Can we *apply* this weak classifier **several times** to get a strong classifier that can get a very small training error rate?
- Yes - by **boosting** samples

ADAboost (Adaptive Boost):
- Works by **manipulating** the **training set** and an **initial weak linear classifier** to sequentially create **several stronger classifiers**
	- Each classifier is trained based on the performance of previously trained classifiers: **Focus on hard-to-classify examples**
	- Final classifier: **Weighted sum** of the created classifier
	- The linear classifiers are *based* on an **exponential loss**

**Exponential loss**:
$$L(\hat{y}_i, y_i)=exp(-y_{i}\hat{y}_i)$$
![[Pasted image 20260421100035.png]]

1. Train the **base linear classifier** on the training data with **equal weights** for all samples
2. **Re-weight** the training data to **emphasise the hard classes** and train a **second classifier**
3. Keep training **new classifiers** on re-weighted data
4. Use a **weighted combination** of all the trained classifiers as the **final model**
#### How do we re-weight the data?
Input: $X$
Ground truth: $y\in\{-1,+1\}$
- Define a **weight** for example $i$ in weak classifier $m:w_{i}^{(m)}$
	- Initial weight for each example $i$ is the same for all examples, normalised: $w_{i}=\frac{1}{N}$
- Define a **cost function** for the $m^{th}$ weak classifier
$$J_{m}=\sum\limits_{i=1}^{n}w_{i}^{(m)}[\hat{y}_{i}^{(m)}\neq y_{i}]=\sum \text{weighted errors}$$
Where $[\hat{y}_{i}^{(m)}\neq y_i]$ is:
![[Pasted image 20260421102602.png]]

Fit a classifier with weighted samples
- i.e. find the one that has the **smallest cost**

So to **re-weight** the data:
- Compute the **weighted error rate** of the $m^{th}$ weak classifier:
$$\epsilon_{m}=\dfrac{J_{m}}{\sum\limits_{i=1}^{n}w_{i}^{(m)}}$$
- Compute the **quality** of the $m^{th}$ weak classifier:
$$\alpha_{m}=log(\frac{{1-\epsilon_m}}{\epsilon_{m}})$$
	- $\alpha_m=0$ if classifier has $\epsilon_{m}=0.5$, and $\alpha_m=\infty$ if classifier is perfect
- New weight for sample $i$ in **next classifier** $m+1$:
$$w_{i}^{(m+1)}=w_{i}^{(m)}exp(\frac{-1}{2}y_{i}\alpha_{m}\hat{y}_{i}^{(m)})$$
![[Pasted image 20260421103218.png]]
The weight gets **exponentially larger** if the classifier is **wrong**
- **Smaller** if it is **right**
#### How do we weight and combine the several weak classifiers?
Given test data, weight predictions of the classifiers, each is weighted by its quality:
$$\frac{1}{2}\alpha_{m}\hat{y}^{(m)}$$
Final prediction using a weighted combination of $M$ classifiers:
$$\hat{y}^{(m)}=sign(\sum\limits_{m=1}^{M} \frac{1}{2}\alpha_{m}\hat{y}^{(m)})$$
Recall: predictions $\hat{y}\in \{-1,+1\}$

We can **bias** the classifier to either class by adjusting $\beta$
- Changing $\beta$ changes the **ratio** of **true positive detections** to **false detections**

![[Pasted image 20260421154443.png]]

We can wee on the graph that currently there aren't any false positives/negatives so it's not important
- However if we had points that were kind of in each region, then as we move the classifier boundary up or down, we can **affect** the number of false positives we get, or the number of false negatives if we go the other way around.
- This can be useful depending on the problem!

![[Pasted image 20260421152229.png]]

The **Receiver Operating Characteristic (ROC)** curve graphically represents how **changing** $\beta$ **changes** the ratio of TP to FP - useful to understand the **robustness** of the classifier

![[Pasted image 20260421155331.png]]

- The more the curve approached the **upper left corner**, the **better** the **classifier**

Adding a **new boosted linear classifier** to an ensemble model **always improves** the ensemble's **ROC curve**
- We can **continue adding classifiers** until we obtain a target rate of **false positives** and **true positives**

![[Pasted image 20260421155845.png]]

### XGBoost
**eXtreme Gradient Boosting**

Boosting-based method that allows working with **huge datasets**
- Uses **regularised regression trees** as the base model

With **regression trees**, each *split* is based on 1 feature; each leaf node gives a **real-valued prediction** *instead* of a **class**

![[Pasted image 20260421161422.png]]

They work the same as decision trees do *until* you get to the **leaves**
- They give real-valued predictions (probabilities) rather than binary decisions

At each leaf node, predict the **mean** of the training $\{y_i\}$ assigned to that leaf
- Let $w_{i}$ denote the **mean** at leaf $l$ with a total of $L$ leaf nodes
- Let $w_{l(i)}$ denote the leaf node to be used as the prediction $\hat{y}_i$ for example $i$

Training *finds* the **tree structure** and the $\{w_l\}$ values that **minimise the square error**
$$f(w_{1}, w_{2}, w_{3}, \dots, w_{L})=\frac{1}{n}\sum\limits_{i=1}^{n}(w_{l(i)}-y_{i})^{2}$$
**Similar complexity** as fitting decision trees for classification
- **Mean** *instead* of the **mode**
- **Squared error** *instead* of **accuracy/information gain**
- A greedy strategy can also be used to grow the tree by using decision stumps

**Greedy recursive splitting**
- Start with some feature
	- Optimise the **split** of that feature
	- So that when you split it into two parts, the **loss function** is **minimised** for that feature
- Then you move on to each branch and you do this recursively for each feature
- When to stop?
	- When the **loss is 0**
	- **or**
	- When splitting keeps the **loss** at the **same amount** as it was **before**

So XGBoost uses an **ensemble** of $M$ **regression trees**
- For example $i$, *each tree* provides a **real-valued prediction**

![[Pasted image 20260421162734.png]]

The **final prediction** is the **sum** of all $M$ predictions

$$\hat{y}_{i}=\hat{y_i}^{(1)}+\hat{y_i}^{(2)}+\hat{y_i}^{(3)}+\dots+\hat{y_i}^{(L)}$$

![[Pasted image 20260421162917.png]]

Note: XGBoost does *not* use the **mean** of the predictions like random forests, it does *not* use a **weighted sum** of predictions like ADAboost either
- Each tree **does not attempt** to **individually predict $y_i$**
- Each new tree tries to '*fix*' the prediction made by **preciously fitted trees**, that all predictions **add up to $y_i$**

When deciding on what type of tree we want to use, one of the things to pay attention to is the **complexity** of the model

The **loss function** to **fit each tree** in XGBoost is:
$$f(w_{1},w_{2},\dots,w_{L})=\frac{1}{2}\sum\limits^{n}_{i=1}(w^{l}_{i}-r_{i})^{2}$$
- Where the **actual ground truth** is used *instead* of $r_{i}$ for the first tree, $m=1$

Training process **monotonically decreases** the **training error**
- Ensemble can **overfit** if trees are *too deep* or there are *too may* trees
- To **restrict** the **depth** of the tree, XGBoost adds a **penalty** (*regularisation*)

$$f(w_{1},w_{2},\dots,w_{L})=\frac{1}{2}\sum\limits^{n}_{i=1}(w^{l}_{i}-r_{i})^{2}+\lambda||w||_{0}$$
- L0-norm enforces all $w_{l}=0$
	- What happens if $w_{l}=0$?
		- The current prediction can **no longer** be **improved**
		- Hence, splitting should be **stopped**
	- To *avoid* working with L0 norm, the L1 norm is commonly used instead:
$$f(w_{1},w_{2},\dots,w_{L})=\frac{1}{2}\sum\limits^{n}_{i=1}(w^{l}_{i}-r_{i})^{2}+\lambda||w||_{1}$$
Note that the **residuals** *act* like the **quality** values used by AdaBoost
- Focus on **decreasing error** for those examples with **large residuals**

