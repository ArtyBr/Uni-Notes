#### Inputs:
- **Abilities/actions** - What the agent can do
- **Goals/preferences** - What it wants, desires, usually in an order of preference
- **Prior Knowledge** - Info the agent has been given beforehand
- **History of Stimuli** - 
	- **Current** stimuli - What it perceives from the environment **now**
	- **Past experiences** - What it has perceived in the past

**Goals** can be specified by some performance measure, usually signified by a numerical value
A **Rational action** is one which maximises the expected value of the performance measure **given the percept sequence (history) to date**
### Dimensions of Complexity
We can view the design space for AI as being defined by a set of dimensions of complexity:
#### 1 Modularity
How many **modules** the agent has in terms of abstraction:
- Single - **Flat** - one layer of abstraction
- Separate, interacting modules - **Modular**
- Modules that are (recursively) decomposed into more modules - **Hierarchical**
**Flat** description is either continuous **or** discrete, **hierarchical** is often a hybrid of both continuous **and** discrete
#### 2 Planning Horizon
How far the agent has to **plan** for in the **future**
- **Static** (non-planning) - World does not change, agent just considers the present environment
- **Finite** stage - Agent reasons about a fixed number of time steps
- **Indefinite** stage - Agent reasons about a finite, but not predetermined number of time steps
- **Infinite** stage - Agent plans on going forever
#### 3 Representation
How the environment/world is represented to the agent
- **Explicit** states - A state the world could be in
- **Features** or **propositions** - States are described using general features, often binary (e.g. Is there a human in sight?)
- **Individuals** and **relations** - There is a feature for each relationship on each tuple of individuals
#### 4 Computational Limits
How reasonable it is to compute the best action based on **knowledge**, **memory** and **computation** limits
- **Perfect** rationality - Best course of action can be determined within (limited) computer resources
- **Bounded** rationality - Agent must make a good decision based on its perceptual, computational and memory limitations
#### 5 Learning
Whether the model is fully specified to the agent
- Knowledge is **given**
- Knowledge is **learned from data or past experience**
Usually a mix of both
##### Uncertainty
- **No** uncertainty - The agent knows what is true
- **Disjunctive** uncertainty - Set of states that are possible
- **Probabilistic** uncertainty - Probability distribution over the worlds
#### 6 Sensing Uncertainty
Whether an agent can determine the state from its stimuli
- **Fully** observable - Agent can observe the entire state of the world
- **Partially** observable - Can be a number of states that are possible given the agent's stimuli
#### 7 Effect Uncertainty
Can the agent predict what will happen after the action it takes?
- **Deterministic** - Resulting state is fully determined from the action and previous state
- **Stochastic** - There is uncertainty about the resulting state
#### 8 Goals/Preferences
What is the agent trying to achieve/what are the motivations?
- **Achievement goal** - A goal to achieve, can be a complex logical formula
- **Complex preferences** - May involve trade offs between various desired goals, perhaps at different times
	- **Ordinal** - only the order matters
	- **Cardinal** - absolute values also matter
#### 9 Number of Agents
Are there multiple reasoning agents needed to take account of?
- **Single agent** reasoning - Any other agents are part of the environment
- **Multiple agent** reasoning - An agent reasons strategically about the reasoning of other agents
Agent goals can be competitive, cooperative or independent of each other
#### 10 Interaction
When does the agent reason?
- **Offline** - Before acting
- **Online** - While interacting with environment

### Task is represented in a simpler/abstracted way to the agent

![[Pasted image 20231006100759.png]]

Representation should be:
- **Rich enough** to express knowledge needed to solve problem
- As **close to the problem** as possible
- Amenable to **efficient computation**
- Able to be acquired from people, data, past experiences

#### Quality of solutions
Different classes of solution:
- **Optimal** solution - **Best solution** according to some measure of quality
- **Satisficing** solution - **Good enough** according to some description of which solutions are adequate
- **Approximately optimal** solution - Measure of quality is **close to the best** theoretically possible
- **Probable** solution - **Likely** to be a solution
#### Decisions and outcomes
- Good decisions can have bad outcomes and vice versa
- **Information** can be **valuable** as it leads to better decisions, this can be quantified sometimes
- We can often trade off computation time and solution quality
	- An **anytime algorithm** can provide a solution at any time; given more time it can provide better solutions
###### Solution quality and computation time

![[Pasted image 20231006101414.png]]

We need to represent a problem in order to solve it on a computer
Many levels of abstraction for representation:

![[Pasted image 20231006101526.png]]

### Physical symbol system hypothesis
- A **symbol** is a meaningful physical pattern that can be manipulated
- A **symbol system** creates, copies, modifies and destroys symbols
**Physical symbol system hypothesis:**
- A physical symbol system has the necessary and sufficient means for general intelligent action

Two levels of abstraction are common in entities:
- **Knowledge** level is in terms of what an agent **knows** and what its **goals** are
	- About the **external world** to the agent
- **Symbol** level is in terms of what **reasoning** it is doing
	- About what symbols and agent uses to **implement** the knowledge level
