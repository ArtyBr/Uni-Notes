A CSP is characterized by:
- A set of variables $V_{1,}V_{2}, ..., V_{n}$
- Each variable $V_{i}$ has an associated **domain** $D_{Vi}$ of **possible values**
- There are **hard constraints** on various subsets of the variables which specify **legal combinations** of **values** for these variables
- A solution to the CSP is an **assignment** of a **value** to each variable that **satisfies** **all** of the **constraints**
CSP as optimization problems:
- For optimization problems there is a **function** that gives a **cost** for each assignment of a value to each variable
- A **solution** is an assignment of values to the variables that **minimises** the **cost function**
### Types of CSP
#### Discrete variables
- **Finite** domains: $n$ variables of domain size $d \rightarrow O(d^{n})$ complete assignments
	- e.g. Boolean CSPs, incl. Boolean satisfiability (NP-complete)
- **Infinite** domains (*integers, strings, etc*.)
	- We cannot enumerate all possible assignments: Need a **constraint language**
		- e.g. $Seminar_{a}>Lecture_{a}$
	- Solvable with **linear** constraints, **nonlinear** constraints undecidable
#### Continuous variables
e.g. time
- **Domain** is **continuous**, e.g. schedule for Hubble Telescope observations
- Linear constraints **solvable** in **polynomial time** by **linear programming** methods
### Types of Constraints
- **Unary** constraints - Involve a single variable - $X\neq green$
- **Binary constraints** - Involve pairs of variables - $SA \neq WA$
- **Higher-order** constraints - Involve 3 or more variables
- **Preferences** or **soft** constraints - e.g. 10.05 is better than 08.05 can be represented by a cost for each variable assignment $\rightarrow$ Constrained optimization problems

## Algorithms
### Generate-and-Test algorithm
"*Just keep guessing until you stumble on the right answer*"
**Generate** the assignment space $D=D_{V1}\times D_{V2} \times ... \times ... D_{Vn}$ - the set of **total** assignments
- **Test** each assignment with the **constraints**
- ![[Pasted image 20231020090129.png]]
- $D$ has $d^{n}$ elements, so need to test $d^{n}$ assignments
### Backtracking algorithms
Systematically **explore** $D$ by **instantiating** the variables **one** at a time
**Evaluate** each constraint predicate as soon as all its variables are **bound**
Any **partial** assignment that does **not** satisfy the constraint can be **pruned**
- e.g. (for scheduling activities) $A=1 \land B=1$ is inconsistent with the constraint $A \neq B$ regardless of the value of other variables

Every solution appears at depth $n$, so we can use **depth-first search**
- The path is **irrelevant**, so we can use **complete-state** formulation
- Branching factor $b=(n-l)d$ at depth $l$, hence $n!*d^n$ leaves
	- Top level branching factor is $nd$ since any of $d$ values can be assigned to any of $n$ variables; next level branching factor is $(n-1)d$ and so on
- Variable assignments are **commutative**
	- $$[AI = 10.05, Algorithms = 11.05] = [Algorithms = 11.05, AI = 10.05]$$
- Backtracking search is the basic **uninformed** algorithm of CSPs

![[Pasted image 20231020093828.png]]
#### Improving search efficiency
**General-purpose** methods can give huge gains in speed
- What variable should be assigned next? *MRV* and *degree heuristic*
- In what order should its values be tried? *LCV*
- Can we detect **inevitable failure** early? *Consistency algorithms*
- Can we take advantage of problem structure? *Cutset conditioning* and *variable elimination*
To answer these questions we can avoid the need to use domain specific knowledge
#### Search heuristics
**Minimum remaining values (MRV)** - choose the variable with the fewest legal values (aka fail-first heuristic) - variable that is most likely to make the search fail

**Degree heuristic** - Tie-breaker among MRV variables
- Choose the variable with the most constraints on remaining variables
- Attempts to reduce branching factor of future choices

**Least Constraining Value (LCV)** - Given a variable, choose the least constraining value: the one that rules out the **fewest** values in the remaining variables
### CSP as Graph Searching
A CSP can be solved by a **graph search**
- A **node** is an **assignment** of values to some of the variables
	- Suppose node $N$ is the assignment $X_1 = v_1,...,X_k = v_k$. Select a variable $Y$ that is not assigned in $N$. For each value $y_i ∈ dom(Y) X_1 =v_1,...,X_k = v_k,Y = y_i$ is a neighbour of $N$ if it is consistent with the constraints
- The start node is the **empty** assignment
- A goal node is a **total assignment** that **satisfies** the constraints
### Constraint Network
- Node for each **variable** (often shown as a circle)
- Node for each **constraint** (often shown as a rectangle)
- There is a **domain** of values associated with each variable node
- There is an arc $<X, c>$ from variable $X$ to each constraint $c$ that involves $X$
### Consistency algorithms
**Idea**: Prune the domains as much as possible **before** selecting values from them
- If constraint $c$ has scope {$X$} ($X$ is the only variable in scope) then arc $<X, c>$ is **domain consistent** if every value of $X$ satisfies $c$
- More generally, if constraint $c$ has scope {$X, Y_{1},..., Y_{k}$}, then arc $<X, c>$ is **arc consistent** if for each $x\in D_{X}$ there are values $y_{1}, ..., y_{k}$ where $y_{i}\in D_{Y}$ such that $$c(X=x, Y_{1}=y_{1}, ..., Y_{k}=y_{k})$$ is satisfied.
#### Arc consistency
An arc $<X,r(X,\overline{Y})>$ is an arc from $X$ to a *relation* of $X$ to any number of $Y$ variables
- An arc $<X,r(X,\overline{Y})>$ is **arc consistent** if, for each value $x ∈ dom(X)$, there is **some value** $\overline{y} ∈dom(\overline{Y})$ such that $r(x,\overline{y})$ is **satisfied** 
- A **network** is arc consistent if **all** its arcs are arc consistent 
- What should we do if arc $<X,r(X,\overline{Y})>$ is **not** arc consistent? 
- All values of $X$ in $dom(X)$ for which there is **no corresponding value** in $dom(Y)$ can be **deleted** from $dom(X)$ to make the arc $<X,r(X,Y)>$ **consistent**.

