Matrix Computation of the Element-Wise method

![[Pasted image 20260209160615.png]]

In $W$, the number of **rows** corresponds to the number of **neurons in the hidden layer** within the network and the number of **columns** corresponds to the number of **input values** to the network

Next, we can **extend** the matrix-based representation to calculate the feed-forward propagations **iteratively**

![[Pasted image 20260209161337.png]]

Examples in the Lecture Slides

**Vectorisation** is an **optimisation technique** that converts the algorithm from operating on **single values** at one time to operating on a **set of values**
- Modern CPUs provide **direct support** for vector operations where a single instruction is applied to multiple data (SIMD). Multi-Core CPUs lead to **orders of magnitude** *speed-up gains*

Further, we can use this to compute **matrix** calculations on **sets of vectors**.

![[Pasted image 20260209161637.png]]

- Instead, with vectorisation techniques we can merge the vectors into a single matrix and compute the calculations at the **same time** on the GPU, since they are **specialised for these operations**

