A DFA has **exactly** 1 transition out of a state for each symbol in the alphabet
An NFA can have **none** or **multiple** transitions out of a state on reading the same symbol

![[Pasted image 20240118100800.png]]

NFA allows for string searching as such:
- For 'chicken':
![[Pasted image 20240118101622.png]]

Whereas Deterministic counterpart (DFA):

![[Pasted image 20240118101651.png]]

Consider an NFA $M=(Q,\Sigma, q_{0},F,\delta)$

**Language accepted by NFA**
The language **accepted** or **recognised** by $M$ is denoted by $L(M)$ and is defined as:
$L(M)=\{s\in\Sigma^{*}| (\hat{\delta}(q_{0},s)\cap F) \neq \emptyset \}$

**Run of NFA**
Consider NFA $M$ and a word $s$
- **A** run of $M$ on the word $s$ is a sequence of states $r_{0},r_{1}, ..., r_{n}$ such that:
	- $r_{0}=q_{0}$
	- $\exists s_{1},s_{2},...,s_{n}\in\Sigma\cup \{\epsilon\}$ such that $s=s_{1}s_{2}...s_{n}$ and $\forall i \in [n], r_{i}\in \delta(r_{i-1},s_i)$

NFAs are **not more powerful** than DFAs
- Since any NFA can be simulated by a DFA with potentially many more states
- The class of languages accepted by NFAs is a strict **superset** of the class of languages accepted by DFAs (regular languages)
	- Can design a DFA whose extended transition function 'mimics' that of the given NFA

Example:

![[Pasted image 20240118103036.png]]

- $\hat{\delta}(q_{0},a)=\{q_{0},q_{1},q_{2}\}$
- $\hat{\delta}(q_{0},b)=\{q_{0},q_{1},q_{2}\}$
- $\hat{\delta}(q_{1},b)=\{q_{0},q_{1},q_{2}\}$
- $\delta(q_{0},a)=\{q_{2}\}$

Reminder:
- $\delta$ is the transition function for **1** jump
- $\hat{\delta}$ is the transition function for **all** possible jumps

>[!note] Epsilon Closure
>ECLOSE($q$) denotes **all** **states** that can be reached from $q$ by following $\epsilon$-transitions alone

![[Pasted image 20240118103527.png]]

![[Pasted image 20240118103804.png]]

![[Pasted image 20240118104350.png]]

- $\hat{\delta}(7,a)=\{5, 7, 8, 9, 10\}$
### Subset Construction
Build a DFA that **simulates** all of the runs of an NFA simultaneously

General description:
- Given NFA $N=(Q, \Sigma, q_{0},F,\delta)$
- and target DFA $M = (Q_{1}, \Sigma_{1}, q_{1},F_{1},\delta_{1})$
	- $Q_{1}=2^{Q}$
	- $q_{1}=ECLOSE(q_{0})$
	- $F_{1}=\{X\subseteq Q|X \cap F \neq \emptyset\}$
	- $\delta_{1}(X,a)=\cup _{x\in X}ECLOSE(\delta(x,a))$
	- $=\{z|\text{ for some }x\in X,z \in ECLOSE(\delta(x,a))\}$
		- $X$ is the set of possible states in the NFA
		- $x$ is a single state possible in the DFA

![[Pasted image 20240119092229.png]]

![[Pasted image 20240119092408.png]]

Upon reading $s_{1}$, gets to a state which represents all of the $\alpha$ states of the NFA
Then upon reading the $a$, reaches a state which represents all $\beta$s and $\gamma$s - the ECLOSE(all $\beta$s)
#### Example of doing subset construction
NFA:
![[Pasted image 20240119092912.png]]

DFA will have 8 states:
Construction:

![[Pasted image 20240119093958.png]]

For every word $s$

![[Pasted image 20240119095345.png]]

A language $L$ is called **regular**
iff it is accepted by some DFA
iff it is accepted by some NFA!


![[Pasted image 20240119095421.png]]

