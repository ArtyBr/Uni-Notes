**Generalised** Non-deterministic Finite Automata

![[Pasted image 20240126163232.png]]

- Like an NFA, but each transition function is defined by the **cartesian product of 2 sets of states** as an **input** and a **regular expression** as an **output**
- The arcs representing the transitions in a GNFA are labelled by the **regular expressions** over the language $\Sigma$

![[Pasted image 20240126125101.png]]

- A run of $M$ on a word $s$ is called an **accepting run** if the **last state** in this run is the accepting state of $M$
- A **word** $s$ is said to be **accepted** by $M$ if some run of $M$ on $s$ is an accepting run. Then, the language accepted by the machine $M$ can be defined equivalently, as $$L(M)=\{s\in\Sigma^*|\text{some run of M on s is an accepting run}\}$$
**Claim**: Every NFA can be converted to an equivalent GNFA
**Proof strategy**:
- Make a new unique final state and add $\epsilon$-transitions from the original final states to the new one
- Add all possible missing transitions that:
	- Leave a state other than $q_{f}$
	- Those that enter a state other than $q_0$
- Label these transitions with the regular expression $\emptyset$

