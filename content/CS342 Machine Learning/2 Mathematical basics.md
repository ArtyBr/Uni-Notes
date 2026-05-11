# Statistics
## Probabilities
**Probability** of of event A:
- $p(A)$ = Area(A) / Total Area
	- Or *count* of A in probability space/total count of events in probability space
- $p(\lnot A) = 1-p(A)$

**Joint probability**: Probability that events $A$ *and* $B$ happen:
- $p(A, B)$
- *Intersection* of $Area(A)$ and $Area(B)$ divided by total area

**Marginalisation rule**:
- $p(A) = \Sigma_xp(A,B=x)$
	- **Joint sum** over all values of *one variable* gives probability of the *other variable*
- $p(A) = p(A, B) + p(A, \lnot B))$

**Random variable**: 
A *function* of a **random event**
A variable whose value *depends* on a **probability**
- $event (D1=x)$ depends on random variable $D1$
- Common to use $p(x)$ to *denote* $p(D1=x)$ when random variable $D1$ is obvious
- The **sum** of the probabilities of a random variable over the **entire domain** is 1:
$$\sum_xp(D=x)=1$$
- e.g. for a dice, outcome of the event $D$ = rolling could be 1, 2, 3, 4, 5 or 6
$$\sum_xp(D=x)=1/6 + 1/6 + ... + 1/6 = 1$$
**Conditional probability, Bayes Rule**
Probability that $A$ will happen *given* $B$ happens
- $p(A|B) = \dfrac{p(A, B)}{p(B)}$
- Conditional probability $p(A|B)$ sums to 1 over all $A$:
$$\sum_xp(x|B)=1$$
Bayes rule allows 'reversing' conditional probabilities $$p(A|B) = \dfrac{p(B|A)p(A)}{p(B)}$$
## Distributions
*Samples of data* form a **distribution**
- A **distribution** is a **function** that shows the **possible values** for a variable and **how often** they occur

![[Pasted image 20250524092055.png]]

The **Sample Distribution** *converges* to the mathematical distribution as the number of samples converges to $\infty$  

The **normal distribution** has interesting parameters:
- **Mean** or **expectation** $\mu$
 $$\mu=\dfrac{1}{n}\sum_{i=1}^{n}x_i$$
- **Standard deviation**, $\sigma$
- **Variance**, $\sigma^2$
$$\sigma^2=\dfrac{1}{n} \sum^n_{i=1} (x_i-\mu)^2$$

The **probability density function (PDF)** calculates the probability of *observing* a **given value**
- The PDF of a normal distribution:
$$p(x|\mu, \sigma^2)=\dfrac{1}{\sqrt{2\pi\sigma^2}}e^{-\dfrac{(x-\mu)^2}{2\sigma^2}}$$
The **cumulative density function** (CDF) of a normal distribution calculates the probability of an observation being *equal* or *less than* a value

![[Pasted image 20250524092942.png]]

To find the probability of a variable occurring between two values $x1$ and $x2$, find CDF($x2$) - CDF($x1$) 

## Expected values
If we have a **random variable** $X$ that can take values $x\in X$, we define the **expectation** of $X$, $\mathbb{E}[X]$ by:
$$\mathbb{E}[X]=\sum_{x\in X}p(X=x)x$$
- Example: rolling a dice, we have $p(X=x)=\dfrac{1}{6}$ for all $x=1,2,3,4,5,6$
$$\mathbb{E}[X]=\sum^6_{x=1}p(X=x)x=\sum^6_{x=1}\dfrac{x}{6}=3.5$$
Generalisation of the notion of an *average*:
- If probabilities are uniform, expectation is the average over values that $X$ can take
- Expectation can also give the 'average' value we expect to see if probabilities are non-uniform
	- If we flip a coin that lands heads $(x=1)$ $75\%$ of the time and lands tails $(x=0)$ $25\%$ of the time, then the expectation is:
	- $\mathbb{E}[X] = 0.75(1)+0.25(0)$
- The expectation is a **weighted average** of the *possible values* the random variable can take
# Matrices and Vectors

![[Pasted image 20250524095451.png]]

**Important matrices**:

![[Pasted image 20250524095609.png]]

**Key operations on matrices/vectors**

![[Pasted image 20250524095722.png]]

![[Pasted image 20250524095737.png]]

Why the inverse? Can't divide by a matrix
- How to find $X$ if we know $A$ and $B$?

![[Pasted image 20250524095906.png]]

# Big O
Big O notation describes the **limiting behaviour** of a function when the argument tends towards a particular value or infinity
- $g(n) = O(f(n))$ means for all large $n$, $g(n) < cf(n)$ for some constant $c>0$

# Min and Max functions
$min$ and $max$ functions return the **minimum or maximum** **value** achieved by a function:
- $min_w\{(w-1)^2\}=0$
- $max_w\{cos(w)\}=1$

A *related* set of functions are the $argmin$ and $argmax$ functions
- Return the *set* of parameter values that *achieve* the minimum or maximum value:
- $argmin_w\{(w-1)^2\}=1$
- $argmax_w\{cos(w)\} = 0, 2\pi, 4\pi, ...$