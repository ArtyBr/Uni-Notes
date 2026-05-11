A **Turing machine** is a 7-tuple ($Q,\Sigma,\Gamma,\delta,q_{0},q_{accept},q_{reject}$) where $Q,\Sigma,\Gamma$ are all **finite sets** and 
- $Q$ is the set of states, 
- $Σ$ is the input alphabet not containing the **blank symbol** $\sqcup$, 
- $Γ$ is the tape alphabet, where $\sqcup ∈ Γ$ and $Σ ⊆ Γ$, 
- $δ : Q × Γ\rightarrow Q × Γ × \{L, R\}$ is the transition function, 
- $q_0 ∈ Q$ is the start state, 
- $q_{accept} ∈ Q$ is the accept state, and
- $q_{reject} ∈ Q$ is the reject state, where $q_{reject} \neq q_{accept}$

**Transitions**
- $a\rightarrow b,(L|R|S)$
	- **Read** $a$
	- **Write** b
	- **Move** to the **(left, right or stay)**
- ![[Pasted image 20240225130921.png]]
###### Example machine

![[Pasted image 20240227091312.png]]

**Informal description:**

![[Pasted image 20240227091318.png]]

**Formal Description/State transition diagram:**

![[Pasted image 20240227091420.png]]

###### Example 2

![[Pasted image 20240227091544.png]]

**Informal Description:**

![[Pasted image 20240227091558.png]]



At any point, the turing machine only cares about:
- The **state**
- The **tape contents**
- The **reading head position**

$X=(u,q,v)$ is a configuration if **current state** is $q$, the tape contains the **string** $uv$ and the **reading head** is on the **first symbol** of $v$

![[Pasted image 20240227091748.png]]

In **this** case, $X=(\vdash1011,q,01111)$

![[Pasted image 20240227091949.png]]

Red = $u$, Green = $v$

Configuration $X$ **yields** configuration $Y$ if the Turing Machine can legally go from $X$ to $Y$ in **one step**

**Terminology**
- **Start** configuration
	- $(\vdash,q_0,w)$
- **Accepting** Configuration
	- $(u,q_{accept},v)$
- **Rejecting** Configuration
	- $(u,q_{reject},v)$

An **accepting** or **rejecting** configuration = **halting** configuration

- Consider a sequence of configurations $C_{1},...,C_{r}$ such that $C_{1}$is the start configuration and for $i=1,...,r-1$ the configuration $C_{i}$ **yields** $C_{i+1}$

This is called the **run** of $M$ **on** $w$

$M$ **accepts** the word $w$ if the run $C_{1},...,C_{r}$ of $M$ on $w$ is such that 
- $C_{1}$ is the **start** configuration **and**
- $C_{r}$ is an **accepting** configuration

**Possible behaviours** of a Turing Machine on an input
- **Halts and rejects** - **Decider**
- **Halts and accepts** - **Decider**
- **Doesn't halt**

A language $L$ is **Turing Recognizable** if it is the language accepted by **some** Turing Machine
- i.e. There is a TM $M$ such that every string in $L$ is accepted by $M$ **and** no string which is **not contained** in $L$ is accepted by $M$
- **Not accepted** = **Either rejected OR doesn't halt**
This language class is also called **Recursively Enumerable**

For a **decider**/**total** TM $M$, $L(M)$ is the language **decided** by $M$
- All words will either be accepted or rejected - will never **not halt** (**always halts**)
- "*Is there an algorithm for...*" = "*Is there a decider which recognises this language*" and vice versa

If an **infinite loop** occurs, the **sign** for this is **if the machine appears in the same configuration twice in a run**

Questions for Turing machines;

![[Pasted image 20240227095533.png]]


### Robustness of Turing Machines

#### **What if the machine has 3 tapes instead of 1?**

![[Pasted image 20240229100826.png]]

We can **simulate** 3 tapes using 1.
- Just make the tape alphabet a **tuple** where each element is a **marked/unmarked** symbol from the original tape alphabet

![[Pasted image 20240229101106.png]]

- First cell = $(a,b,a)$
- Third cell = $(\hat{b},a,b)$
- ... etc.

Start by **copying** the input to the 'first track' of the tape

![[Pasted image 20240229101233.png]]

Simulate each move of the 3-tape machine using multiple moves of the single tape machine
#### What if the work tape is infinitely long in both directions?

![[Pasted image 20240229101520.png]]

Fold the machine in 'half', making a single tape machine where each element is a 2-tuple, as in the last example
#### Equivalence with 2-stack PDAs

If you give a PDA 2 stacks instead of 1, it basically turns into a Turing Machine

![[Pasted image 20240229101636.png]]

^ How to represent a memory tape using 2 stacks