**Algorithm:**
- The arcs can be considered **in turn** making **each** arc **consistent** 
- When an arc has been made arc consistent, does it ever need to be checked again? 
- **Yes**: an arc $X,r(X,\overline{Y})$ needs to be **revisited** if the **domain** of one of the $Y$’s is **reduced** 
- There are three possible outcomes when all arcs are made arc consistent: (Is there a solution?) 
	- **One** domain is **empty** =⇒ **no** solution 
	- **Each** domain has a **single** **value** (*highly unlikely*) =⇒ **unique** solution 
	- **Some** domains have **more than one value** (*This is what usually happens*) =⇒ there **may or may not** be a solution (and so we need to search/solve)
###### Example

![[Pasted image 20231025102510.png]]

##### Finding solutions when Arc Consistency finishes
- If some domains have **more than one** element =⇒ **search** 
- We can also **split** a domain, then **recursively** solve **each half** =⇒ **domain splitting** or **case analysis** 
	- The idea is to split a problem into a number of **disjoint cases** and solve each case **separately** 
	- The **set** of **all solutions** to the initial problem is the **union** of the solutions to **each case** 
- It is often **best** to split a domain in **half** 
- Do we need to restart AC from **scratch**? **No**, just need to consider arcs that are possibly **no longer arc consistent** as a **result of the split** (i.e., if $X$ has domain split, start with arcs of form $⟨Y,r⟩$ where $X$ appears in $r$ and $Y$ is not $X$).
###### Example

![[Pasted image 20231025102707.png]]
#### Hard and Soft constrains
Given a set of variables, assign a value to each variable that either 
- **Satisfies** some **set of constraints**: satisfiability problems — **hard constraints** 
- **Minimizes** some **cost function**, where each assignment of values to variables has some cost: **optimization** problems — **soft constraints** 
Many problems are a mix of hard and soft constraints (called constrained optimization problems).
#### Problem structure
- Suppose that each subproblem has $c$ variables out of $n$ total variables 
- There are $n/c$ subproblems each of which takes at most dc to solve, and worst-case solution cost is therefore $n/c · d^c$, i.e., linear in n 
	- E.g., if n = 80, d = 2, c = 20 then 
	- $2^{80}$ = **4 billion years** at 10 million nodes/sec 
	- $4 · 2^{20}$ = **0.4 seconds** at 10 million nodes/sec.
### Tree-Structured CSPs
- **Theorem**: if the constraint graph has no loops, the CSP can be solved in $O(nd^2)$ time 
- Compare to general CSPs, where worst-case time is $O(d^n)$ 
	- (This property also applies to logical and probabilistic reasoning: an important example of the relation between syntactic restrictions and the complexity of reasoning.)

1. Choose a variable as root, order variables from root to leaves such that every node’s parent precedes it in the ordering 2 For $j$ from $n$ down to
 ![[Pasted image 20231025103455.png]]
2. **Remove** inconsistent** domain elements for $⟨Parent(X_j),X_j⟩$ 
	 - (At this point the CSP is directionally arc consistent, so no backtracking needed in next step. The reverse order checks ensure deleted values do not endanger consistency of processed arcs. Runs in $O(nd^2))$ 
3. For $j$ from 1 to $n$, assign $X_j$ consistently with Parent($X_j$).
### Nearly tree-structured CSPs
- Conditioning: instantiate a variable, prune its neighbours’ domains, i.e., assign variable so remainder is a tree. 
	- ![[Pasted image 20231025103706.png]]
- **Cutset conditioning**: instantiate (in all ways) a set of variables such that the remaining constraint graph is a tree 
- **Cutset size** c =⇒ runtime $O(d^c ·(n −c)d^2)$, i.e., very fast for small c
### Variable elimination
- **Idea**: eliminate the variables one-by-one, passing their constraints to their neighbours 
**Variable Elimination Algorithm**: 
- If there is **only one variable**, return the **intersection** of the (unary) constraints that contain it 
- Otherwise, **select** a variable X 
	- **Join** the constraints in which X appears, forming constraint R1 
		- Think of it in same way as a database join
	- **Project** R1 onto its variables other than X, forming R2 
	- **Replace** all of the constraints in which X appears by R2 
	- **Recursively solve** the simplified problem, forming R3 (etc.) 
	- **Return** R1 joined with R3

- When there is a **single** variable remaining
	- If it has no values, the network was **inconsistent** (no solution 
- The variables are eliminated according to some elimination **ordering** 
- Different elimination orderings result in different size intermediate constraints
###### Example
**Initial Network**:
![[Pasted image 20231025104421.png]]

**Network Made Arc-Consistent**:
![[Pasted image 20231025104448.png]]

![[Pasted image 20231025104509.png]]

![[Pasted image 20231025104517.png]]

![[Pasted image 20231025104525.png]]