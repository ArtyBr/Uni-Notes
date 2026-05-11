# Supervised Learning
*"Teach by example"*
Train the model on **labelled** data to get better predictive accuracy
- The process of learning from training dataset can be thought of as a 'teacher' supervising the learning process
- Given $x, y$, learn a function map $x\rightarrow y$

Examples:
- Regression
	- Input: Ordered data
	- Output: Continuous, real number
- Classification
	- Input: Unordered data
	- Output: Discrete, labelled class

Neural computing is about using **data** to train a **neural network**

![[Pasted image 20260114122447.png]]

# Unsupervised Learning
Train the model on **unlabelled** data with no guidance
- There is no correct answers and there is no 'teacher' to supervise the learning process
- Goal: learn the **underlying structure** of input $x$

Examples:
- Clustering
	- (k-means)
	- Input: No. of centroids (user-specified)
	- Output: averaging data as the centre
- Association
	- (Recommendation system)
	- Structural Similarity search
		- Similar items bought by similar customers

![[Pasted image 20260114122722.png]]
# Semi-Supervised Learning
SSL is a hybrid learning technique that uses **labelled data** to **ground predictions**, and **unlabelled data** to learn the **shape** of the larger data distribution
- Compared with SL, SSL can achieve **higher accuracy** with **less labelled data**
- SSL is particularly useful when labelled data is scarce or expensive to obtain

SSL is based on the **continuity** and **cluster** assumptions

**Continuity assumption**:
- **Close-together** data points area likely to have the same label
**Cluster assumption**:
- Data points of the **same cluster** are likely to have the same label

There are a few methods of conducting semi-supervised learning
## Inductive Learning
Inductive learning trains the model on a labelled dataset.
- The goal is to learn a **general rule (model)** that can be applied to make **predictions** on *new, unseen data*

Examples:
- Linear/Logistic regression
- SVM

**Self-learning**
1. Train supervised model on labelled data $L$
2. Test on unlabelled data $U$
3. Add the most confidently classified member of $U$ to $L$
4. Repeat 1-3 until no unlabelled data

![[Pasted image 20260114124033.png]]

## Transductive Learning
Transductive learning aims to make predictions specifically for the **unlabelled data** in the training set, given a set of labelled and unlabelled data, **without** necessarily learning a **general rule** for new, unseen data.

![[Pasted image 20260114124514.png]]

Unlike inductive learning, which splits data as training and testing set, transductive learning trains a model on the entire dataset using all of the available data points (both labelled and unlabelled).

Examples:
- K-NN
- Label propogation
- Semi-supervised GNN (Graph Neural Network)

# Reinforcement Learning
*"Teach by experience"*
Enforce the model to learn how to make decisions based on **feedback** 
(No predefined data)
- Invented by Google DeepMind, this studies how an agent can learn to behave via **feedback and interaction** with an environment to **maximise rewards**
- There is no true answer, but a reinforcement agent decides what to do to perform the task. In the absence of training data, it is bound to **learn from experience**
- Given: state-action pairs
- Goal: Maximise future rewards over $x$ time steps

An agent takes **actions** in an environment, which is **interpreted** into a **reward** and a **representation of the state**, which are **fed back** into the agent

# Pavlovian Conditioning
We condition the agent to expect some condition based on another learned condition that we would like it to associate with it.

We combine the response with the condition to teach the agent to associate the two, then when we take away the response, the agent still expects the response when the condition is met.

![[Pasted image 20260114125538.png]]