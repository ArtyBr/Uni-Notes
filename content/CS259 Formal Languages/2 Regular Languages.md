A language $L$ is called **regular** if it is accepted by some DFA (deterministic finite state automaton)

**Operations** on Languages
- Languages are **sets** of **strings**
- **Any** set operation can be applied to languages
- Examples of **unary** set operations?
	- *Complementation, Reversal, Truncation*
- Examples of **binary** set operations?
	- *Union, Intersection, Concatenation*

If $L$ is regular, is every subset of $L$ also regular?
No - if $L$ is a language of every single possible state, then there exists an $L'\subset L$ that is non-regular, since we know non-regular languages exist. 
e.g. $L'=\{x\in {0,1}^{*} | \text{number of 0s = number of 1s}\} \subset \Sigma^*$
#### Complementation
Suppose $L$ is regular. Is $\bar{L}$ also regular?
**Yes** - If $M=(Q, \Sigma, q_{0}, F, \delta)$ is a DFA for $L$ then $M'=(Q, \Sigma, q_{0}, Q/F, \delta)$ is a DFA for $\bar{L}$

Regular languages are **closed** under complementation

**Compliment** of a language is just the language but the accepted states are **reversed**
- So:
- If encountering a language and asked if it is regular:
	- If the language is a compliment of a regular language talked about in lectures, then can deduce that it too is regular
#### Intersection
Regular languages are **closed** under intersection

![[Pasted image 20240118115630.png]]

Can we **use** $M_1$ and $M_2$ to decide whether a string is in $L_{1}\cap L_{2}$?

Take the string - give it to $M_1$ and $M_2$ and if both accept then yes, otherwise no

Can we **build** a DFA $M_3$ such that it simultaneously returns the result of running the word on $M_1$ and $M_2$?

General description of the intersection automaton:

![[Pasted image 20240118120050.png]]

Coordinates for the states are combined between both DFAs.
##### Example

![[Pasted image 20240118120241.png]]

- $M_{1}$ starts in $q_{A}$ and $M_{2}$ starts in $q_{C}$, so $M_{3}$ starts in $(q_{A}, q_{C})$
- If they both read a 1:
	- $M_{1}$ transitions to $q_{B}$, $M_{2}$ goes to $q_{C}$, so $M_{3}$ goes to $(q_B, q_{C})$
	- Then if another 1:
		- $M_{3}$ goes back to $(q_{A},q_{C})$
- etc.
- Final state is only the state that contains both final states of $M_{1}$ and $M_{2}$

![[Pasted image 20240119103956.png]]

If different alphabets between $\Sigma_{1}$ and $\Sigma_{2}$:
$\Sigma_{3}=\Sigma_{1}\cap \Sigma_{2}$
#### Union
Regular languages are **closed** under union

Same as intersection, except final states are actually $(F_{1}\times Q_{2})\cup (F_{2} \times Q_{1})$
#### Set difference
$L_{1}\backslash L_{2}=L_{1}\cap \overline{L_{2}}$

Regular languages are **closed** under set difference

If $L_{1}$ and $L_{1}\backslash L_{2}$ are regular, is $L_{2}$ also regular?
False: **counterexample**
- $L_{1}=\emptyset$
- $L_{2}=$ any non-regular language
- Then $L_{1}\backslash L_{2}=\emptyset$ and it is regular but clearly $L_{2}$ is not
