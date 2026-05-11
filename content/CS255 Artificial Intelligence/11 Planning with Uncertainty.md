Logicism was the earliest approach for knowledge representation (1960s and 1970s) 
Logicism has its **problems**: 
- It **cannot** (easily) cope with **uncertainty** 
- The real-world does not always have logical implications
	- e.g., no symptom or prognosis is logically implied by the existence of a disease 
- MYCIN used ‘Certainty Factors’ to try and avoid these issues 
	- However, such approaches suffer from a lack of sound theoretical backing
# Probability
Probability is the process of **reasoning** about experiments that have a set of **distinct outcomes**
- e.g.,: 
	- Drawing the top card from a deck 
		- The outcome is the face of the card (52 possible outcomes) 
	- Picking a person from a population and determining whether they are a smoker or not 
		- Two possible outcomes Smoker, Non-Smoker
- We call an experiment **well-defined** once the set of outcomes has been **identified** 
- Definitions: 
	- A measure of **chance** 
	- The **proportion** of cases in which an event occurs 
	- A measure of **belief** in the proposition
#### **Where** do probability numbers come from?
- The **frequentistic** view: 
	- From **repeatable**, **identical**, experiments 
	- The **relative frequency** of each **outcome** approaches a **limit** 
		- For example, a *coin toss* or *dice roll* 
- The **objectivist** view: 
	- Propensities of objects to **behave** in certain ways 
	- Can use the frequentist approach to calculating probabilities 
- The **subjectivist** view: 
	- **Degree of belief** rather than any physical significance 
	- “In my opinion” the probability of getting a tooth cavity is 0.1 i.e., 1 in 10 people get a cavity 
	- **May not translate** to numbers achieved in **experiments** 
	- **May not find** **agreement** between different people
### Probability Space
- The **Sample Space**, $Ω$
	- Defines the **finite set of possible outcomes** $s_1,s_2, ...,s_n$, 
		- i.e., the states of the world 
	- Outcomes in $Ω$ must be **mutually exclusive** and **exhaustive** (**atomic** events) 
	- A **probability measure**, $(Ω, P)$, is obtained by assigning a real number $P(s) ∈ [0, 1]$ to each state $s_i ∈ Ω$ such that $\Sigma_{s_{i}\in\Omega} P(si) = 1$
	- This is also known as a **finite probability space** 
- An **Event** $E$ is a **set of outcomes**, $E ⊆ Ω$
	- $Φ$ is the **impossible** event, $P(Φ) = 0$
	- $Ω$ is the **certain** event, $P(Ω) = 1$ 
	- For an event $E ⊆ Ω: P(E) = \Sigma_{s_{i}\in E} P(s_i)$
### Random Variables
Given a probability space, $(Ω, P)$, a **random variable**, $X$, is a **function** on $Ω$ 
- Each element in $Ω$ is assigned a **unique value** 
- The set of **possible values** $X$ can assume is called the **domain** of $X$ 
- A **random variable** with a **finite domain** is known as a **discrete random variable** 
- We can use random variables to construct **propositions** 
	- A **primitive proposition** is either an **assignment** to a *variable*, a **comparison** between a *variable* and a *value*, or a **comparison** between a *variable* and a *variable*
	- A **proposition** can be constructed from primitive propositions using **logical connectives**
		- e.g., $X = Heads ∧ X \neq Y$
- We can express the **probability** of a proposition, α, in relation to each possible state, $s_i$, of $Ω$: 
	- $P(α) = \Sigma_{s_{i} \in \Omega :\alpha =\text{True in } s_{i}}  P(si)$

