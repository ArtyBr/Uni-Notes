Lots of probability recap..

Discrete Random Variables, Continuous Random Variables
Joint and Conditional Probabilities
- Probability of a sensor measurement given a state
- Or probability of a new state, given a previous state and some sensor reading
Law of Total Probability
- We can find the probability of an event by adding the probabilities of that event with other events if we know all the exhaustive cases
Marginal and Total Probability
Bayes' Theorem

![[Pasted image 20260203152155.png]]

Marginal likelihood

![[Pasted image 20260203152531.png]]

Recursive State Estimation
- We would like to update the state estimation for each time $t$

![[Pasted image 20260203153031.png]]

The **ultimate goal** is an estimate of the state at time $t$, the belief or **posterior** of the state, $Bel(x)$

![[Pasted image 20260203153100.png]]

- $x \rightarrow$ **states**
- $u \rightarrow$ **actions**
- $z \rightarrow$ **observations**


We can use **bayes rule** to express this in terms of the observation/**sensor model** $P(z|x)$, the **prior** $P(x)$ and the **normalisation constant**

![[Pasted image 20260203153221.png]]

The **prior $P(x)$** is just $x_t$ **before** our measurement

Assuming **sensor independence**, we can say that *if* we have the state at that times step, *then* the measurement is **independent** of everything else

![[Pasted image 20260203153328.png]]

We can **then** use the **law of total probability** to rewrite the second part as the probability  of going from $x_{t-1}$ to $x_{t}$ multiplied by the probability of $x_{t-1}$ summed/integrated over **all** $x_{t-1}$'s

![[Pasted image 20260203153435.png]]

But, we can **assume** (*'markov assumption'*) that $x_t$ is **only** impacted by the **previous state** $x_{t-1}$ and the **perturbation** $u_{t}$ so that we can simplify further

![[Pasted image 20260203153539.png]]

![[Pasted image 20260203153731.png]]

We use **two steps**:

- **Prediction**
	- ![[Pasted image 20260203153802.png]]
	- Take the belief at time $t-1$ and apply the dynamics. Integrate or sum that over all the possible $x$ at $t-1$ to get the predictions before measurement
- **Measurement/update (correction)**
	- ![[Pasted image 20260203153833.png]]
	- Multiply that by the likelihood of the measurement to get the new belief (which needs to be normalised)

To implement that, we need to:
- Specify the observation and motion models, based on knowledge of the dynamics of the system
- Decide how to **represent** the belief - need to be able to carry out the integration or restrict ourselves to finite state spaces so that the integral becomes a finite sum

There are different realisations with different properties:

![[Pasted image 20260203154050.png]]

We need to **choose** how to **approximate**, *trade-off* between:
- **Computational efficiency**
- **Accuracy**
- **Ease of implementation**

---

**Discrete filter**

We want the integral to be tractable - to find a **closed form** solution
- At least **both** $P(z_t|x_t)$ and $P(x_{t}|u_{t},x_{t-1})$ need to be tractable

We can replace the integral with a **sum**
- Conceptually, just **enumerate** all possible cases

There is a **more elegant** way of doing it:

Say we have $N$ states:
- We can represent $P(z_t|x_t)$ and $P(x_{t}|u_{t},x_{t-1})$ by $N$ by $N$ matrices
- Belief propagation can be done with matrix multiplication

No need to do the summation
- Normalise the state vector each time!

With a toy example, we have a robot in some room:

![[Pasted image 20260203154609.png]]

![[Pasted image 20260203154508.png]]

Assume the robot tells us the following from a sequence of sensor observations:

![[Pasted image 20260203154534.png]]

![[Pasted image 20260203154454.png]]

![[Pasted image 20260203154621.png]]

![[Pasted image 20260203154633.png]]

![[Pasted image 20260203154929.png]]

---

**Discrete probability function**

![[Pasted image 20260203154955.png]]

![[Pasted image 20260203155136.png]]

