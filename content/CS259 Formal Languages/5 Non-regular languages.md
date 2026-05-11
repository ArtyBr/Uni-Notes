Let $L=\{O^{n}1^{n}|n\geq 0\}$
Is $L$ regular? **No** - DFA would need to somehow count the 0s infinitely and have an infinite amount of paths to count the 1s same number of times
## **Proving non-regularity**
### Using the index of a language to show non-regularity
First - recall extended transition function of a DFA $$\hat{\delta}:Q\times\Sigma^{*}\rightarrow Q$$
$\hat{\delta}(q,w)$ = the state that the DFA reaches when it **starts** in $q$ and **reads** a string $w$
- $\hat{\delta}(q,x.y)=\hat{\delta}(\hat{\delta}(q,x),y)$

![[Pasted image 20240128132331.png]]

After reading all of these strings, the DFA will get into the same state

There is an obvious **equivalence relation** given by the transition function - which state the DFA will end up in given some input string

- Two strings $x$ and $y$ are **distinguishable** by $L$ if there is a string $z$ such that $x.z \in L$ but $y.z \notin L$ or vice-versa

![[Pasted image 20240128132714.png]]

- Strings $x$ and $y$ are **indistinguishable** otherwise, in which case we write $$x\equiv_{L}y$$
 If $L$ is a **regular** language, then $\equiv_{L}$ has a **finite** index
 - So, if $\equiv_L$ has **infinite** index, then $L$ must be **non-regular**

**Myhill-Nerode theorem**
- $L$ is a **regular language** iff $\equiv_L$ has a **finite index** 

To prove that $L=\{O^{n}1^{n}|n\geq 0\}$ is non-regular, we need to:
- Provide an infinite set of strings and
- Prove that they are pairwise **indistinguishable**
This will prove that they must all lie in distinct equivalence classes of $\equiv_L$ and so $\equiv_L$ must have an **infinite** number of equivalence classes

![[Pasted image 20240128142051.png]]

### Pumping Lemma for Regular Languages
If $L$ is a regular language, then there exists $m>0$ such that for every long enough string in the language ($|w|>m$) there exists a decomposition $w=xyz$ such that 
- $|y|>0$
- $|xy|\leq m$ such that
- for all $i\geq 0,xy^{i}z\in L$

