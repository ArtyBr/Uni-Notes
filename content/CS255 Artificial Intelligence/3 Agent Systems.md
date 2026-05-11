An **agent system** is made up of an **agent** and an **environment**
- An agent receives **stimuli** from the environment
- An agent carries out **actions** in the environment
### Agent System Architecture
We can view an **agent** as comprising a **body** and a **controller**

![[Pasted image 20231011094253.png]]

- An agent **interacts** with the environment through its **body**
- The body is made up of
	- **Sensors** that interpret **stimuli**
	- **Actuators** that carry out **actions**
- The **controller** **receives** **percepts** from the body
- The **controller** **sends** **commands** to the body
- The body can also have reactions that are not directly controlled
## Implementing a controller
A **controller** is the brains of the agent
- Agents are situated in **time**: they receive sensory data in time, and do actions in time
- Controllers have **memory** and **computational abilities**
- The controller specified the command at each time
- The command at each time can depend on the **current** and **previous** percepts
### Agent functions
- Let $T$ be the set of **time points**
- **Percept trace** - Sequence of all past, present and future percepts received by the controller
- **Command trace**  - Sequence of all past, present and future commands output by the controller
- **Transduction** - function from percept traces to command traces - 
	- $f:$ [Percept trace] $\rightarrow$ [command trace]
	- A transduction is **casual** if the command trace up to time $t$ depends **only** on percepts up to $t$
The **Controller** is an implementation of a **causal transduction**
- An agent's **history** at time $t$ is a sequence of past and present percepts **and** past commands
- A causal transduction specifies a **function** from an **agent's history** at time $t$ into its **action** at time $t$
	- $f:$ [Agent's history] $\rightarrow$ [Action]
### Belief States
An agent does not always have access to its entire history, only what it has **remembered**
- The **memory** or **belief state** of an agent at time $t$ encodes all of the agent's history that it has access to
- The belief state encapsulates the information about its **past** that it can use for current and future actions
- At every point in time a controller has to **decide**:
	- What should it **do**?
	- What should it **remember**?
	- .. As a **function** of its **percepts** and its **memory**

![[Pasted image 20231011094310.png]]

For a discrete time, a controller implements:
- **Belief state function** - `remember(belief_state, percept)` returns the next belief state
- **Command function** - `do(memory, percept)` returns the command for the agent

Abstractly, we view the intelligent agent as thinking like this:

![[Pasted image 20231011094532.png]]

However implementing it like this would be too **slow** if we were to feed each output into the next
## Hierarchical Control
### Overview
A better architecture (than the percept $\rightarrow$ reasoning $\rightarrow$ action system) is a **hierarchy of controllers**
- Each controller sees the controllers below it as a *virtual body* from which it gets percepts and sends commands
- The lower-level controllers can:
	- Run much **faster** and react to the world more quickly
	- Deliver a **simpler** view of the world to the higher level controllers

![[Pasted image 20231011095055.png]]

###### The following functions will be implemented at each level to communicate the outputs

![[Pasted image 20231011095409.png]]

- **Memory** function - `remember(memory, percept, command`
- **Command** function - `do(memory, percept, command)`
- **Percept function** - `higher_percept(memory, percept, command)`
#### What should be in an Agent's Belief State?
An agent decides what to do based on its **belief state** and what it **observes**
- A purely **reactive** agent does **not** have a belief state
- A **dead reckoning** agent does not perceive the world
- It is often useful for the agent's belief state to be a **model** of the **world** (i.e. agent itself and the environment)
#### Agent Functions
We can view an agent as being specified by the **agent function** mapping percept sequences to actions
- $f: P \rightarrow A$

The **ideal rational agent**: does whatever action is expected to maximise performance measure on basis of percept sequence and built-in knowledge
- In principle there is an **ideal mapping** of percept sequences of actions corresponding to ideal agent

The simple approach to this is a **lookup table**. However, this is doomed to **failure** because:
- **Size** of table (too large)
- **Time** to build (too long)
- Agent has no **autonomy**

But the lookup table suggests a notional "*ideal mapping*"
- One agent function exists that is **rational** (approximates ideal mapping)
- Our aim: **Find** a way to **implement** the **rational agent function**
- Implementation must:
	- Be relatively **efficient**
	- Exhibit **autonomy** (if required)
	- Get as **close** as possible to the **ideal mapping**
### Agent types
Broadly speaking, there are five fundamental **types** of agent i.e. ways of building the agent function, with **increasing generality**
#### Simple reflex agents
 - Based on a set of **condition-action** rules
	- We can view these rules as **summarising** the notional **lookup table**
	- Although simple, reflex agents **can** still achieve relatively **complex** behaviour
	- We can also add the learning of new rules etc.

	 ![[Pasted image 20231011124833.png]]
#### Reflex agents with state (Model-based)
 - **Retain knowledge** about the world
	- Need some **internal state** to track the environment
	- **Update state** function typically uses knowledge about how the world evolves and how actions affect the world
	- Agent can use such information to track **unseen** parts of the world

	![[Pasted image 20231011125228.png]]
#### Goal-based agents
- Have **representation** of what states are **desirable**
	- Knowing the **current environment** state is **not** generally enough to choose action: need know **what** the agent is trying to achieve
	- Can combine information about **environment**, **goals** and **effects** of actions to choose what to do
	- Relatively simple if goal is achievable in single action
	- Typically goal achievement needs a **sequence** of actions - use search or planning
	- A goal-based agent is much more **flexible** than a reflex agent
	![[Pasted image 20231011130722.png]]
#### Utility-based agents
Ability to discern some **useful measure** between possible means of achieving some state
	- 
#### Learning agents
Able to **modify their behaviour** based on their **performance**, or in the light of **new information**