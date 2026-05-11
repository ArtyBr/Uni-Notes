Same as a Turing Machines, but has a **dedicated output tape**
- Finite set of states, with one special “enumeration’’ state
- **Read/write** work tape 
- **Write only** output tape
Always **starts** with **blank work tape**

When it enters an “enumeration’’ state, the word **contained** in the **output tape** is said to be “enumerated’’ 
- The machine then **erases** the output tape, sends the write only head back to the **beginning** of the output tape, leaves the work tape untouched, and continues

Formally, $L(E)$ = the **language** enumerated by the enumerator $M$
- $\{w|w\text{ is enumerated by }M\}$

$L$ is Turing Recognizable iff some enumerator enumerates it
- **Recursively enumerable = Turing recognizable**

Given an enumerator $E$, how do we convert it to a Turing Machine $M$ accepting $L(E)$

![[Pasted image 20240229102418.png]]

**Note**: If $w$ is not in $L(E)$, then $M$ may never halt

Other direction:

Given a TM $M$, how do we convert it to an enumerator $E$ accepting $L(M)$?

![[Pasted image 20240229102654.png]]

