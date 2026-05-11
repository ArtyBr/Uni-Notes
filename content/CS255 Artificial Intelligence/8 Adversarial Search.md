Adversarial search concerns **multi-agent** environments, where goals are in **conflict**
- Other agents, **opponents**, introduce **uncertainty**
- An *adversarial search agent* must deal with **contingencies** when searching for a solution
- The **complexity** of games is typically high and may be limited by **time**
	- Typically have to make **best guess** based on **experience** and **time available**
	
**Uncertainty** in **games** may arise from:
- **Opponent** trying to make the best **move** for **themselves**
- **Randomness** e.g. *dice*
- **Insufficient time** to determine exact **consequences** of actions

Key things for **exam** and **coursework**
- *Minimax, alpha-beta pruning, Imperfect decisions, MSTS*
## Perfect Decisions
**Formal view** of a game:
- **Initial state**
	- The board **position** and **player** to move
- A set of **operators** (or **successor function**)
	- Defines the **legal** **moves** and **resulting states**
- A **terminal test**
	- Determines when the game is over
- A **utility/payoff function**
	- Gives a **numeric value** for terminal states
We can build a **game tree** based on the initial state and operators for each player

$Max$ is the player (us) trying **maximise** our utility function
- Unlike, $Min$, which from our perspective tries to **minimise** our utility function
$Max$ must form a **strategy** that will win whatever $Min$ tries to do
- Strategy should include **correct move** for $Max$ for **each possible move** by $Min$

![[Pasted image 20231028153729.png]]

We start by considering **perfect decisions** - even though not always practical
### Minimax
Gives the **optimal strategy** for $Max$
**Idea**: Choose move with **highest** minimax **value**

**Minimax value** of a **state** is the **utility** of being in that state assuming both players play **optimally** from that state until the end of game
$Max$ prefers maximum values, $Min$ prefers minimal so:
- $MinimaxValue(n)=$$$\begin{cases}

Utility(n) & \text{if n is terminal}\\

max_{s\in Successors(n)}MinimaxValue(s) & \text{if n is a Max node}\\
  
min_{s\in Successors(n)}MinimaxValue(s) & \text{if n is a Min node}\\
 
\end{cases}$$
Minimax obtains **best** **achievable payoff** against **best play**
#### **Algorithm**:
- **Generate** the **complete game tree** 
- Use the **utility function** to rate the **terminal states** 
- Use the utility of **terminal states** to give the utility of **nodes one level up** 
- Continue **backing up tree** until the algorithm reaches the **root** 
- Max should choose the move that leads to **highest utility** — **minimax decision** 
	- Maximises utility under the assumption that the opponent will play to minimise it

![[Pasted image 20231101100639.png]]
#### **Analysis**
- **Complete?** - *Yes*, game tree is **finite**
- **Optimal?** - *Yes*, against an **optimal opponent**
- **Space?** - linear, $O(bd)$ - Performs DFS
- **Time** - $O(b^d)$ - Terrible for real games
$b= \text{branching factor}, d=\text{maximum depth}$

- We can extend minimax to multiple player (> 2) games by using vectors of utilities at each node in the tree 
- The backed up value of n is the utility vector of whichever successor is best for the player choosing at n
### Alpha-Beta Pruning
- Exploring the complete search tree is often **impractical**:
	- Alternative is to **prune** branches that will **not influence decision** 
- **Intuition**: Consider node $n$ that player **might** move to. If the player has a **better choice** $m$ either at the **parent** of $n$, or **further** up the tree, then $n$ will never be reached in **actual play** and we can **prune** it 
- As soon as we discover there is a **better choice** than $n$ (by looking at its descendants), we **prune** it

Minimax is a *depth-first search*, and alpha-beta pruning gets it name from the parameters backed up the path 
- α=value of best choice along the path for **Max** (highest value)
- β =value of best choice along the path for **Min** (lowest value) 

Alpha-beta search updates α and β as it searches, pruning as soon as the value of the current node is known to be worse than current α or β for Max or Min respectively 
- Pruning is done by terminating the recursive call

![[Pasted image 20231101101253.png]]
#### Analysis
The effectiveness of alpha-beta pruning is dependent on the order of examining successors 
- **Solution**: Try to examine the **best successors first** 
	- If we could do this, alpha-beta looks at O$(b^{\frac{d}{2}})$ nodes instead of $O(b^d)$ for minimax — roughly twice the lookahead 
- For **random** order successors alpha-beta looks at $O(b^{\frac{3d}{4}})$ nodes 
- In practice, a simple **ordering function** can give **significant advantage** 
	- E.g., for *chess*, consider captures, threats, forward moves, then backward moves.
