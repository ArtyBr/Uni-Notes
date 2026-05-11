## Iterative Improvement Algorithms
In some problems the **path** is **irrelevant**; the goal state is the solution.
**Local search** uses a **single** **current** state (not multiple paths) and typically moves to **neighbours** of that state 
Not systematic, but:
- Low **memory** usage
- Can find reasonable **solutions** in **continuous** spaces
Useful for **optimisation problems** including CSPs - finding the best **state** according to some **objective function**

The state space - set of **complete configurations**
Goal - a **particular configuration** (or subset of configurations)
- e.g. traveling salesman problem: find *optimal* configuration
- ![[Pasted image 20231026082514.png]]
We use **iterative improvement** algorithms on such problems - keep the 'current' state, and try to **improve** it
- Often only require **constant space** (since only tracking **current state**)
- Suitable for **online** as well as **offline** search

Consider all states laid out on **surface** of a **landscape**
- **Height** of any point = **evaluation of state** at that point
- ![[Pasted image 20231026091021.png]]
- Move around the landscape trying to find highest peaks (or lowest if evaluation = cost)
### Hill Climbing
- Always tries to **improve** the **state** (Or reduce cost if evaluation is cost - called *greedy descent*)
- Each iteration, move in direction of **increasing** **value**
- There is no search tree, just **current state** and its **cost**
- If several alternatives have **equal value**, choose one at **random**

![[Pasted image 20231026091933.png]]

#### Problems with Hill Climbing
**Local maxima** - Local peaks (lower than highest peak) - the algorithm will halt without an optimal solution

![[Pasted image 20231026092205.png]]

**Ridges** - Ridges with steep sides (top with gentle slop) - the search will **oscillate** from side to side, making **no real progress**
- This problem occurs when step size of search is large as it skips over the maximum

![[Pasted image 20231026092254.png]]

**Plateau** - Flat areas, the search will conduct a random walk
May be a local maximum (no uphill above) or a **shoulder** (possible to progress)
- If reach a plateau, allow sideways moves to try and get off a shoulder
- Limit number of sideways moves - otherwise can conduct infinite walk on plateau
### Greedy descent - Local search for CSPs
Maintain an **assignment** of a **value** to each **variable**
Repeat:
- Select a variable to **change**
- Select a **new value** for that variable
- Until a satisfying assignment is found

**Aim**: Find an assignment with zero unsatisfied constraints
- Given an assignment of a value to each variable, a **conflict** is a **violated constraint**
- The **goal** is an assignment with **zero conflicts**
- Heuristic function to be **minimized**: number of conflicts

Several options for choosing a variable to change and a new value for it: 
- Find a **variable-value pair** that **minimizes** the number of conflicts 
- Select a **variable** that participates in the **most** conflicts 
- Select a **value** that **minimizes** the number of conflicts 
- Select a **variable** that appears in **any** conflict 
- Select a **value** that **minimizes** the number of conflicts 
- Select a **variable** at **random** 
- Select a **value** that **minimizes** the number of conflicts 
- Select a **variable** and value at **random**; accept this change if it does not increase the number of conflicts.
#### Complex domains
- When the domains are **small** or **unordered**, the **neighbours** of an assignment can correspond to choosing **another value** for one of the variables 
- When the domains are **large** and **ordered**, the neighbours of an assignment are the **adjacent** values for one of the variables (e.g., “aab” adjacent to “aaa”) 
- If the domains are **continuous**, gradient descent changes each variable proportionally to the gradient of the heuristic function in that direction The value of variable $Xi$ goes from $vi$ to $vi − η \frac{∂h}{∂Xi}$ where $η$ is the step size.
#### Randomized Greedy Descent
As well as downward steps we can allow for:
- **Random steps**: Move to a random neighbour
- **Random restart**: Reassign random values to all variables

Consider these 1-dimensional search spaces in which the search can step right or left:

![[Pasted image 20231026093637.png]]

