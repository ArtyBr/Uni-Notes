Determining whether a problem is tractable or intractable is a difficult problem in itself
- We can **prove** a problem $X$ is a tractable by giving an **algorithm** that solves $X$
- Proving a problem is **intractable** is **harder**
	- This may require an extremely clever argument, or even a brand new technique depending on the problem
##### **Example**: Prove that `FIND-MAX` cannot be solved in **better** than linear time
**Proof**: We show that every integer must be visited at least once
- Suppose some algorithm $G$ exists that **does not** visit every element
- Consider the input $A=0, 0,...,0$ and suppose (wlog.) that element $a_k$ is **not** visited
- Then consider $A'$ where $a'=1$ and $a'_i=0$ for all $i\neq k$
- Then $G$ cannot distinguish between $A$ and $A'$, so it is incorrect for one or the other. 
- So $G$ does not solve `FIND-MAX` - no such algorithm exists
## Reductions
A **reduction** is a **description** of how to solve one problem by using a **solution** for **another** problem

When reducing from $X$ to $Y$, we are **not** allowed to **know** anything about the **specific solution** to $Y$ 
- We don't know anything about the implementation of $Y$ which would help us with $X$, only the result of applying $Y$ to an input
$Y$ acts like a **black box** which we can put **inputs** into and get **results** back. It is **not** a specific algorithm

We call this an **oracle** for $Y$
### Polynomial-time reduction
Problem $X$ **polynomial-time reduces** to problem $Y$ **iff** any instance of $X$ can be solved using:
- a **polynomial number** of **standard computational steps**, plus
- a **polynomial number**  of **calls** to an oracle of $Y$
(Instances of $Y$ must also be of polynomial size)

>[!note] Notation
>When $X$ is polynomial-time reducible to $Y$, we write $$X\leq_{p}Y$$ 
>We can write a solution for $X$ in terms of a solution for $Y$ with **at most** polynomial extra work
>- We sometimes say $X$ is **no harder** than $Y$
>- Or, **at most polynomially harder** than $Y$

Suppose we know $X\leq_{p}Y$ for some problems $X$ and $Y$.
1. If $X$ **cannot** be solved in polynomial time, then **neither can** $Y$
2. If $Y$ **can** be solved in polynomial time, then $X$ **can** as well
These are **contrapositives** to each other, so if **one** is true then both are (they are true)

**Relationship** between two problems in terms of tractability:
- **Theorem**: If $X\leq_{p}Y$ and $Y\in P$, then $X\in P$
- **Corollary**: If $X\leq_{p}Y$ and $X\notin P$, then $Y\notin P$

If $X\leq_{P}Y$ **and** $Y\leq_{p}X$ then we can say that $X$ and $Y$ are **polynomial-time equivalent** and write $X\equiv_{p}Y$

Algorithms give **upper bounds** on the **complexity** of a problem
- Reductions help us to **classify** problems according to **relative** difficulty - they provide upper bounds **in terms** of **other algorithms**
