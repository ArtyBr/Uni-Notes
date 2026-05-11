Tuple: $G=(V,\Sigma, R, S)$
- $V$ - Finite set of **variables** or **non-terminals**
- $\Sigma$ - Finite **alphabet**
- $R$ - Finite set of **production rules** or **productions**
	- ![[Pasted image 20240206092438.png]]
- $S\in V$ - **START variable**

You don't stop deriving until you run out of production rules to apply

![[Pasted image 20240206092731.png]]

If $A\rightarrow B$, we say $A$ **yields** $B$

For every $\alpha, \beta \in(V\cup \Sigma)^*$ where $\alpha$ is non-empty:
- $\alpha \implies \beta$ if $\alpha$ can be rewritten as $\beta$ by applying a **production rule**
-  $\alpha \implies^* \beta$ if $\alpha$ can be rewritten as $\beta$ by applying a finite number of **production rules** in succession

If multiple variables in sentence, which do you replace?
- In left-most derivation, replace the **left-most** variable

### Parse Trees

![[Pasted image 20240206093429.png]]

### Ambiguity
A grammar $G$ is ambiguous if it can generate the same string with **multiple parse trees**
$\equiv$
A grammar $G$ is ambiguous if the same string can be derived with two **left-most derivations**

(This is bad)

**Some** ambiguous grammar can be **rewritten** as an equivalent **unambiguous** grammar

![[Pasted image 20240206094242.png]]

**Not all** ambiguous grammars can be rewritten as an equivalent unambiguous grammar
- "Inherently ambiguous" grammars
### Chomsky hierarchy of Grammars

Multiple types of grammars:

![[Pasted image 20240206095102.png]]

![[Pasted image 20240206095048.png]]

Containment: Lower languages (Type 1, 2) are **subsets** of Higher languages (Type 3, 2..) 
### DFA to strictly right-linear grammars

![[Pasted image 20240210115117.png]]

For each variable, rule assigns:
- All strings that will take the machine **to some accept state** if it **starts** from that variable

![[Pasted image 20240210115243.png]]

However, since from $Y$ you cannot get to a final state, you can eliminate all strings with a $Y$ in them:

![[Pasted image 20240210115318.png]]
### DFAs to strictly left-linear grammars

![[Pasted image 20240210120153.png]]

For each variable, rule assigns:
- All strings that will take the machine **to** that variable **from the start state**

![[Pasted image 20240210120239.png]]

