Only need to learn:
[[2 Learning Paradigms]]
Q:
Describe the main differences between **Classification** and **Clustering**
- Give one real-world example for Classification and one for Clustering
A:
- Classification is used for **supervised learning**, whereas clustering is used for **unsupervised learning**
	- Labels are given for classification training, whereas data points are not pre-labelled for a clustering problem

- For classification, there are predefined labels assigned to each input instance according to their properties, whereas for clustering thsoe labels are missing
- For classification there a need of **training and testing** dataset for verifying the model created, but there is no need for training and testing dataset in clustering
An example of classification is SVMs, whereas an example of clustering is k-means clustering example.
---
Q: 
- Describe what you understand by Pavlovian conditioning, and how you would train a pavlovian dog
A:
- During conince I think that has a better flow to it for revisionditioning we would like to associate the stimulus (bell) with the response of Salivation from the dog by teaching it that it us
---
[[3 Transfer Learning]]
[[4 Biological Neuron]]
Q:
- Describe three major types of biological neurons
- Provide an example on how they work collaboratively
A:
- Sensory, Motor and Relay neurons
- Sensory neurons are in the receptors, and tell the brain what is happening, causing our sensations
- Relay neurons transmit the communications across the CNS from our Relay neurons to the Motor Neurons
- Motor neurons activate the motor cortex muscles based on the signals they get
---
[[5 Action Potential & Synaptic Transmission]]
Q:
- Figure depicts membrane potential changes
- Explain why the membrane potential is increased slightly in Stage 2 bu increased sharply in Stage 3
A:
- Initial stage, we are resting so the channels are closed
- Stimuli cause some voltage-gates sodium channels to open (small increase in potential)
- Once we get to the threshold of -55mV (threshold), the voltage-gated sodium channels will all open allowing a rapid rush of sodium ions into the cell, resulting in a sharp increase of voltage within the cell
---
[[7 MP Neuron]]
Q:
- Emulate the 3-input OR gate with MP neuron. Draw it's Rojas Diagram.
A:
- Draw truth table (16 rows)
- Write the threshold value necessary as a result (sum of all $x$s, and sign depends on whether output is 0 or 1)
- Pick the most limiting values for the threshold value $\theta$ value
---
Q: 
- Consider the following function : $f=0 if (x_{1}, x_{2}, x_{3}=(0,0,0)), 1 otherwise$
- Show that $f$ can be modelled by a single McCullock-Pitts neuron
- Find appropriate values for its synaptic weights and threshold
A:
- One way: draw rojas diagram with threshold value
- Can plot this in 3D alternatively
	- Show that a single hyperplane can split apart the values (linearly separable)
	- e.g. $x_{1}+x_{2}+x_{3}=1$ is a hyperplane that splits them apart
---
[[8 Single-Layer Perceptron]]
[[9 Multi-Layer Perceptron]]
- Drawing multiple-decision boundary thresholds with MLP
---
[[10 Activation Functions]]
- Relationship between Sigmoid and TanH
- Sigmoid is a **rescaled version of tanh**
- $tanh(x)=2\sigma (2x)$
- Derivative of Sigmoid
- Derivative of Tanh
- Some of the functions' graphs and their limitations
---
[[11 Forward Propagation]]
- Compute output using Matrix and element-wise methods
	- Common mistakes with matrix:
		- when a neuron has no bias value, give it a value of 0 (need to remember bias values)
	- z = W . x + b
	- a = ReLU(z)
- Apply **vectorisation** to forward propagation
---
[[12 Loss Functions and Regularisation]]
Specifically:
- Overfitting
	- Networks learns the details of the training data, meaning it is unable to generalise to the testing data (unseen data)
- , and how to prevent it using:
	- Put limitations on the number of parameters to learn (However NP-hard, so we come up with some potential ways of doing it)
- Lasso Regularisation
	- L1 norm is used
		- L1 norm has corners. It is very likely that the meeting point is at one of the corners
		- Indicates some sort of sparsity for the training weights
		- Therefor generates **sparser solutions** than L2
	- $\underset{w}{min}L(y,\hat{y})$
	- $||w||_{1}=N$
- Ridge Regularisation
	- L2 norm is used
		- L2 norm has no corners, it is very unlikely that the meeting point is at one of the axes
	- $||w||_{2}^{2}=N$
---
[[13 Hopfield Network]]
- Given the following memorised patterns for bipolar HN, determine its connection weight $W$
	- First we vectorise each pattern (column-wise vectorisation - go down by columns and not across by rows)
	- Then get the transposed one and get their product 
	- Then get the average of the weight matrices and then 0 out the diagonals (-I)
	- After we get the weight matrix $Z$ we can test its stability

- Design a bipolar HN of $3$ neurons that **remembers** a single pattern (1,1,-1). Determine if this pattern is a stable state
	- $x=[1,1,-1]$ $W=x\cdot x^{T}-I =...$
	- If the result of applying it to +1 of the original, is equal to the original, then stable
---
[[14 Backpropogation]]
- Way of **training a neural network**
- First to forward pass (forward propagation)
- Apply each corresponding function to get the next result as a function of the previous inputs
- Then calculate partial derivative of each stage **backwards** (back propagation)
- Multiply these partial derivatives together in order to get total derivative (chain rule)

- Can get a concrete neural network with weights and biases and need to calculate the derivative of one of the weights in relation to the output.
---
[[15 Recurrent Neural Networks]]
- RNN Output Predictions
- Elman Network: Feedback from Internal State Output to input
- Jordan Network: Feedback from Network Output to Input

- LSTM
- Various type of RNNs
	- One-to-many, Many to one, Many to many
	- Particularly Many to many based on whether the number of input or output signals are the same, we need to know the applications of each architecture
		- NER for $|X|=|Y|>1$
			- When input and output layers are the same size, the most popular application of many-to-many RNN is NER
		- If $|X|\neq |Y|$, an encoder is a very popular application
---
[[16 Natural Language Processing]]
- Need to know each phase, and what happens at each phase with a toy example to demonstrate
	- Particularly for normalisation we should know the difference between stemming and lemmatisation
	- Remember the short examples for the words
- PoS Tagging

- Document-level representation
	- Sparse storage (Yale, COO)
	- TF x IDF
	- N-grams

- World-Level representation
	- CBOW (Continuous bag of words)
	- Skip-Gram

- Hierarchical Softmax
	- Using Huffman trees to construct word embeddings 
---
[[17 HITS model]]
- Three algorithms for computing HITS
	- Iterative approach
	- Dominant Eigenvector
	- SVD-based method
		- Proofs of the decomposition of the adjacency matrices
		- No algorithm questions
---
[[18 Graph-based Similarity Search]]
- Properties of SimRank measure
	- Reflexivity, Symmetry, Boundedness, Transitivity
	- Check the properties of the similarity measure
	- Whether it is bounded or transitive?
	- To prove these, we use the induction approach
	- For proof of transitivity, we can discuss the different cases (incoming neighbours or no incoming neighbours) whether it is transitive or not
---
[[19 Generative Adversarial Network (GAN)]]
- Derive the optimal discriminator $D$ formula
[[20 Graph Neural Network (GNN)]]
