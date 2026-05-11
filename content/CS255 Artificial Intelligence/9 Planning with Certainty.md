## Knowledge Bases
**Idea**: Build an AI system that has **2 separate** **layers** to it:

![[Pasted image 20231102193325.png]]

A **knowledge base** is a **database** of **facts/beliefs**
- These facts/beliefs are true from the **perspective** of the **agent**
- Formally: 
	- **Knowledge base** = set of **sentences** in a formal **knowledge representation language**
- This is **specific** to the environment of the agent
An **inference engine** is a **mechanism** for **reasoning** about those beliefs
- **Independent** of the **world/domain** - inferences are made using the same logic everywhere

Allows us to take a **declarative** approach to building an agent
- We can `TELL` **it** what it needs to know
- The agent can then `ASK` **itself** what to do
	- Answers should follow from the **KB** through **inference**
	- The result of `ASK` **must follow** from **previous** `TELL`s as determined by the **inference mechanism**
Both terms are **internal** to the agent
#### Advantages
Can reason using **inference** and their **knowledge**, offering several advantages:
- Accepting **new** tasks in the form of **goals**
- **Adapting** to **environmental** change by updating knowledge
- Inferring **unseen** properties of the world from their **perceptions**

A knowledge-based approach **excels** in finding better solutions compared to **simple** search methods. These agents exhibit remarkable **flexibility** in various scenarios:
- **Adopting new goals**
- Handling **partially observable environments**
- Adapting to **dynamic environments**
#### Characterising in 3 levels
Knowledge-based agents can be characterized on three levels:
1. **Knowledge Level** - What is known, regardless of its implementation; allows us to work at an abstract level using **Ask** and **Tell** — i.e., we can take a declarative approach.
2. **Logical Level** - Knowledge encoded in formal sentences.
3. **Implementation Level** - Data structures (strings, arrays, linked lists, etc.) in the knowledge base (KB) and algorithms that manipulate them.
## Simple Knowledge-Based Agent
A **simple knowledge-based agent** must possess the following capabilities:
1. **Represent states, actions, etc.**
2. **Incorporate new percepts**
3. **Update internal representations of the world**
4. **Deduce hidden properties of the world**
5. **Deduce appropriate actions**
### Simple Knowledge-Based Agent Function
- The **knowledge base** (KB) is used to store information.
- On each iteration, the agent tells the knowledge base about its **perceptions** and asks what actions to perform.
- The representation details are hidden by **Make-Percept-Sentence** and **Make-Action-Query**, allowing us to work at the knowledge level.
- Inference details (logical level) are hidden in **Tell** and **Ask**.

![[Pasted image 20231102195926.png]]
## Logic
- **Logics** are formal **languages** for representing information such that conclusions can be drawn
- **Syntax** defines the **sentences** in the language 
- **Semantics** define the “**meaning**” of sentences 
	- i.e., define the truth of a sentence in a world
#### Entailment
**Syntax**: $KB$ |= $\alpha$
- Knowledge base $KB$ **entails** sentence $a$ iff $a$ is **true** in **all** worlds where $KB$ is **true**

Entailment is important since it provides a strong way of showing that if **certain** propositions are true, then some **other** proposition must be true
- At a **representation** level - **Sentences** entail **sentences**
- At the **world** level - **Facts** follow **facts**
- **Semantics** give a **mapping** of **sentences** to **facts**

Logical inference generates sentences that are entailed by existing sentences and should **ensure** relationship **mirrored** in **real world**
#### Inference
**Syntax**: $KB ⊢_{i}\alpha$  
- Sentence $\alpha$ can be **derived** from $KB$ by **procedure** $i$

- **Soundness** - $i$ is sound if whenever $KB ⊢_{i}\alpha$ it is **also true** that $KB$ |= $\alpha$ 
- **Completeness** - $i$ is complete if whenever $KB$ |= $\alpha$ it is **also true** that $KB ⊢_{i}\alpha$
### Propositional Logic
Simplest logic, illustrates the basic ideas
- **Proposition symbols** $P_{1}, P_2$ etc. are **sentences**
- **Negation** - If $S$ is a sentence, $¬S$ is a sentence
- **Conjunction** - $S_{1}$ and $S_2$ is a sentence, $S_{1}∧ S_2$
- **Disjunction** - if S1 and S2 is a sentence, $S_1 ∨ S_2$ is a sentence 
- **Implication** - if S1 and S2 is a sentence,$S_1 ⇒ S_2$ is a sentence 
- **Equivalence** - if S1 and S2 is a sentence, $S_1 ⇔ S_2$ is a sentence

