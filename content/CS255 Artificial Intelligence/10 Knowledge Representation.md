**Thinking** is the process of:
- **Retrieving** what is **relevant** in our **knowledge store** to our present situation
- **Reasoning** with that knowledge to make decisions
### Knowledge
**Knowledge** can be viewed as a **relation** defines by the **propositional attitude** between the **knower** and a **proposition**
- John **knows** that Abraham Lincoln was assassinated 
- John **hopes** that Mary will be at the party 
	- There's a **probability** that something will happen in the future
- John **believes** that American foreign policy is responsible for the state of the word 
	- There is **no commitment** to whether the proposition is true or false, and different agents may have conflicting beliefs
What **matters** is whether the proposition is **true** or **false**
- This is what defines the state of the world, according to an agent
### Reasoning
Explicitly representing **all** representations believed to be true is **difficult**
**Reasoning** bridges the gap between what is **represented** and what is **believed** by the agent
- Let $KB$ be a set of **propositions** believed to be true and $α$ be a proposition **not** in $KB$ 
- Then $α$ is said to be logically entailed by $KB, KB |= α$, if we believe $α$ to be implicitly true given the propositions in $KB$ 
Knowledge representation languages need to have a well-defined notion of **entailment** 
- What does it mean for a proposition to be true or false? 
- What else we can decide is true or false based on that knowledge?
The **expressiveness** of a representation language has a direct impact on the **computational complexity** of the reasoning process
### Knowledge representation hypothesis
Any mechanically embodied intelligent process or agent:
- Contains a **collection of propositions** which is **believes** to be **true**
- **Reasons** with the propositions during its operation

Why do we want to build such knowledge-based systems?
- We can **add new tasks** that **depend** on previous knowledge 
- We can **extend** existing behaviour by **adding** new beliefs 
- It is easier to **debug** faulty behaviour
	- i.e., by locating erroneous beliefs 
- We can concisely **explain** and **justify** the behaviour of the system

### Rule based systems: Pros and Cons
- Provide an easy **mapping** between how expert expresses their **knowledge** and the **format of rules** (cf. ML) 
- Each rule represents an **independent piece of knowledge** 
	- However, this makes the relationship between the rules more **opaque**, and 
	- it becomes **less manageable** as the number of rules increases 
- **Separation** of knowledge from the **processing/control structures** 
- Ability to represent and reason with **uncertain** knowledge 
- Exhaustive search through all rules during each inference cycle (can be **costly**) 
- The Knowledge Acquisition Bottleneck: The system does **no independent learning** 
- Rule-based systems may be **brittle**
## Logical Representation
### Propositions
Typically, it is more natural to represent knowledge as logical formulae rather than as a table of information 
- With formulae, it is easier to **check correctness** 
- We can incrementally **add** to formulae easily 
- We can **extend** with infinitely many variables and domains 
For efficient reasoning, we can exploit the **Boolean nature** of such formulae 
A **proposition** is a statement that is true or false, which naturally can be represented as a logical formulae
$$a\land b\land c$$
### Semantics
When creating a knowledge base (KB), we must choose the **variables** that will be used to build propositions 
- These should have **meaning** to the KB designer
- We then give the system **knowledge** about the domain, and can then make inquiries 
	- Knowledge will take the form of a **definite clause**, $h ← b,$ which consists of two parts 
	- The **body**, $b$, is a **logical formulae** that can evaluate to true or false 
	- The **head**, $h$, is a **variable** that can be determined to be true as a result of b being true 
	- $\leftarrow$ "*Can be derived from*"
	- $h \leftarrow b$ 
		- $h$ can be derived from $b$
- Notably, the **system** does **not** understand the meaning of the symbols 
- The **user** must **interpret** the semantic meaning of the symbols

**User's view**:
- The user must decide the task domain, this is known as the **intended interpretation** 
- A variable must be **associated** with each proposition the user wants to represent 
- We must tell the system the clauses that are true in the intended interpretation, this is known as **axiomatizing the domain** 
- If $KB |= α$ then $α$ must be true in the intended interpretation 
- Users interpret the system’s responses using the intended interpretation of the symbols

