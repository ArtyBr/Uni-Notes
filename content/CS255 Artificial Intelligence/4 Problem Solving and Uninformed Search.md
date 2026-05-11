Often we are not given an algorithm to solve a problem, but only a specification of what is a solution — we have to **search** for a solution 
- A typical problem is when the agent is in **one state**, it has a set of deterministic actions it can carry out which change its state, and wants to get to a **goal state** 
- Many AI problems can be abstracted into the problem of finding a path in a **directed graph** of states
- Often there is **more than one way** to represent a problem as a **graph**

A **problem-solving agent** is a goal-based agent that will determine sequences of actions that lead to **desirable states**
### Problem-Solving
**Problem solving** is the process of finding a sequence of actions that would result in you achieving a **desirable state**

There are **four steps** a problem-solving agent must take:
- **Goal formulation** - Identify its goal given the current situation
- **Problem formulation** - Identify permissible actions (or *operators*), and states to consider
- **Search** - Find sequence of actions to achieve goal
- **Execution** - Perform the actions in the solution

There are **two types** of problem-solving:
- **Offline** - Complete knowledge of problem and solution
- **Online** - Acting without complete knowledge of problem and solution

###### Algorithm for a simple problem-solving agent
```ruby
function Simple_Problem_Solving_Agent(p) 
	returns action 
	
inputs : p, a percept 

variables : 
	s, an action sequence, initially empty 
	state, a description of the current world state 
	g, a goal, initially null 
	problem, a problem formulation 
	
state ← Update_State(state,p) 
if s is empty then 
	g ← Formulate_Goal(state) 
	problem ← Formulate_Problem(state,p) 
	s ← Search(problem) 
action ← Recommendation(s,state) 
s ← Remainder(s,state) 
return action
```
###### Dimensions of (typical) State-space Search

![[Pasted image 20231011102019.png]]
#### Problem Types
- **Deterministic**, **fully** observable - **Single-state** problem
	- Sensors tell agent the current state and it knows exactly what its actions do
	- **Knows** exactly what state it will be in after any action
- **Deterministic**, **partially** observable - **Multiple-state** problem
	- Limited access to state, but knows what its actions do
	- Can determine **set** of states resulting from action
	- Instead of single states, agent must manipulate **sets** of states
- **Stochastic**, **partially** observable - **Contingency** problem
	- Don't know current state, and don't know what state will result from action
	- Must use **sensors** during **execution**
	- Solution is a **tree** with branches for contingencies
	- Often **interleave** search and execution
- **Unknown** state space, i.e. knowledge is learned - **Exploration** problem (*online*)
	- Agent doesn't know what its actions will do
#### Problem Formulation: State-space problems
A **state-space problem** consists of:
- A set of **states**
- A subset of states called the **start states**
- A set of **actions**
- An **action function**: Given a state and an action, returns a **new state**
- A set of **goal states**, specified as goal test function, `goal(s)`
- A criterion that specifies the **quality** of an acceptable solution
#### Selecting a State Space (Abstraction)
The real world can be extremely complex - state space may need to be **abstracted** for problem solving
- **Abstract** state ~ Set of **real** states
- **Abstract** operators ~ Combination of of **real** actions
- **Abstract** solution ~ Set of real paths that are solutions in **real** world
#### State Space Graphs
A general formulation of a problem solving task is a **state space graph**:
- A (directed) **graph** comprising of a set $N$ of **nodes** and a set $A$ of ordered pairs of nodes, called **arcs** and **edges**
- Node $N_2$ is a **neighbour** of $n_1$ if there is an arc from $n_1$ to $n_2$. That is, if $<n_1, n_2> \in A$
### Tree Search Algorithms (general)
**Tree search** is the basic approach to problem solving
- Offline, simulated exploration of the state space
- Starting with a start state, **expand** one of the explored states by generating its successors to build (*find neighbours by considering possible actions*) a search tree
#### Implementing Tree Search
- A **state** represents a physical configuration
- A **node** is a data structure comprising part of the search tree:
	- **State**
	- **Parent**
	- **Children**
	- **Depth**
	- **Path Cost**
- Nodes waiting to be expanded are called the **frontier**
- Represent the frontier as a **queue**

![[Pasted image 20231011111923.png]]

#### Search Strategies in general
Strategies are defined by the **order of node expansion**
Evaluated along several **dimensions**:
- **Completeness** - Does it always find a solution (if it exists)?
- **Optimality** - Does it always find a least-cost solution?
- **Time-complexity**: Number of nodes expanded, usually expressed in terms of
	- Maximum **branching factor** $b$
	- **Depth** of least cost solution $d$
	- Maximum **depth** of **state space** $m$ (may also be infinite)