- (a) Greedy descent with **random restart** should find optimal value **quickly**, and a **random walk** would **not** work well since many random steps are needed to escape local minima 
- (b) **Random restart** quickly gets **stuck** on a peak and does not work very well, but a **random walk** and greedy descent can **escape local minima**
### Stochastic search
Mix of:
- **Greedy descent**
- **Random walk**
- **Random restart**
#### Variants of random walk
Variants of random walk: 
- When choosing the best variable-value pair, randomly sometimes choose a random variable-value pair
- When selecting a variable then a value: 
	- Sometimes choose any variable that participates in the most conflicts 
	- Sometimes choose any variable that participates in any conflict 
	- Sometimes choose any variable 
- Sometimes choose the best value and sometimes choose a random value.
#### Runtime distribution
Solution: Plot the runtime (or number of steps) and the proportion (or number) of the runs that are solved within that runtime 
Which algorithm is best depends on how much time is available or how important it is to find a solution

![[Pasted image 20231026094141.png]]

### Simulated Annealing
- Pick a **variable** at **random** and a new **value** at **random**
- If it is an **improvement**, **adopt** it
- If it is **not** an improvement, adopt it probabilistically, depending on:
	- **Temperature parameter**, $T$, which can be reduced over time
	- With current assignment $n$ and proposed assignment $n'$ we move to $n'$ with probability $$e^\frac{h(n')-h(n)}{T}$$
- ![[Pasted image 20231026094634.png]]
- Change temperature based on how much progress you're making with currently use temp
- Often ran many times
#### Tabu lists
- To prevent **cycles**/**repeats** we can maintain a **tabu list** of the $k$ last assignments
- Do **not** allow an **assignment** that is **already in** the tabu list
- IF $k=1$ we do not allow an assignment of the same value as the variable chosen
- Can be **expensive** for **large values** of $k$
### Parallel search
More similar to Hill Climbing:
- We can refer to a **total assignment** as an **individual** 
- **Idea**: Maintain a population of k individuals instead of a single individual 
	- At every stage, update each individual in the population 
	- Whenever an individual is a solution, it can be reported 
	- Similar to $k$ restarts, but uses $k$ times the minimum number of steps.
### Beam search
Similar to **parallel search**, with $k$ **individuals**, but instead choose the k **best** out of all of the neighbours 
- When k =1, beam search is equivalent to *greedy descent* 
- When k =∞, beam search is equivalent to *breadth-first search* 
- The value of k lets us limit **space** and **parallelism**
	- i.e. how wide is the tree (if viewed as a tree)
#### Stochastic beam search
**Probabilistically** choose the $k$ individuals at the next generation
- The probability that a neighbour is chosen is **proportional** to its **heuristic value**, typically using $e^{-h(n)/{T}}$ (*Boltzmann/Gibbs distribution*)
- Maintains **diversity**
- Heuristic value reflects the **fitness** of the individual
### Genetic Algorithms
Related to stochastic beam search
With GAs, successor states are obtained from **two parents**
- Start with a population of $k$ **randomly generated** individuals i.e. states
- Each individual is represented as a **string** over a **finite alphabet**

![[Pasted image 20231028143527.png]]

- Each individual is evaluated by a **fitness** function
	- The **fitness function** is used to determine the probability of being chosen for **reproduction**
- **Pairs** of individuals are chosen according to these **probabilities**

For each pair, a **random** **crossover** point is chosen
- Offspring are **generated** by crossing over **parent strings** at chosen **crossover point**

Finally, each location in the newly created children is subject to a random **mutation** with small probability

![[Pasted image 20231028143829.png]]

- If two parents are **quite different**, crossover can produce a state that is very different from both parents
- GAs take **large steps early** in the search, **smaller steps** as population **converges**
The **primary advantage** of GAs is the ability to crossover **large blocks** that have evolved independently to perform useful functions

**Choice of representation** is **fundamental** - GAs work best if the representation corresponds to meaningful components of solution