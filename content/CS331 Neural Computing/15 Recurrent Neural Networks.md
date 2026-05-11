Simple RNNs have only a **single unidirectional** hidden layer

![[Pasted image 20260223160754.png]]

However, there are also different **variations** of RNNs:
- **Deep** recurrent neural networks (DRNN)
	- An RNN can be deep not only with respect to **time**, but also to **space** as in a feed-forward network
		- A deep RNN is constructed by **stacking layers** of the RNN together
			- i.e., taking the output hidden state and **feeding** it into **another hidden layer** as an input sequence and **repeating** this process
		- 
		- In Deep RNNs, the hidden state infromation is passed to:
			- The **next** time step of the **current layer**, *and*
			- The **current** time step of the **next layer**
		- There are two ways of **introducing depth** into the RNNs:
			- Expanding looping operations to **multiple hidden units**
			- **Increase computational depth** of a hidden unit
		- ![[Pasted image 20260223161333.png]]
		- ![[Pasted image 20260223161319.png]]
- **Bidirectional** recurrent neural networks (BRNN)
	- Standard RNNs have a **limitation** that the **future** input information **cannot be reached** from the **current state**
		- ![[Pasted image 20260223161457.png]]
	- **Bidirectional RNNs** allow RNNs to have **both backward** and **forward** information about the sequence at every time step
	- BRNNs put **two** **independent RNNs together**,
		- The input sequence is fed in **normal time order** for one RNN, and in **reverse time order** for the other
		- The outputs of the two RNNs are **concatenated** at each time step
		- ![[Pasted image 20260223161616.png]]
- **Long Short-Term Memory** (LSTM) - Gated
	- First, we look at how forward propagation works/looks in the vanilla RNN.
		- We have a **vanishing gradient** problem here:
		- ![[Pasted image 20260223161802.png]]
		- ![[Pasted image 20260223161807.png]]
		- ![[Pasted image 20260223162501.png]]
		- We can 'Alleviate' this **exploding gradient**
			- We can use a clipping function, '*Gradient Clipping*'
		- And we use *Long Short-Term Memory* to avoid '**Vanishing gradient**'
		- There are a few things we want to do to the traditional RNN:
			- ![[Pasted image 20260223163019.png]]
			- ![[Pasted image 20260223163028.png]]
			- ![[Pasted image 20260223163036.png]]
			- We introduce **gates** to a memory cell to **control** its temporal dependency
				- LSTM is a type of RNN that uses **three gates** to control the flow of information passed through the memory cell
					- **Input gate** (i): Controls if data can **enter** the **memory**
					- **Output gate** (o): Controls if data can be **output** from the **memory**
					- **Forget gate** (f): Controls if **all** previous data in the **memory** can be **erased** ('forgotten')
				- ![[Pasted image 20260223163307.png]]
			- A memory cell at time $t$ is associated with 3 gates ($i_{t},o_{t},f_{t}$)
			- For simplicity, we can consider each $(i_{t},o_{t},f_{t})$ to take the **binary value** 0 or 1. In practice, the values of them are *between* 0 and 1, returned by a Sigmoid function
			- ![[Pasted image 20260223163503.png]]
			- The **input gate**:
				- ![[Pasted image 20260223163517.png]]
				- $i=1$ **Allows** new data to be added into memory
				- $i=0$ **Disallows** new data added into memory
			- The **Output gate**:
				- ![[Pasted image 20260223163547.png]]
				- $o=1$ **Allow** memory data sent to hidden unit at next time step
				- $o=0$ **Disallow** memory data sent to hidden unit at next time step
			- The **Forget gate**:
				- ![[Pasted image 20260223163628.png]]
				- $f=0$: **Empty** memory (i.e. clear all previous data in memory)
				- $f=1$ **Do not empty** memory
		- We get the following LSTM equation:
			- ![[Pasted image 20260223163947.png]]
			- ![[Pasted image 20260223163953.png]]
		- First part of the equation - get memory value
			- ![[Pasted image 20260223164227.png]]
		- Second part of the equation - get activation value
			- ![[Pasted image 20260223164243.png]]
		- Third part of the equation - get hidden cell value
			- ![[Pasted image 20260223164307.png]]
		- Finally, we need to do **normalisation**
			- ![[Pasted image 20260223164322.png]]
		- So:
			- ![[Pasted image 20260223164338.png]]
			- ![[Pasted image 20260223165120.png]]
		- This ends up being the '*Hadamard Product*' - The entry-wise vecor product
			- ![[Pasted image 20260223165153.png]]
		- ![[Pasted image 20260223165559.png]]
		- LSTMs have the form of a **chain** of **repeating modules** of a neural network:
			- ![[Pasted image 20260225120623.png]]
- **Gated** recurrent units (GRU) - Gated

---

There are further **variations** of RNNs, based on the input and output types.

- **Many** to **one**:
	- We have multiple inputs but the output is a single value.
	- E.g. Amazon customer reviews:
		- ![[Pasted image 20260225122809.png]]
	- We get multiple words as input and output just a magnitude (0-5)
	- A movie rating system uses review texts as **multiple** inputs to produce a **single** rating for a movie on a scale of 1 to 5

- **One** to **many**:
	- One value gives multiple outputs:
	- e.g. Music composition - we give one note and  the model predicts the rest of the music
		- ![[Pasted image 20260225122946.png]]
	- Many-to-one RNN takes a musical note as a **single** input and **predicts** what the most likely next note to occur is, aiming to create brand bew musical sequences

- **Many** to **many**
	- When the **input** and **output** layers are the same size, the most popular application of many-to-many RNNs is NER (Name Entity Recognition)
	- ![[Pasted image 20260225123113.png]]
- Another variation is when the **lengths** are **not equal**
	- ![[Pasted image 20260225123140.png]]
	- **Machine translation** is a common application of **unequal** many-to-many RNNs, which can return words more or less than the input string (since you may have an unequal amount of words in English that translate to a different number of words in Chinese)

- **Attention**
	- The basic encoder-decoder encodes the **whole** input sequence, regardless of its length, into a fixed-length vecotr, being **inefficient** to deal with long sentences
	- The encoder-decoder with attention model learns to align and translate jointly
		- It allows RNNs to encode the input sequence into a sequence of vectors and chooses a **subset** of these vectors **adaptively** while decoding the translation
	- ![[Pasted image 20260225123608.png]]