Given a **model**, we can identify true/false values for each propositional symbol

**Inference** - Enumeration method
- Let $\alpha=A\land B$ and $KB=(A\lor C)\land(B\lor \lnot C)$
- Does $KB$  |= $\alpha$
	- Does the knowledge base **entail** our query sentence?
- Check all possible models - $\alpha$ must be **true** wherever $KB$ is **true**

![[Pasted image 20231103082536.png]]

Yes - wherever $KB$ is true, $alpha$ is also true
#### Representation
Represent **percepts** as **sentences**, and put them in the knowledge base

Example with *Wumpus World*
- The knowledge base contains sentences of the form $S_{2,1}$ representing stench in in $[2, 1]$
- The agent is given some **knowledge** about the world
- For example, if stench in $[2,1]$ then wumpus in a neighbouring square (or $[2,1]$) i.e., $$rule_n : S_{2,1} \implies W_{3,1} ∨W_{2,2} ∨W_{1,1} ∨W_{2,1}$$
- The agent can then mechanically draw **conclusions** with standard **inference**
- Also give the agent **action rules**
	- e.g. $$rule_{m}:A_{1,1}\land East_{A}\land W_{2,1} \implies \lnot Forward$$
	- $A$ - denotes Agent
- The agent can then `ASK` its knowledge base "Should I move forward?"
#### Problems with propositional logic
- **Scalability** - Too many propositions
	- “Don’t go forward if a wumpus in front of you” requires **64 rules** (16 squares × 4 orientations)
	- If world is larger, problems scales exponentially
- **Dynamism** - The world may **change**, so we need a **different symbol** for each time step 
	- e.g. $A_{1,1}^{0},A_{1,2}^{1}$ etc.
*Planning* offers a solution
## Planning
Planning systems prevent **uncontrollable branching** when presented with large possibility of **actions**
### **Planning systems**:
- Open up the **action**, **state** and **goal** representations to allow **selection** - represent in **first-order logic**
	- **States** and **goals** = sets of sentences
	- **Actions** = descriptions of preconditions and effects
This allows the planner to make **direct connections** between **states** and **actions**

*Divide and conquer* by **subgoaling**
- A planner can consider **several smaller** problems, and then **combine solutions**
- Works because there is **little** interaction **between** subplans
- Planning relaces the requirement for **sequential construction** of solutions
- Allows planner to add actions **where needed** so it can make *obvious* or *important* decisions to reduce branching factor
#### **Search vs Planning**:

![[Pasted image 20231103084802.png]]
#### Simple Planning Agent

![[Pasted image 20231103084853.png]]

**Overview**:
- **Update** the knowledge base
- If not **already executing** a plan, **generate a goal**, and **construct a plan** to achieve it
- The agent must be able to **cope** if the goal is **infeasible** or **achieved**
- Once the agent **has a plan**, it will **execute** to **completion**
- There is **minimal interaction** with the environment: **perceive** to determine **initial state**, but then just execute plan
	- No relevance checks
## Situation Calculus
Way of describing **change** in **first-order logic**
- The world is viewed as a sequence of **situations** 
	- **Snapshots** of the **state** of the world
- Situations are generated by **previous** situations by **actions**
- **Functions** and **predicates** that **change** (called **fluents**) with time are given a **situation argument** 
	- e.g., At$(Agent,[1,1],S_0)$; those that do not change (called eternal or atemporal), are not given an argument, e.g., Wall(0,1) 
- Change is represented by a function `Result(action,situation)` which denotes the result of performing action in situation

Preconditions of actions:
- **Possibility** axioms — Describe when it is **possible** to **execute** an action 
	- Possibility axioms have the form: $Precondition =⇒ Poss(a,s)$
	- E.g., $At(Agent,x,s) ∧ Adjacent(x,y) ⇒ Poss(Go(x,y),s)$ 
- **Effect** axioms — Describe the **changes** due to an action 
	- Effect axioms have the form: $Poss(a,s) =⇒ changes$
		- (i.e., when executing the action, if then action is possible then the effects happen) 
	- E.g., $Poss(Go(x,y),s) =⇒ At(Agent,y,Result(Go(x,y),s))$