**System's view**:
- The system does **not have access** to the intended interpretation 
- It is **only** aware of the **knowledge base** 
- The system can **determine** if a particular formula is a logical consequence of $KB$, but does **not** understand what that formula really means
## Expert Systems
An **expert system** is a computer program that represents and reasons with knowledge of some **specialist** subject with a view to solving problems or giving advice 
- **Simulates** human reasoning (in a domain) ▶
- **Performs** reasoning over a representation of human knowledge 
- Solves problems with **heuristic** or **approximate** methods 
- $$\text{Knowledge} + \text{Inference} = \text{Expert System}$$
![[Pasted image 20231110084939.png]]

### Example: MYCIN System
![[Pasted image 20231110090451.png]]

![[Pasted image 20231110090459.png]]

- **Knowledge acquisition program**
	- Takes information from humans/experts and turns it into **knowledge** for the knowledge base

![[Pasted image 20231110090506.png]]

![[Pasted image 20231110090512.png]]
#### Control Structure

![[Pasted image 20231110090521.png]]

![[Pasted image 20231110090530.png]]
#### AND/OR tree

![[Pasted image 20231115105102.png]]

**MYCIN therapy rules:**

![[Pasted image 20231115105118.png]]
#### Conclusion

![[Pasted image 20231115105319.png]]

## Rules as knowledge
People tend to associate **intelligent** behaviour with **regularities** in behaviour
- Rational people act **consistently**
**Production rules** are a formalism that has been used in *automata theory*, *formal grammars* and the design of *programming languages* 
- In expert systems literature, they are often referred to as **condition-action** rules or **situation-action** rules
### Canonical Systems
A canonical system is a formal system based on:
- An **alphabet**, $A$, for making strings
- Some **strings** that are taken as **axioms**
- A set of **productions** of the form:
	- $\alpha_{1}\$_1...\alpha_m \$_{m}\rightarrow \beta_{1}\$_1...\beta_m$  
	- Grammar rules for **manipulating** strings of symbols
	- These grammar rules are also known as **rewrite rules**
		- (similar to regular expressions)
### Knowledge Representation
We have a **vocabulary** (instead of the alphabet in canonical systems) that consist of:
- A set $O$ of **names** of objects in the **domain**
- A set $A$ of **attributes** of the objects
- A set $V$ of **values** that these attributes can take
We also have a **grammar** for generating symbol structures
- Object-attribute-value **triples**
- $(o,a,v),o\in O,a\in A,v\in V$
### Working memory
Working memory (WM) is a **store of facts** (assertions/propositions)
- These define the **initial state** of the $KB$/Model
- **Rules** define the **operators** allowing **transitions** from one state to another
Each **fact** is referred to as a **Working Memory Element** (WME)
- Facts are described using the **vocabulary** and **grammar** of the system
- Facts can be **interpreted** as an existential sentence in FOL
### Production Rules
Production rules take the form:
- if $P_1$ and $P_{2}$ ... and $P_m$ are $TRUE$
	then perform actions $Q_{1}$ and $Q_2$ ... and $Q_{n}$
**Two-part** structure:
- An **antecedent** set of **conditions** (the *if* part of the rule)
	- A **condition** is represented by an **object-attribute-specification** vector 
	- Takes the form type $\text{attribute}_1:\text{specification}_1 ... \text{attribute}_k:\text{specification}_k$ 
	- The set of conditions is **interpreted conjunctively** 
	- A condition (that is not negated) must **match** a WME 
	- Matching implies that the type is **identical** 
	- Each attribute-specification pair in the condition has a **corresponding** attribute-value pair in the WME, where the value matches the specification
	- If there is a WME for each condition, the **consequent** action will be performed 
- A consequent set of **actions** (the *then* part of the rule)
	- Actions operate (add/delete/modify facts) on **working memory**
#### Example

![[Pasted image 20231115171341.png]]
### The Rule Interpreter
**Recognise-act** cycle
- **Match** the **antecedent** conditions of rules against elements in **working memory**
- If **more than one** rule's **antecedent** matches, choose one of the rules based on some **conflict resolution** strategy
- Apply the rule
- Repeat the cycle
Cycle **halts** when no rules become active or if the **action** of rule fired is to **halt**
### Controlling Inference Behaviour
**Global Control**
- Domain **independent**
- **Hard coded** with inference engine
**Local Control**
- Domain **dependent**
- Coded in the form of **meta rules**
	- Reason about **which** rule to fire rather than **about objects** in the domain
