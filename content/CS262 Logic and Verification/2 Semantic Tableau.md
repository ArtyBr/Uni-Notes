Proof technique 
- **Refutation** system 
	- Prove that a formula $X$ is a **tautology**, we begin with $\lnot X$ and produce a **contradiction**

Form of a **tree**:

![[Pasted image 20240123132732.png]]

- Each **branch** is a **conjunction** of the formulas on that branch
- The **tree** is a **disjunction** of all of its branches
- Disjunction of conjunctions - DNF

In each step, select a branch and a non-literal formula $N$ on that branch
- If N = ¬$\top$, then extend the branch by a node labelled ⊥ at its end
- If N = ¬⊥, then extend the branch by a node labelled $\top$ at its end
- If N = ¬¬Z, then extend the branch by a node labelled Z at its end
- If N is an α-formula, then extend the branch by two nodes labelled α1, α2 (α-expansion) at its end
- If N is a β-formula, then add a left and right child to the final node of the branch, and label one of them β1 and the other one β2 (β-expansion)

Contradiction if:
- In one branch (which is conjunction of its nodes), if there are both $X$ and $\lnot X$ - this branch is **closed** - there is a contradiction
- **Or** if $\bot$ appears in the branch