### Planning
Planning can be seen as a **logical inference** problem using **situation calculus** 
- Logical sentences are used to describe the initial state, goal, and operators 
**Initial state**: a sentence about a situation S0 
- E.g.$$At(Home,S0) ∧ ¬Have(Milk,S0) ∧ ¬Have(Bananas,S0)... $$
**Goal state**: a logical query for suitable situations 
- E.g. $$∃s · At(Home,s) ∧ Have(Milk,s) ∧ Have(Bananas,s)..$$
###### Example

![[Pasted image 20231103095001.png]]

#### Frame Axioms
So, we also need to describe how the world stays the same 
**Frame axioms** — capture the non-changes due to an action,
- e.g., $At(o,x,s) ∧ (o\neq Agent)∧¬Holding(o,s) ⇒ At(o,x,Result(Go(y,z),s))$
If there are F **fluents** and A **actions**, we require O(AF) frame axioms 

The **Frame Problem** is a long standing problem in AI: 
- **Representational**: proliferation of frame axioms (original frame problem) 
	- representational problem now largely solved 
- **Inferential**: having to carry properties through inference steps, even if they remain unchanged 
	- inferential problem avoided by planning; we do not address it for inference systems
#### Successor-state Axioms
Successor-state axioms solve the **representational frame problem** 
Each axiom is about a predicate (not an action per se): 
- **General form**: P true afterwards ≡ (an action made P true ∨ P true already and no action made P false) 
- We need a successor-state axiom for each predicate that can **change** over time 
- The axiom must list **all ways** the predicate can become true or false

Theoretically these axioms are all that is required, but they're **impractical**

To make planning **practical** we must use a **restricted language**
- **Reduces** possible solutions to search through
- Actions represented in in a restricted language allows the creation of **efficient** planning algorithms
### Strips
Most planners use the `STRIPS` language (or extensions to it)
- **States** - Conjunctions of **function-free ground literals**
	- i.e. predicates applied to constants (possibly negated)
	- State descriptions may be **incomplete**
- **Closed-world** assumption: Most planners assume that if the state description does **not** mention a positive literal, can assume it to be false
- **Goals** - Conjunctions of **literals**, may contain variables
- **Planner** - Asks for a **sequence** of actions that make the goal true if executes
#### Strips operators
Operators comprise of three components:
- `OP`: **Action**
	- e.g. $Buy(x)$
- `PRECOND`: **Precondition**
	- e.g. $At(p), Sells(p, x)$
	- Conjunctions of **positive** literals
	- Preconditions **implicitly refer** to the situation immediately **before** the action
- `EFFECT`: **Effect**
	- e.g. $Have(x)$
	- Conjunctions of **function free literals**
		- i.e. both positive and negative
	- Effects **implicitly refer** to the **result** of the action

There is no **explicit** situation information 

![[Pasted image 20231108101306.png]]

- An operator with **variables** is an **operator schema** — a family of actions requiring instantiation 
- An operator is **applicable** in state $s$ if we can **instantiate** each variable so that the **precondition** is **true** in $s$

Standard search: node = concrete world state 
	→search space of states
Planning search: node = partial plan 
	→search space of plans 
**Definition**: An **open condition** is a precondition not yet fulfilled 
- Operators on partial plans: 
	- **Add** a **link** from an existing action to an open condition 
	- **Add** a **step** to fulfil an open condition
	- **Order** one **step** with respect to another 
- **Gradually** move from **incomplete**/vague plans to **complete**, correct plans

**Terminology:**
- **Progression**: start from the initial situation and search forward to the goal  
	- **High branching** factor and search space
- **Regression**: Search **backward** from the goal to the **initial situation** 
	- **Reduces** **branching factor**, but complicated by conjunctions in goals and ensuring all conjunctions achieved
- A **partial plan** is an **incomplete** plan, with some steps **not instantiated** 
- **Partial order**: **Some** steps are **ordered** with respect to others 
- **Total order**: **All** steps are **ordered**, i.e., a list of steps
## Partially Ordered Plans
Principle of **least commitment** — leave choices as long as possible 
- Try to **minimise** breaking **causal links**
A **plan** comprises of: 
- A set of **steps** (corresponding to the operators) 
- A set of **ordering constraints** on steps, e.g., $S_i < S_j$ ($S_i$ before $S_j$) 
- A set of **variable bindings** 
- A set of **causal links** 
	- e.g., $S_i \rightarrow^c S_j$ ($Si$ achieves $c$ for $Sj$) 