- **Space complexity** - Maximum number of nodes in memory (expressed in terms of $b, d$ and $m$)
### Uninformed Tree Search Strategies
Uninformed search only uses information from the **problem definition** - no measure of the best node to expand
#### Breadth-First - Expand shallowest unexpanded node
`QueueingFunction` = put successors at **end** of queue (*queue*) (FIFO)
- **Complete?**
	- Yes - always finds best solution if $b$ is finite
- **Time?**
	- $1 + b + b^2 + b^3 +... + b^d = O(b^d)$
- **Space?**
	- Each leaf node kept in memory: $O(b^d-1)$ explored and $O(b^d)$ in frontier, so complexity is $O(b^d)$ i.e. dominated by size of frontier
- **Optimal?**
	- Only if **non-decreasing** function of depth; **not** optimal in general
- **Space** is the **main problem** in Breadth-First
#### Depth-First - Expand deepest unexpanded node
`QueueingFunction` = insert successors at **front** of queue (*stack*) (LIFO)
- **Complete?**
	- No. Fails in **infinite-depth** spaces or spaces with **loops**
- **Time?**
	- $O(b^m)$ **terrible** if $m$ much larger than $d$. However, if solutions are dense then may be much faster than breadth-first search
- **Space?**
	- $O(bm)$ i.e. **linear** space
- **Optimal?**
	- No
### Graph Search
**Graph search** is a practical way of exploring the state space that can account for repetitions
- Given:
	- A graph
	- Start nodes
	- Goal nodes,
- **Incrementally explore paths** from the start nodes
- Maintain a **frontier** of paths from the start node that have **been explored**
- As search proceeds the frontier expands into the **unexplored nodes** until a **goal node** is encountered
- The way in which the frontier is expanded defines the **search strategy**
```js
Input: 
graph
set of start nodes
boolean procedure goal(n) that tests if n is a goal node

frontier -> {<s>:s is a start note}
while frontier is not empty:
	select and remove path <n0, ..., nk> from frontier
	if goal(nk)
		return <n0, ..., nk>
	for every neighbour n to nk
		add <n0, ..., nk, n> to frontier
end while
```
- Which **value** is selected from the frontier at each stage **defines** the **search strategy**
- The **neighbours** define the **graph**
- `goal()` defines what is a **solution**
- If more than one answer is required, the search can continue from the return call
#### Breadth-first Graph Search
- Treats the frontier as a **queue**
- It always selects one of the **earliest** elements added to the frontier
- If the list of paths on the frontier is $[p_1, ..., p_r]$
	- $p_1$ is selected and its neighbours are added to the end of the queue, after $p_r$
	- $p_2$ is selected next

![[Pasted image 20231012231633.png]]

Order of nodes at the **end of paths** as selected from the frontier
##### Complexity
- If the branching factor for all nodes is finite, breadth-first graph search is guaranteed to find a solution if one exists - it is guaranteed to find a path with fewest arcs
- Time complexity is exponential in the path length: $b^n$, where $b$ is branching, $n$ is path length
- The space complexity is exponential in path length: $b^n$
- The search is unconstrained by the goal (i.e. the strategy does not favour paths which seem more promising).
#### Depth-first Graph Search
- Depth-first graph search treats the frontier as a **stack**
- It always selects one of the **last** elements added to the frontier
- If the list of paths on the frontier is $[p_1, ..., p_r]$
	- $p_1$ is selected and the paths that extend $p_1$ are added to the front of the stack (in front of $p_2$)
	- $p_2$ is only selected when all paths from $p_1$ have been explored
	
![[Pasted image 20231012233325.png]]

##### Complexity
- **Not** guaranteed to halt on **infinite** graphs or graphs with **cycles**
- Space complexity **linear** in the number of arcs from the **start** of the **current node**
- If the graph is a finite tree with forward branching factor $\leq b$ and all paths from the start having at most $k$ arcs, worst case is $O(b^k)$
- Search is **unconstrained** by the goal
#### Lowest-cost-first Graph Search
Orders paths on the frontier based on the **cost** of the **whole path**

Sometimes there are costs associated with arcs
The **cost** of a **path** is the **sum** of the costs of its arcs
$$cost(<n_0, ..., n_k>)=\sum^k_{i=1}cost(<n_{i-1}, n_i>)$$
An **optimal solution** is one with **minimum** cost
- At each stage, **lowest-cost-first** search selects a path on the frontier with lowest cost
- The frontier is a **priority queue** ordered by **path cost**
- The **first** path to a goal is the **least-cost** path to a goal node
- When arc costs are equal => breadth-first search