## Imperfect Decisions
Alternative to searching either all or most of the tree - **not** finding a **terminal state**
**Cut** **off** tree **earlier**, using:
### A **heuristic evaluation function** to get a value for states
- Gives an **estimate of the expected utility** for a given position 
- Should:
	- **Order** terminal states as per **utility function**
	- **Approximate** the **actual utility** of a state

Most **evaluation functions** calculate **features** of a state
- E.g., for *chess*: number of pawns, rooks, knights, king safety etc.

These features define equivalence classes of states; each class will lead to a win, draw, or lose with some probability — we can evaluate the expected value of each class 
- We can combine features with a weighted linear function $w1f1 + w2f2 + ... + wnfn$ where $w$’s are weights and $f$ ’s features
	- Assume they are **independent**
### A **cutoff test** to determine **when** to stop going down the tree
- Turn **nonterminal** nodes **into** **terminal** leaves

- **Simplest approach** is to set a fixed depth: cutoff test succeeds at depth d 
- More **robust approach** is to use **iterative deepening**: continue until out of time, then return the best move found so far 
- Both are **unreliable** (due to approximation in evaluation function)

**Solution**: **Only** apply the **evaluation function** to **quiescent positions** those whose value is unlikely to change significantly in near future 
- **Nonquiescent** positions are expanded until quiescent positions reached 
- This extra search is called **quiescent search** 
- Quiescent search is **restricted** to certain types of move (e.g., capture in chess) to quickly **resolve uncertainties** in position 
- **Horizon problem**: When faced with an **unavoidable damaging** **move** from the opponent, a fixed-depth search is **fooled** into viewing **stalling** moves as **avoidance**

- Singular extension search as a means of avoiding horizon problem 
	- Singular extension = a move that is “clearly better” than all others 
	- E.g., in *chess*, can search to see whether opponent can advance pawn to 8th row, turning it into a queen 
- Forward pruning: immediately prune some moves from a node with no further consideration (e.g., people don’t consider all moves in chess) 
- Only safe in special cases: 
	- If two moves are symmetric or equivalent then only consider one of them 
	- Nodes very deep in search tree.
### Monte Carlo Tree Search
Alpha-beta tree search is **limited** by: 
- The **branching factor** (in practice a higher branching factor reduces the depth that can be searched), and 
- The difficulty of **defining** the **evaluation function** 

**Monte Carlo Tree Search** (MCTS) addresses these limitations 
The **idea** is to estimate the value of a state from the **average utility** over simulations (called playouts) of **complete games** starting from the state 
- A playout policy is used to determine which moves to make during a playout: 
- For some games (e.g., Go) we can learn from self-play using neural networks 
- For some games we can use game-specific heuristics (e.g., capture moves in chess)

**Pure** MCTS
- Do N simulations starting from the current state, and track which moves from the current position have the highest win percentage 
- As N increases this converges to optimal play 
In most games however, the computation cost of pure approach is too high: need a selection policy to focus search on important parts of game tree, i.e., we need to balance: 
- Exploration of states having few playouts 
- Exploitation of states having done well in past playouts to increase the accuracy of the estimate

MCTS does this by maintaining a search tree and growing it on each iteration using: 
- **Selection**: starting at the root, choose a move (using selection policy) leading to a successor, and repeat, moving to a leaf. 
- **Expansion**: grow the tree by generating new child of the selected node 
- **Simulation**: perform a playout from the newly generated child node (determine the outcome, but do not record these moves in the tree) 
- **Back**-propagation: use the result of this playout to update the search tree going up to the root 
**Repeat** these steps for a fixed number of iterations or until out of time, then return the move with the highest number of playouts (motivation is that 65/100 is better than 2/3 since the latter has a lot of uncertainty).

![[Pasted image 20231101103732.png]]

**UCT** - Upper confidence bounds **applied to trees**

![[Pasted image 20231101104313.png]]

Key thing - finding an appropriate value for $C$ 
- Required some tuning etc.

**Exam** - Could be a question on Monte carlo tree search
- Could be asked to apply a selection policy
- Don't need to remember the formula
- But need to understand what it does in principle
## Games with chance
Many games contain chance
- e.g., *backgammon* through the dice roll 
Legal moves are dependent on the roll of the dice, so cannot construct complete game tree 
- We have to include chance nodes 
- Chance nodes are labelled with result (e.g, dice value) and probability 
- We can calculate the expected value taken over the possible results of a chance node.

![[Pasted image 20231101102743.png]]

We can **generalise** minimax to use expected values:

![[Pasted image 20231101102807.png]]

**Expectiminimax** considers all outcomes of each chance node, so $O(b^{d}n^ d)$ where n is number of distinct outcomes 
- We can **prune** a chance node without looking at its children if we put bounds on the utility function — although a chance node will be an average, we know the bounds within which it lies



