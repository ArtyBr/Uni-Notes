- **Inputs** are example sets of **features**
- **Outputs** are desired class labels based on the input
Goal of **Supervised learning**:
- Use **data** to find a **model** that outputs the **correct label** *based* on the **features**

**Naive classifier** - counts how many times each label occurs in the collected data, and predicts the *most common* label, i.e. the **mode**
- e.g. 4 days of upset stomach vs 1 day of non-upset - *predict* **upset**
- *Ignores* the input features (what type of food was eaten)
# Decision Trees
A **decision tree** is a *simple program* consisting of:
- A nested sequence of **if-else** decisions *based on* the features -> **splitting rules**
- A **class label** as a *return value* at the end of each sequence

![[Pasted image 20250525135644.png]]

There are **many possible** decision trees:
- We would like to **search** for a tree that **performs well** within the *context* of our **sample data**
- To *search* for a decision tree, the input is the collected data and the output is a **program** or **model**
	- This is called **training** the model in supervised learning
Supervised learning is useful when we have *lots* of **labelled data**, but:
- The problem is **too complicated** to write a program *ourselves*
- Human experts *cannot explain* the assignment of certain labels
- There is *no human expert* for the problem
## Learning a decision stump
A **decision stump** is the *simplest* decision tree 
- 1 **spitting rule** based on thresholding 1 **feature**
- ![[Pasted image 20250526113515.png]]
How do we find the **best rule** (feature, threshold and leaf labels)?
- Define a **score** (metric) for the rule
- *Search* for the rule with the **best score**

**Classification accuracy** tells us how many examples are labelled correctly *if we use this rule*
- Example: ![[Pasted image 20250526114259.png]]
The **score** evaluates the *quality* of a rule

![[Pasted image 20250526114354.png]]

Highest scoring rule: `egg > 0` which leaves **upset** and **not upset**
- Only need to test feature thresholds that *happen in the data*
## Supervised learning
Organise the training data into a **matrix** with **labels** in a **vector**
- Feature matrix $X$ has $n$ rows as **examples**, $d$ columns as **features**
	- $x_{ij}$ is feature $j$ for example $i$ (quality of food $j$ on day $i$)
	- $x_{i}$ is the row (vector) of all features for example $i$ (all the quantities on day $i$) - **feature vector**
	- $x_{j}$ is the column (vector) $j$ of the matrix (the value of feature $j$ across all examples)
- Label vector $y$ contains the labels of the $n$ examples
	- $y_{i}$ is the label of example $i$ (1 for *upset*, 0 for *not upset*)

![[Pasted image 20250526115039.png]]

**Training phase:**:
- Use $X$ and $y$ to find the best performing **model**, e.g. a decision stump
**Prediction phase**:
- Given an example $x_{i}$, use the trained **model** to *predict* a label $\hat{y_{i}}$ (upset or not upset)
**Training error**
- Fraction of times our **prediction** $\hat{y_{i}}$ does *not* equal the true $y_{i}$ label
- The **true label** is called the **ground truth**

## Decision Tree learning
Decision stumps have only 1 rule based on only 1 feature
- Very *limited* class of models - usually *not* very accurate
Decision **trees**  allow *sequences* of splits based on *multiple* features
- Very *general* class of models - can achieve a *very high accuracy*
- Usually very **computationally expensive** to find the **best** decision tree
	- We need to go over **several** features and thresholds to find the best performing tree
Most common *decision tree learning* algorithm:
- **Greedy recursive splitting**
### Greedy recursive splitting
- Find decision stump with **best score**:
	- ![[Pasted image 20250526123725.png]]
- This stump allows splitting data into **smaller sets**:
	- ![[Pasted image 20250526123743.png]]
- **Fit** a decision stump to each leaf's data:
	- ![[Pasted image 20250526123839.png]]
- **Add** new stumps to the tree
	- ![[Pasted image 20250526124657.png]]
- New decision tree **depth** = 2
	- **Depth** - number of steps before prediction
	- Data split into four smaller datasets
- Depth of the tree can increase, *continue splitting until*:
	- Leaf nodes **assign labels** **correctly** to all training samples *or*
	- We reach a user-defined **maximum depth**
## Metrics for decision trees
### Accuracy
**Accuracy** is a good metric for **leaf nodes** $\rightarrow$ *maximize accuracy*
- Accuracy may *not* be a good metric for **internal nodes**
- Consider a dataset with 2 features and 2 classes - positive (+) and negative (-)
- ![[Pasted image 20250526124917.png]]
Can we separate the classes by a decision stump?
- No, testing whether $x_{i_{1}}>th$ or $x_{i_{2}}>th$ divides space horizontally or vertically only
### Information gain
Instead, most common metric is **information gain**
- Split so that the **entropy** of the **labels** *decreases* the *most* or we *gain the most information*
What is **entropy?**
- **Entropy** measures **randomness** of a set of variables
	- Measures the **spread** of values
- For a categorical variable that can take a total of $C$ values, the entropy is:
$$
entropy = -\sum^C_{c=1}p_{c}\log_{2}p(c)
$$
- Where $p_{c}$ is the *proportion of times* we have value $c$
- **low entropy** means *very predictable*
	- ![[Pasted image 20250526125413.png]]
- **high entropy** means *very random*
	- ![[Pasted image 20250526125422.png]]
	- Minimum value is 0, maximum value is $\log_{2}C$

**Score function** - Choose the split that *decreases* the entropy of the labels the most; i.e. the one that gives the *highest* **information gain**
$$
IG = entropy(y)-\frac{n_{yes}}{n}entropy(y_{yes})-\frac{n_{no}}{n}entropy(no)
$$
- $entropy(y)$ - entropy of the labels **before split**
- $entropy(y_{yes})$ - entropy of labels for samples **satisfying** given rule
- $\frac{n_{yes}}{n}$ - no. of samples satisfying the rule divided by number of samples

Information gain *large* if labels are **more predictable** or **less random**
- Even if it does not increase classification accuracy at *one depth*, this metric can make classification easier at the *next depth*

![[Pasted image 20250526130046.png]]

![[Pasted image 20250526130059.png]]