### Behaviour as proofs
When facts **match** a rule's condition part, the rule fires - either:
- Adding a **new fact**
- Arriving at a **solution**
Producing a solution from a knowledge base is akin to a logical proof
- We are demonstrating that something logically **follows** from the **initial state** of the system
- We call the series of rules we fire an **inference chain**
There are 2 main ways to build such a chain:
#### Forward Chaining
**Data driven**: Starts from **known data** (facts)
- Facts that **can** be inferred **will** be inferred even if they are not related to the goal

Procedure of **matching** production rules that **can be fired**
- Select rules that produce new WMEs for the KB
- Repeated until **no rules** can fire, then **check** to see if the desired solution (fact) is not in the KB
- Forward chaining is both **logically sound** and **complete**
However:
- Make undertake significant processing on firing rules that do not contribute towards the goal

![[Pasted image 20231116210544.png]]

![[Pasted image 20231116210513.png]]
#### Backward Chaining
**Goal driven**: System has a **goal** and the inference engine attempts to find the **evidence** to prove it
- Only use data which is **needed** to determine the goal

Start from the **query**, and work backwards to determine if it is a logical **consequence** of the KB
- We select an element of the clause, and find a **production rule** that results in that element being **added** to the KB
- We replace the element with the **condition** of the production rule
- We repeat until the query clause is **entirely** made up of elements that **already exist** in the KB
	- Or we fail

Unlike Forward Chaining, Backward Chaining is **non-deterministic** - base on the choice of production rules
- Backward chaining can stop early if one of the element of the query **cannot be derived**
- Since the query is made up of conjunctives, if one element cannot be derived from the KB, then the **whole query** cannot be derived
- However when choosing the production rule that derives the element, we may also hit a **dead end**
	- Doesn't mean we have to stop, just need to select a **new**production rule
	- We can only definitively say an element **cannot be derived** if we have explored **all** the clauses

![[Pasted image 20231117103451.png]]

![[Pasted image 20231117103500.png]]




#### Ask the user
We can also introduce **askable** clauses
- The only way to retrieve **new** information is from a user/expert
- Define some clauses as '**askable**'
	- Information the system can ask about
	- Instead of making the user input **all** information (tedious)
- We can modify **backward chaining** to incorporate the ability to clarify information with the user
- The user and the system now have a **symmetric** relationship

## Knowledge level debugging
There are **four** types of **non-syntactic** errors in rule-based systems
- An **incorrect answer** produced 
	- A clause that **should** be **false** has been **interpreted** to be **true** (or vice versa)
- An answer was **not produced**
	- A clause that **should** be **true** could **not** be **derived**
- The program gets stuck in an **infinite loop**
- The system asks **irrelevant questions**
	- Requires reassessment of the KB
### Incorrect answers
Suppose some clause/variable was proved **false** in the intended interpretation
- There **must** be some **rule** in the KB that was **used** to prove that clause
- Either:
	- **One** of the variables in the tule is **false** in the intended interpretation
		- We can debug this by asking the user if each variable should be true
	- **All** of the variables are **true** in the intended interpretation
		- The **rule itself** is wrong and should be reassessed
### Missing answers
If a variable is **true** in the intended interpretation, but could **not** be **proved**, then either:
- There is **no appropriate rule** for that variable
- There **is** a rule, and it **did not fire** when it should have 
	- This means that one of the variables in the condition **should** be true, but is not
- We can solve this **recursively**, by finding all the variable that **should** be true but to not have a rule
### Infinite Loops
A knowledge based system can get stuck in an infinite loop if the rules are **cyclical**
- If you convert the KB into a **directed graph**, can check for **cycles**
	- Nodes are the **variables**
	- Edges represent that the source node is used to **derive** the destination node
- Rules should be reassessed to ensure an **acyclic** KB
- Forward chaining **cannot** get stuck in infinite loops
	- Only fires a rule if the consequent set is **not already** in working memory
## Conflict Resolution
The firing of a rule may **affect** the activation of **other rules**, since it changes the KB

