# 1

![[Pasted image 20231207150804.png]]

Q-Learning:
- Store $Q[s,a]$ for all $s, a$
- Update them via a temporal-difference learning method until they converge to the actual value
	- Every step, you receive a new reward
	- Use that reward/difference to update the TD method

The facts that the agent believes are true, based on previous percepts and conclusions from them
- Q-array, the estimates for $Q(s,a)$ for all $s, a$
- 

![[Pasted image 20231207150958.png]]

Things that the agent can sense
- The reward $r$
- The new state $s'$
- Percept $p_t$

![[Pasted image 20231207151007.png]]

$\epsilon$ greedy strat:
- $\epsilon$ - random action
- $1-\epsilon$ - Take argmax $Q(s,a)$ 

![[Pasted image 20231207151622.png]]

- $Q(s,a)\leftarrow (1-\alpha)Q(s,a)+\alpha (r+max\gamma Q(s', a')$

# 2

![[Pasted image 20231207152454.png]]

Subtract some baseline value/gradient and find where the max is
# 3

![[Pasted image 20231207152742.png]]

Deterministic option: $\epsilon$-greedy
Stochastic option: Softmax
- Take an action with a probability
Initialise Q-values to encourage exploation
"Optimism in the face of uncertainty"
# 4

![[Pasted image 20231207153340.png]]

![[Pasted image 20231207153352.png]]

Add a distance-based reward, or form of state
yes would be helpful
# 5

![[Pasted image 20231207153645.png]]

Just one update:
$Q(s=34,a=7)\leftarrow (1-\alpha)Q(34,7)+\alpha (3+\gamma max(65, a'))$

# 6

![[Pasted image 20231207154022.png]]

$Q(s,a)=0, \forall (s,a)$
$(S_{17},right, 0, s_{18})$
$(S_{18},up,10,s_{14})$
$(S_{14},right,-4,S_{15})$
1 - $Q(S_{17},right)\leftarrow (1-alpha)Q(S_{17},right)+\alpha (0 + \gamma maxQ(S_{18},a'))$
2 - 
$Q(S_{18},up)\leftarrow (1-alpha)Q(S_{18},up)+\alpha (10+\gamma maxQ(S_{18},a'))$
3 - 
$Q(S_{14},right)\leftarrow 0.9Q(S_{14},right)+0.1\times(-4+\gamma maxQ(S_{15},a'))$