Once all variables are bound we have a fully instantiated plan 
- A plan is **complete** iff:
	- *every precondition is achieved* 
- A precondition is **achieved** iff: 
	- *it is the effect of an earlier step and no possibly intervening step undoes it* 
- A plan is **consistent** iff:
	- *there are no contradictions in the ordering or binding constraints*

A **problem** is **defined** by a **partial plan** containing just **start** and **finish** steps
- The **initial state** is the **effect** of **start** 
- The **goal** is the **precondition** of **finish**
- Ordering constraints are **added** as **arrows** between **actions** 
	- (assuming that we use the graphical Strips representation)

POP is a **regression planner** to search through **plan space** 
- Each iteration we **add a step** (to achieve preconditions), backtrack if inconsistent 
- Only add steps to **achieve** a precondition that has **not yet** been achieved 
- Each iteration we establish **causal links** — **without breaking** other links
	- i.e., links are protected 

POP is **sound**, **complete**, and **systematic** (no repetition)
#### POP Algorithm
- Start with a minimal partial plan
- Each iteration find a step to achieve a precondition $c$ of a step $S_{need}$
- Do this by choosing an operator $S_{chosen}$ to achieve the precondition 
- Record a causal link, $S_{chosen} \rightarrow^c S_{need}$, to the newly achieved precondition 
- Resolve any threats to causal links 
- If we fail to find an operator or to resolve a threat to a causal link then backtrack.

![[Pasted image 20231108102840.png]]

![[Pasted image 20231108102848.png]]

![[Pasted image 20231108102854.png]]
#### Clobbering
A **threat**, or **clobberer**, is a potentially **intervening** step that **destroys** the condition achieved by a casual link

![[Pasted image 20231108103157.png]]

**Demotion**: Put before $GO(HWS)$
**Promotion**: Put after $Buy(Drill)$

The key point is that causal links are **protected links**
- Protect them by ensuring that **threats** - steps that might delete (clobber) the link — are ordered before or after (demoted or promoted)

Blocks world example:
![[Pasted image 20231108103810.png]]

- The planner goes through multiple cycles for each step that is shown
- **Dotted line** is an **ordering constraint** - representing clobbering
	- Need to do steps in order of dotted lines, since otherwise constrained by previous actions (on path of dotted line)
- z matches start state of value, so value is as table
- Each solid line represents one iteration through the algorithm

![[Pasted image 20231108104015.png]]

We can solve these conflicts by **demoting** $s_1$ and **promoting** $s_2$

![[Pasted image 20231108104132.png]]

Therefore, after dotted lines: 
- Complete $s1$ and $s3$ **before** s2 due to clobbering
### Problems for `STRIPS` and POP

![[Pasted image 20231108104249.png]]

![[Pasted image 20231108104258.png]]

![[Pasted image 20231108104308.png]]
## Hierarchical decomposition

![[Pasted image 20231108104510.png]]

We introduce **abstract operators** that can be **decomposed** into the steps that implement them
- These decompositions are predetermined and stored in a library of plans - works best when there are several possible decompositions

A plan $p$ **correctly implements** a nonprimitive operator $o$ if it is a complete and consistent plan for achieving the effects of $o$ given the preconditions of $o$
- $p$ must be **consistent**
- Each effect of o must be asserted by a step of p (and not denied by a later step) 
- Each precondition of the steps in p must be achieved by a previous step or be a precondition of o 
Guarantees that a nonprimitive operator can be replaced by its decomposition in the plan 
- But, we still need to check threats when introducing new steps from the decomposition

![[Pasted image 20231108104707.png]]

Each iteration: Try to add a step and resolve a nonprimitive
**Solution**: Must check that **all** operators are primitive

![[Pasted image 20231108104751.png]]
#### Broadening operator descriptions
![[Pasted image 20231108104820.png]]

![[Pasted image 20231108104950.png]]

![[Pasted image 20231108105048.png]]
#### Resource Constraints
![[Pasted image 20231108105210.png]]

![[Pasted image 20231108105218.png]]
#### Temporal Constraints
![[Pasted image 20231108105359.png]]
## The real world
