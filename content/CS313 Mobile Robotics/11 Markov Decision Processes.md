A deterministic approach might be:
1. Generate the map
2. Search the map, compute the optimal path
3. Command the robot to follow the optimal path

In this case there might be some issues, for example:
- Robot may not respond to your command
- May perform some random motion
- Map may be wrong
- Movements that we command may not be executed correctly by the robot
	- Path execution is inherently uncertain - commands are performed with a probability less than 1
	- Sometimes, the optimal path may not be the optimal way to follow

So, in a **mark decision process**:
We are **given**:
- A set of **states** $S$ - robot locations
- Set of **actions** $A$ - possible movements
- **Transition model** $T(s, a, s')$ - uncertainty in robot motion
	- We use a **markov property**:
		- The **probability** of transitioning from $s$ to $s'$ **depends** **only** on the **current state** $s$
			- Not on the history of earlier states
- **Reward** function $R(s)$ - How much do I **gain** by reaching state $s$
	- May be positive or negative, but has to be **bounded**
		- Won't have indefinite reward
	- Can be represented as a function $R(s, a, s')$
		- The reward of transitioning from state $s$ to $s'$ under action $a$
- **Discount factor** $\gamma$

**Evaluate**:
- Policy $\pi$ that **maximises** the future expected **reward**

We use a process of **value iteration**
Basic idea is:
- Calculate the **utility** of each state 
	- (overall benefit of being at a state $s$)
- Use the state utilities to select an **optimal action** in each state

Now the question is, what is the overall benefit of being at a state $s$
- The utility

![[Pasted image 20260309141903.png]]

- We say that we have a starting $s$ and follow a policy $\pi$. This gives us a reward.
- For each point, when we apply this, the reward will be updated accordingly.
- The discount factor is applied each time too

Using $U^{\pi}(s)$, we can define the optimal policy $\pi^*$ as the one that chooses the action $a$ that maximises the **expected utility** of $s$:

![[Pasted image 20260309142029.png]]

Because the next state is **uncertain**, we compute the expectation using the **transition probabilities**:

![[Pasted image 20260309142056.png]]

Combining the utility recursion with the optimal policy gives the **Bellman equation**:

![[Pasted image 20260309142248.png]]

This equation says that the **value** of a state equals the **resward received** now plus the **best possible expected value** of the **next state**

- If there are $n$ possible states, then there are $n$ Bellman equations (for each state)
- To compute the $n$ utiltiies, we would like to solve simultaneously the $n$ Bellman equations
	- Problematic because $max$ is not a linear operator
- Use value iteration applying Bellman update:
	- ![[Pasted image 20260309142402.png]]
- Assume $\gamma=1$
- Start with the utilities of all states initialised to 0
- Guaranteed to converge

All wrapped up int his algorithm:

![[Pasted image 20260309143131.png]]

An **optimal policy** can be found as you progress
- At a given iteration, the optimal policy at state $s$ is determined by:
	- ![[Pasted image 20260309143200.png]]

The **key difference** between **state reward** and **utility**:
- **Short-term** vs **long-term**
- **Concrete** vs **expected**

