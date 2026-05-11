![[Pasted image 20260211121407.png]]

**Associative Learning** involves **encoding relationships** between items
- e.g. Pavlov conditioning between a stimulus and a response
- A Hopfield Network uses **associative memory** to associate an input (partial/corrupted image)  to a full memorised image

**Associative memory** is the ability to **access an item** by just knowing **part** of its content
- There can be **multiple fixed points** in the Hopfield Network (HN)
- CAM retrieves a similar memorised pattern to the corrupted input, so that the corrupted input can be recognised and 'pulled' to the closest fixed point of the HN

![[Pasted image 20260211121609.png]]

A **pattern** ($n\times n$ pixels) can be represented as an $n^{2}\times 1$ vector $x$, requiring the state information of $n^{2}$ neurons
- Each element $x_i$ denotes a **state** (activity) of a neuron $i$
- For a **discrete** HN, each state $x_{i}$ takes:
	- **Bipolar** values (1 - firing, -1 - not firing)
	- or **binary** values (1 - firing, 0 - not firing)

![[Pasted image 20260211121746.png]]

**Feedforward vs Recurrent**
- Recurrent networks have '**states**' (activations from previous time steps need to be remembered): **Short-Term memory**
- RNN networks are **dynamic** - their 'state' is **changing** **continuously** until they reach an **equilibrium point**

![[Pasted image 20260211121853.png]]

**Hopfield Network**:
- A **special form** of RNNs 
- A **single layer, fully connected** auto associative network
- Neurons act as **both** input and output with a binary threshold
- An **energy-based** network
- Often used for pattern recognition

![[Pasted image 20260211122053.png]]

- The **output** of a neuron is the **input** to **other neurons** but **not the input to itself** (no self-feedback)
- HN is a **complete graph** $G=(V,E)$
	- A node $i\in V$ is a perceptron with a **state** $x_{i}\in \{1,-1\}$ or $\{1,0\}$
	- A pair $(i,j)\in E$ **links** a **weight** $W_{i,j}$ (connection **strength**)
	- Edge is traversed in **both directions** (symmetric: $W_{i,j}=W_{j,i}$)
	- **No self-loops** ($W_{i,j}=0$)

Follows the **Hebbian Learning Rules**
- Neurons that fire together, wire together
- Neurons that fire out of sync, fail to link

**Simultaneous activation** of neurons leads to **increases** in **synaptic strength** between neurons 

![[Pasted image 20260211122405.png]]

---
**Weight matrix** for a single memorised pattern

![[Pasted image 20260211122615.png]]

Now, we can extend this idea to **two patterns**:

![[Pasted image 20260211122922.png]]

Now for **binary patterns** (rather than bipolar)

![[Pasted image 20260211123324.png]]

And the weight matrix for the binary ones:

![[Pasted image 20260211123420.png]]

So how do we update the neuron states?

![[Pasted image 20260211123653.png]]

---

**Stability** of the HN

![[Pasted image 20260211123813.png]]

Why is a **single memorised pattern** *stable*?

![[Pasted image 20260211124049.png]]

For multiple memorised patterns (stability):

![[Pasted image 20260211124336.png]]

![[Pasted image 20260211124509.png]]

![[Pasted image 20260211124547.png]]

![[Pasted image 20260211124351.png]]

---

**Energy** in Hopfield Networks

Energy is the **capacity** for the HN to **evolve**.
- The network **will evolve** **until** it arrives at a **local minimum** in the **energy contour**

The **global energy** $E$ is the sum of many local contributions.
- Each **local contribution** is the **product** of **one connection weight** with the binary states of **two neurons**

![[Pasted image 20260216160346.png]]

- When $\overset{\rightarrow}{s}$ (the states of all neurons for output) agrees well with $W\overset{\rightarrow}{s}$ (the states inputted to all neurons, $E$ becomes the lowest)  
- Hebbian updating rule essentially reduces this energy by aligning neurons output states $\overset{\rightarrow}{s}$ with its incoming inputs $W\overset{\rightarrow}{s}$

![[Pasted image 20260216161608.png]]

![[Pasted image 20260216161614.png]]

**Theorem**:
- The **energy $E$** decreases each time a neuron state changes

![[Pasted image 20260216161637.png]]

