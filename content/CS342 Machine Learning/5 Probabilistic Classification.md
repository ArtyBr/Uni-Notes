Example for the lecture:
- Say we want to create an ML model that can **detect spam emails**

Spam filtering as **supervised learning**:
- Collect several e-mails, label them
	- $y_{i}=1$ if email $i$ is **spam**, $y_i=0$ if email $i$ is **not spam**
- Extract **useful** and **discriminative** features - a '*bag of words*'
	- mail represented by the features in this 'bag'
	- $x_{ij}=1$ if **word/phrase $j$ is in email** $i$, else $x_{ij}=0$

![[Pasted image 20251021101508.png]]

- Emails may not have **all** the features in the 'bag of words' - some emails may appear to be **more 'spam-like'** than others

We can use **naive Bayes classification**

**Probabilistic classifiers** model the **conditional probability**, $p(y_{i}|x_{i})$
- **If** an email **has words** $x_{i}$, what is the **probability** that the email **is spam**?
- **Classified as spam** if probability of **spam** is **higher** than probability of **not spam**

![[Pasted image 20251021101734.png]]


To model the conditional probability, naive Bayes classifier used **Bayes rule**:
$$p(y_{i}=spam|x_{i})=\dfrac{p(x_{i}|y_{i}=spam)\times p(y_{i}=spam)}{p(x_i)}$$
We need to train a model that computes $p(y_{i}=spam|x_{i})$
- We have access to **labelled training data** which makes it easier to compute the probability that spam email has features $x_{i},p(x_{i}|y_{i}=spam)$
- Hence, computing $p(x_{i}|y_{i}=spam)$ is **easier** during training than computing $p(y_{i}=spam)$

$p(y_{i}=spam)$ is **easy to compute** from training data
- $p(y_{i}=spam)=\dfrac{|\text{spam emails}|}{|\text{total emails}|}$

$p(x_{i})$ is the probability than an email **has feature vector** $x_{i}$
- Hard to compute - with $d$ words, we need to collect $2^{d}$ samples just to see each word combination onceq
- Feature vectors for spam filtering are usually **high dimensional** - high $d$
- $p(x_{i})=\dfrac{|\text{emails with }x_{i}|}{|\text{total emails}|}$

However, we can **ignore** this term (when comparing two probabilities):

![[Pasted image 20251021102521.png]]

Finally, we want $p(x_{i}|y_{i}=spam)$ - probability that a spam email **has features** $x_{i}$
- Not very easy to compute, but we can **approximate it**
- Not easy to compute **because** we need to check **every possible feature vector $x_{i}$**
- $p(x_{i}|y_{i}=spam)=\dfrac{|\text{spam emails with }x_{i}|}{|\text{spam emails}|}$


We make an **important assumption** about $p(x_{i}|y_{i}=spam)$
- **Assume** each feature $x_{ij}$ in vector $x_{i}$ is **conditionally independent** given label $y_{i}$

- Additionally, it may be **better** to **choose** features such that they appear **more conditionally independent** in order to satisfy this rule **better** than otherwise

The assumption lets us say the following:
$$p(F_{2},F_{1}|spam)=p(F_{2}|spam)\times p(F_{1}|spam)$$
**equally**,
$$p(F_{2},F_{1}| \text{not spam})=p(F_{2}|\text{not spam})\times p(F_{1}|\text{not spam})$$

Meaning that for each word we would like to compute the probability of seeing, we can compute it **independently** of seeing other words.
- Making the calculation **much easier**

![[Pasted image 20251021132205.png]]

![[Pasted image 20251021104553.png]]

Example:

![[Pasted image 20251021104826.png]]

Given a test sample $\tilde{x_{i}}$, the prediction $\hat{y_i}$ is set to the class $c$ that **maximises**:
$$p(\tilde{y_{i}}=c|\tilde{x_i})$$

Under the naive Bayes assumption, we can **maximise**:
$$p(\tilde{y_{i}}=c|\tilde{x_{i}})\propto \Pi_{j=1}^{d}[p(\tilde{x_{ij}}|\tilde{y_i}=c)]p(\tilde{y_{i}}=c)$$

Example:

![[Pasted image 20251021105155.png]]

---

**Laplace Smoothing**

Say we take the word '*Millionaire*' as another feature to detect spam:

$$p(millionaire=1|spam) = \dfrac{|\text{spam emails with word millionaire}|}{|\text{spam emails}|}$$

Assume we have **no spam emails** with the word *millionaire* in our **training dataset**
- $p(millionaire=1|spam)=0$
- Spam emails with the word *millionaire* will **not be filtered** out - word may appear during testing

We can use **Laplace smoothing** to fix this.
- For **binary features**, add 1 to the numerator and 2 to the denominator
$$\dfrac{|\text{spam emails with word millionaire}|+1}{|\text{spam emails}|+2}$$
Laplace smoothing can be useful for **all features**
- Helps against **overfitting** - similar occurrence of individual features in the training data

A common **variation** is to use a real number $\beta$ instead of $1$.
- $\beta k$ to denominator if feature has $k$ **possible values**
$$p(x_{ij}=c|y_i=class)\approx \dfrac{(\text{no. samples in class with }x_{ij}=c)+\beta}{(\text{no. examples in class})+\beta k}$$

In the previous example (of binary classification), $\beta=1;k=2$

---

If we want to extend this to **real number** values of $x_{i}$, then we can fit the **seen** values of $x_{i}$ to a **gaussian** (normal distribution), and use values from the gaussian to **estimate unseen probabilities**