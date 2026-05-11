Aka McCullock and Pitts Neuron, or **Linear threshold gate** model
- Accepts 0 or 1 inputs and produces a 0 or 1 output **(binary)** based on a certain **threshold value** (user-specified)

**Inspired** the **biological neuron**

![[Pasted image 20260126163333.png]]

There are equivalences between the **biological** and **artificial** neurons:

ANN - **Inputs**
BNN - **Dendrites**
- Receives **signals** from **other neurons**

ANN - **Node** (unit)
BNN - **Soma**
- **Adds** the **incoming signals**
- **Generates** the **action potential** if the threshold is met

ANN - **Output**
BNN - **Axon terminal**
- **Transmit** the action potential to **other neurons**, or to **structures** outside the nervous system

ANN - **Weights**
BNN - **Synapses**
- **Strength** (amplitude) of a connection between neurons

ANN vs BNN:

![[Pasted image 20260126163820.png]]

---

![[Pasted image 20260126163801.png]]


We have **two types** of input in an MP neuron: **excitatory** and **inhibitory**

![[Pasted image 20260126163833.png]]

An MP neuron can be depicted as a **Rojas diagram**:
- The node is **divided** into a **white half** and a **black half**
- The **threshold** $\theta$ is written on the **white half** of the node
- For each **inhibotory** input, a **small circle** is drawn at the **end of its edge**

![[Pasted image 20260126164031.png]]

---

**Vector representation**

![[Pasted image 20260126164419.png]]

**Heaviside function**

![[Pasted image 20260126164437.png]]

Next, when we apply the threshold function to the sum of the value
- The MP neuron can be represented concisely as follows:

![[Pasted image 20260126164650.png]]

---

**Using** the MP neuron to **emulate** logic gates

We have AND, OR and NOT.
- We can derive other gates such as NAND, and IMPLY gates by combining these basic ones

In general, we:
- Write the **truth table**
- Find **threshold $\theta$**
- Depict the Rojas Diagram

For a NOT gate:

![[Pasted image 20260126165236.png]]


![[Pasted image 20260126165648.png]]

For any number of inputs of AND gate, we just set the threshold to being the number of inputs. I.e. for 16 inputs, threshold is 16. (all need to fire)

For OR, just set the threshold to 1.

![[Pasted image 20260126165843.png]]

Same with multi-input OR gate.

---

You can also represent the gates using a region below/above a **straight line** (MP neuron can only represent a straight line): 

![[Pasted image 20260128121437.png]]

Therefore, if we want to **show** that a function can be modelled by an MP neuron, we can:
- Draw a graph and show the geometric interpretation via a plane or line