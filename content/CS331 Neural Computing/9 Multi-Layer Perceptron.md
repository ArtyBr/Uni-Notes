An XOR gate **cannot** be represented using a **linear function**
- It is **linearly inseparable**

Proof: no possible solution for it:

![[Pasted image 20260202163025.png]]

There are **two** treatments for this problem:
1. Replace the existing **threshold function** with a more powerful function
	- ![[Pasted image 20260202163207.png]]
	- How can we modify this activation function?
		- Currently, there is only one 'step' that can occur to go from one band to another (1 and 0)
		- ![[Pasted image 20260202163434.png]]
	- However, we can introduce a more complex threshold function which can have two steps and introduce more regions
	- ![[Pasted image 20260202163510.png]]
2. Increase the number of layers while keeping each unit using a threshold function
	- ![[Pasted image 20260202163234.png]]
	- If we instead **increase the number of layers**, we essentially allow the function to have **more steps** between the 0/1 bands
	- ![[Pasted image 20260202164200.png]]


- So, by combining multiple neurons, we **combine** the thresholds and achieve more unique and distinct classification zones/decision boundaries for the function:
- ![[Pasted image 20260202164118.png]]