The method for choosing **which** rule to fire when more than one **can** be fired in a given inference cycle is called a **conflict resolution**
- Can significantly change the behaviour of the system
- Can affect the runtime of a backward chaining system
The set of rules than can potentially be fired in a **single cycle** is referred to as the **conflict set**
#### Basic approach to conflict resolution
- Fire rules in order of **appearance** in the **knowledge base**
	- **Stop** when the **goal** is **reached**
	- However:
		- **Rule order** has a strong influence on outcome
		- This ordering is a form of **implicit** knowledge
- Alternative: use **rule priority**
	- Make **explicit** the order in which rules may fire
	- However, it is difficult to define priorities, and makes it harder to add new rules
### Conflict Resolution Mechanisms
Typically, use the following rule-independent conflict resolution mechanisms :
- **Specificity**: Fire the **most specific** rule 
	- More **conditions** = more **difficult** to satisfy 
		- Takes more data in working memory into account 
	- Used to deal with **exceptions** to more general rules

![[Pasted image 20240106130204.png]]

- **Recency**: Fire the rule that uses the data most **recently** entered into the working memory

![[Pasted image 20240106130232.png]]

- **Refractoriness**: A rule is only allowed to fire once on the same data 
	- Refractoriness **prevents loops** in inference
#### Example

![[Pasted image 20240106130419.png]]
### Meta knowledge
Knowledge **about knowledge** 
- Concerns the use and control of **domain knowledge** in an expert system 
- **Represented** in the form of **meta rules** 
- Determine the strategy for the use of task specific rules in the expert system 
	- May be domain independent but more likely to be domain dependent 
	- Elicited from the domain expert in addition to domain (task specific) knowledge
#### Examples
Domain **independent** meta rule:

![[Pasted image 20240106130724.png]]

Domain **specific**:

![[Pasted image 20240106130745.png]]

## Efficient Rule Matching
Motivating observations: 
- The working memory is only modified very **slightly** in each **recognize-act cycle** 
- Many rules **share conditions** 
Many systems use variations of the RETE algorithm: 
- Create a **network** from **rule antecedents** (offline) 
	- Two types of nodes 
	- α node: represents **simple**, **self-contained** tests 
	- β node: variables create constraints between **different conditions** 
- Example:
	- `If (person name:x age:(<14) father:y) (person name:y occupation:doctor) THEN ...`
#### RETE algorithm
- During operation of the Production System 
	- Tokens representing **new** or **changed** WMEs are **passed through the network** 
	- Tokens that make it through the network **satisfy** the rule 
		- A **new** conflict set is generated from the conflict set of the **previous cycle**, incorporating any changes made to WM 
		- Only a small part of the WM needs to be matched 
	- If a token cannot move through the network, it is **reassessed** when the corresponding WME is **modified**
## Assumption-based Reasoning
- Often, we want agents to make assumptions rather than doing deduction from their knowledge 
**Abduction** - 
- An agent makes **assumptions** to **explain observations** 
	- For example, *it hypothesizes what could wrong with a system to produce the observed symptoms* 
**Default Reasoning** - 
- An agent makes **assumptions** **of normality** to **make predictions**
	- For example, *a delivery robot may assume that a route is open, even if this is not always true*
### The Assumption-based Framework
Defined in terms of two sets of **formulae**: 
- A set of closed formula, $F$, called the **facts** 
	- These are given as **true** in the world 
	- Can include **integrity constraints**, which are formulae that evaluate to **false** 
	- Means that every variable in the condition **cannot be true at the same time** 
- A set of formulae, $H$, called the **possible hypotheses** or **assumables**
### Default Reasoning and Abduction
There are two strategies for using the assumption-based framework: 
**Default Reasoning** - 
- Where the truth of a clause $g$ is **unknown** and is to be determined 
	- An **explanation** for $g$ corresponds to an **argument** for $g$ 
**Abduction** - 
- Where $g$ is **given**, and we are interested in **explaining** it 
- $g$ could be an observation in a recognition task, 
- or a **design goal** in a design task 
Given **observations**, we typically do **abduction** 
**Default reasoning** is then used to find the **consequences**
### Computing Explanations
We need to find the set of **assumables** that **imply** our clause/query 
- For **each variable** in our query, we **select** a rule that results in that variable being **added** to working memory 
- We **replace** such variables with the **condition** of the **corresponding rule** 
- Repeat until **all variables** in the query are **assumables** 
- Must ensure we do **not violate integrity constraints**