**Definitions:**
- $Q$ - Finite set of **states**
- $\Sigma$ - Set called the **alphabet**
- $\Sigma^{*}$ - Set which consists of **all possible strings** over a language
- $q_{0}\in Q$ - In which state to **start** the computation
- $F \subseteq Q$ - **Final/accepting** states
- $\delta : Q \times \Sigma \rightarrow Q$ - **Transition** function (from one state to another)

A **machine** $M$ defined by the tuple $M=(Q, \Sigma, q_{0}, F, \delta)$ is called a **(deterministic) finite state automaton** or a **(deterministic) finite state machine**

**Representing** a DFA:

![[Pasted image 20240116103231.png]]

How would this **compute**?

| Input      | Output |
| ---------- | ------ |
| a          | Accept |
| aa         | Accept |
| abcbc      | Reject |
| bcabc      | Reject |
| $\epsilon$ | Reject | 

#### Examples:

![[Pasted image 20240116103718.png]]


![[Pasted image 20240116103706.png]]

# Formalizing how a DFA computes
**The empty word** - $\epsilon$
- $|\epsilon|=0$ (length of $\epsilon$ is 0)
- $L_{1}= \{\}$ is the **empty language**, $L_{2}=\{\epsilon\}$ is a **non-empty** language
- $\Sigma^{*}$ always **contains** $\epsilon$
#### Monoids
**Comprise** of a **set**, an **associative binary operation** on the set with an **identity element**
- $(\mathbb{N}_{0}, +, 0)$ is a monoid
	- Here, + denotes **addition**
- $(\mathbb{N}, \times, 1)$ is a monoid
	- Here, $\times$ denotes **multiplication**
- $(\Sigma^{*},\circ, \epsilon)$ is a monoid
	- Here, $\circ$, denotes **string concatenation**
	- $ab\circ cd=a\circ b\circ c\circ d = abc\circ d = abcd \circ \epsilon$
### Extended transition function
$\delta$ - The **transition function** of a DFA expresses the **change in state** upon **reading** a **single symbol**
$\hat{\delta}$ - The **extended transition function** of a DFA expresses the **change in state** upon **reading** a **string**

![[Pasted image 20240116105057.png]]

Formally, the extended transition function $\hat{\delta}$ is a **function** $\hat{\delta}$ : Q × $Σ^*$ $→ Q$ and is defined as follows: 
- For every $q ∈ Q$, $\hat{\delta}$$(q, ε)$ $= q$. 
- For every $q ∈ Q$ and word $s ∈ Σ∗$ such that $s = wa$ for some $w ∈ Σ∗$ and $a ∈ Σ,\hat{\delta}(q, s) =$ $δ(\hat{\delta}(q,w), a)$
### Run of a DFA
#### Accepted Languages
Consider a DFA $M = (Q, Σ, q_0, F, δ)$. The **language accepted** or **recognized** by $M$ is denoted by $L(M)$ and is defined as:
$$L(M)=\{s\in\Sigma^* | \hat{\delta}(q_0, s)\in F\}$$
#### Run of DFA on a string
Consider a DFA $M = (Q, Σ, q_0, F, δ)$. Consider a **string** $s=s_{1}s_{2}...s_{n}$, where $s_{i}\in \Sigma$ for each $i \in [n]$.
- The run of $M$ on the empty word $\epsilon$ is just the state $q_{0}$
- The run of $M$ on the word $s$ is a sequence of states $r_{0}, r_{1}, ..., r_{n}$, where:
	- $r_{0}=q_{0}$
	- $\forall i\in [n], r_{i}=\delta (r_{i-1}, s_{i})$
#### Accepted run on a word
- The run of $M$ on a word $s$ is called an **accepting run** if the **last state** in the run is an **accepting** state of $M$
- A word $s$ is said to be **accepted by** $M$ if the run of $M$ on $w$ is an accepted run. That is:
$$L(M)=\{s \in \Sigma^{*} | \text{the run of M on s is an accepting run}\}.$$
