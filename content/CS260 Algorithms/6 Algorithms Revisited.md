**Class test** - 
1 Question by Graham (Wks 1-5)
1 Question by Alex (Wks 6-8)

#### **What is an algorithm?**
Day one:
- "*A procedure used to solve a problem or perform a computation*"
Now:
- "*An algorithm is a well-specified* **set of instructions** *that, when followed, provide the* **answer** *to a* **given question**"
No computers required!

![[Pasted image 20231106141930.png]]

- Each algorithm **must** solve **every instance** of the problem
- **Some** algorithms may be **optimal**
- Algorithms can have **multiple implementations**

Implementation details do not need to be included in the **proof** or **description** of an algorithm

One implementation detail:
- *How big is a number?*
Doesn't matter for the time being, since this is part of the implementation and not the algorithm
#### What does an algorithm look like?
A **sequence** of **steps**, where:
- Each step is **simple enough** to be **carried out** by a **person**
- Each step is **well-defined**
- Each step is **unambiguous**
#### What is an algorithm for?
- To get the **answer**
- To **prove** that an answer **can be found**
- To **prove** that an answer can be found **efficiently**
	- i.e. using a *reasonable amount* of *time* and *space*
- To **prove** a **relationship** between **multiple problems**
#### What is a proof?
A **convincing argument** that **claim** is **true**
- Proofs exist to convince a human *reader*, not a *computer*

When **writing** algorithms, we provide proofs of:
- **Correctness** - to convince the reader that our algorithm provides the **answer** we claim it does
- **Efficiency** - to convince the reader that our algorithm runs within the **time/space** bounds that we claim it does
##### Simple Example
**Problem**: Given a nonempty sequence of integers $A=a_{1}...a_{n}$, find the **largest element** in $A$
```
largest = 0
for element in A
	if element > largest:
		largest = element
return largest
```
**Proof of correctness**: By induction on $n$
**Proof of efficiency**: Algorithm performs exactly $n-1$ comparisons and up to $n$ assignments, so its running time is $\Theta(n)$. Space usage is **constant**.
#### Relationships between algorithms
Often, problems admit algorithms that can be written in terms of **other** algorithms
- **Fact**: An algorithm `FIND-MAX` exists, taking linear time and $O(1)$ space
- **Problem**: Given an nonempty sequence of integers $A=a_1...a_n$, find the **smallest** element in $A$
	- **Algorithm**: Create a sequence $B=b_1...b_n$ such that $b_i=-a_i$ 
	- Let $m$ be the result of running `FIND-MAX` on $B$ 
	- Return $m$
### Complexity
We **classify** problems according to how difficult they are
- The **complexity** of a problem is the **time**/**space** efficiency of the **best** algorithm that solves it
- Not every problems complexity is known
#### P
A problem can be solved in **polynomial time** if there exists an algorithm that solves it in time $O(n^k)$ for some constant $k$
- All problems that can be solved in polynomial time are said to be in the **complexity class** $P$
- $P$ is also called $PTIME$
- All constant time algorithms, linear time algorithms, quadratic algorithms etc. are all in $P$
We care **more** about **time** than space

A problem is **tractable** if it can be solved **in practice**, using a **real computer**, in a **reasonable** amount of **time**
