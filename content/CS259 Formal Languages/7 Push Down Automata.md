In order to make a machine for a CFL automata need to have some other abilities.

![[Pasted image 20240210123018.png]]

What data structure should be used to store symbols as they're processed?
- Always trying to match the **newly read input symbol** to the **last** stored symbol

**Last in, First out** - **Stack**

Finite Automaton + Stack = **Push Down Automaton**

A **pushdown automaton** is a 6-tuple $(Q, \Sigma, \Gamma, \delta, q_0, F)$, where $Q, \Sigma, \Gamma$ and $F$ are all finite sets, and 
- $Q$ is the set of states
- $\Sigma$ is the input alphabet 
- $\Gamma$ is the stack alphabet
- $\delta : Q \times \Sigma_{\epsilon} \times \Gamma_{\epsilon}\rightarrow P(Q \times \Gamma_{\epsilon})$ is the transition function, 
- $q_0 \in Q$ is the start state
- $F ⊆ Q$ is the set of accept states

By default, a Push-Down Automata is **non-deterministic**

---

**Transitions in PDMs**:

![[Pasted image 20240210124031.png]]

**From** $X$, **read** '$a$', **pop** '$c$', **push** '$b$' and **move to** $Y$
- In the transition function, we have $\delta(X, a, c)=(Y,b)$

But sometimes, we want to be able to:
- Transition **without reading anything**
	- "Read $\epsilon$" is equivalent to saying you can move **without reading the next symbol in the input**

- **Only pop** the stack and **not push**
	- "Push $\epsilon$" is equivalent to saying **don't push anything on to the stack**

- **Only push** something on to the stack **without popping**
	- "Pop $\epsilon$" is equivalent to saying **don't pop anything from the stack**
		- i.e. doesn't matter what the top of the stack is

