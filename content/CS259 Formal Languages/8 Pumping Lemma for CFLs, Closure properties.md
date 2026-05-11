Proving that a language is **not a CFL:**

- Assume that L is a CFL
- Let m be the pumping length given by the pumping lemma
- Choose the string $w=a^mb^mc^m ∈ L$. Clearly, $|w| \geq m$
- Take an arbitrary decomposition $w=uvxyz$ where $|vxy|\leq m$ and $|vy|>0$
- We will now prove that $uv^ixy^iz ∉ L$ for some $i$, contradicting the pumping lemma

##### Example:
- $L=\{a^nb^nc^n|n\geq 0\}$
- Choose $vxy\in c^*$
- ![[Pasted image 20240222103617.png]]
- Now, if you choose $i$ to be 0, of course $uv^0xy^0z\notin L$
- This contradicts the pumping lemma and implies that $L$ is not a CFL
#### Testing Finiteness
If $G$ contains **no strings** of length **longer** than the **pumping length** $m$, then the language is **finite**
- If $G$ contains even **one** string of length **longer** than $m$, it contains a string of length at most $2m-1$

### Closure Properties
- **Union**
	- **Closed**
		- $G_{1}=(V_{1},\Sigma_{1}, R_{1}, S_{1})$
		- $G_{2}=(V_{2},\Sigma_{2}, R_{2}, S_{2})$
		- $G = (V,\Sigma,R,S)$ where:
			- $V=V_{1}\cup V_{2}$
			- $\Sigma = \Sigma_{1}\cup \Sigma_{2}$
			- $R=R_{1}\cup R_{2}\cup \{S\rightarrow S_{1}|S_{2}\}$
			- $L(G)=L(G_{1})\cup L(G_{2})$
- **Intersection** of 2 CFLs
	- **Not closed**
		- $P=\{a^ib^jc^j|i,j\geq 0\}$
		- $Q=\{a^ib^ic^j|i,j\geq 0\}$
		- $P\cap Q= \{a^ib^jc^j|i,j\geq 0,i=j\}$
		- =
		- $\{a^nb^nc^n|n\geq 0\}$, which is **not** a CFL!
- **Concatenation**
	- **Closed**
		- $G_{1}=(V_{1},\Sigma_{1}, R_{1}, S_{1})$
		- $G_{2}=(V_{2},\Sigma_{2}, R_{2}, S_{2})$
		- $G = (V,\Sigma,R,S)$ where:
			- $V=V_{1}\cup V_{2}$
			- $\Sigma = \Sigma_{1}\cup \Sigma_{2}$
			- $R=R_{1}\cup R_{2}\cup \{S\rightarrow S_{1}S_{2}\}$
			- $L(G)=L(G_{1})L(G_{2})$
- **Kleene closure**
	- **Closed**
		- $G_{1}=(V_{1},\Sigma_{1}, R_{1}, S_{1})$
		- $G = (V,\Sigma,R,S)$ where:
			- $V=V_{1}$
			- $\Sigma = \Sigma_{1}$
			- $R=R_{1}\cup \{S\rightarrow \epsilon|S_{1}S\}$
			- $L(G)=L(G_{1})^*$
- **Complementation**
	- **Not closed**
		- $P\cap Q=\overline{(\bar{P}\cup\bar{Q})}$
		- Since CFLs are **not closed** under **intersection**, they are **not closed** under **complementation**

However,
- **Intersection** of a CFL with a regular language
	- **Closed** - since don't need to keep track of 2 stacks being intersected!
		- Let $M_{1}=(Q_{1}\Sigma,q_{1},F_{1},S_{1})$ - DFA
		- Let $M_{2}=(Q_{2},\Sigma,\Gamma,q_{2},F_{2},S_{2})$ - PDA
		- Define $M=(Q,\Sigma,\Gamma,q,F,S)$ where:
			- $Q=Q_{1}\times Q_{2}$
			- $q=(q_{1},q_{2})$
			- $F=F_{1}\times F_{2}$
			- and
			- ![[Pasted image 20240225123703.png]]
		- Basically, for each rule in PDA (read a, push b, pop c) do the reading state - transition to the new state corresponding to both the DFA and PDA, and do the stack operations as normal for the PDA.