# Events
Probability is typically defined in terms of some **experiment**. The **sample space** $\Omega$, of the experiment, is the set of *all possible outcomes* of the experiment

>[!note] Definition 2.1
>An event, $E$, is any **subset** of the **sample space**, $\Omega$

In general, the sample space may be **discrete**, meaning that the number of outcomes $|\Omega|$ if **finite** or **countable**

The sample space is **continuous** if the number of outcomes $|\Omega|$ is **uncountable**

>[!note] Definition 2.2
>If $E_{1}\cap E_2=\emptyset$, then $E_1$ and $E_2$ are **mutually exclusive**

>[!note] Definition 2.3
>If $E_{1}, E_{2}, ..., E_{n}$ are events such that $E_{i}\cap E_{j}=\emptyset, \forall i \neq j$, and such that $\bigcup^{n}_{i=1}E_{i}=F$, then we say that events $E_{1}, E_{2}, ..., E_{n}$ **partition** set $F$

# Probabilities
Given a sample space $\Omega$, we can talk about the **probability** of an event $E$, written $P\{E\}$. The probability of event $E$ is the probability that the outcome of the experiment *lies in the set* $E$

Probability on events is defined via the three **Probability Axioms**:

>[!note] Non-negativity
>$P\{E\} \ge 0$, for any event E

>[!note] Additivity
>If $E_{1}, E_{2}, E_{3}, ...$ is a countable sequence of events, with $E_{i}\cap E_{j}=\emptyset, \forall i \neq j$, then
>$$P\{E_{1}\cup E_{2}\cup E_{3}\cup ...\}=P\{E_{1}\}+P\{E_{2}\}+P\{E_{3}\}+...$$

>[!note] Normalization
>$P\{\Omega\}=1$

There are some more rules that can be useful when the events **overlap**:

![[Pasted image 20251012144555.png]]

>[!note] Lemma 2.5
>$P\{E\cup F\}=P\{E\}+P\{F\}-P\{E\cap F\}$

Following from Lemma 2.5, we have

>[!note] Lemma 2.6 (Union Bound)
>$P\{E\cup F\}\le P\{E\}+P\{F\}$

# Conditional probability on Events

>[!note] Definition 2.7
>The **conditional probability** of event $E$ *given* event $F$ is written as $P\{E|F\}$ and is given by the following, where we assume $P\{F\}>0$:
>$$P\{E|F\}=\dfrac{P\{E\cap F\}}{P\{F\}}$$

Further, we get the **Chain rule for conditioning**

>[!note] Theorem 2.10 Chain rule for conditioning
>Let $E_{1}, E_{2}, ..., E_{n}$ be events, where $P\{\bigcap^{n}_{i=1} E_{i}\}>0$. Then
>$$P\{\bigcap^{n}_{i=1}E_{i}\}=P\{E_{1}\}\cdot P\{E_{2}|E_{1}\}\cdot P\{E_{3}|E_{1}\cap E_{2}\}...$$
>$$=P\{E_{n}|\bigcap^{n-1}_{i=1}E_{i}\}$$

