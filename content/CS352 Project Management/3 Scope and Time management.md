Once the project is given the go-ahead and the Charter is **signed-off** by the sponsor, the **Project Manager takes over**:
1. Breakdown work, Plan timeline
2. Set out milestones, Identify bottlenecks
3. Plan budget, Allocated resources

# Planning stage
## Scope Management
Create a Work Breakdown Structure (WBS)

![[Pasted image 20251020152833.png]]

"A **deliverable-oriented** hierarchical decomposition of the **work** to be executed by the project team to accomplish the **project objectives** and create the **required deliverables**"

- Defines the **scope** of the project

**Deliverables** - *Outputs*. Must align with initial objectives.

**Work package** - *Smallest unit* of the WBS
- Set of related tasks and deliverables
- Clearly defined interactions with other WPs
- Clear identification of inputs, outputs and internal activities
- Clear estimation of cost, duration and resources

**Milestones** - Points of **control** that **separate** work packages

When defining this, need to be **deliverable-oriented** - NOT objective or process oriented

Difference between *objectives* and *deliverables*
- **Objectives** are SMART
- Objectives are good when writing down the **purpose**
- They show the desired **benefits, outcomes** or **improvements**

- Whereas **deliverables** are **outputs** or **products**
- Related to **scope**/**work**
- Estimable, Parallelisable, Purchasable

The WBS should be **deliverable**-oriented (*what*), **not** objective-oriented (*why*) or **process** oriented (*how*)

The **pros** of doing this include:
- Deliverables are easier to **estimate** than objectives
- Makes us focus on the **essentials**
- Once scope is defined, the ‘what’ is **fixed**, but ‘how’ remains **fluid**
- Allows us to **manage scope**

The cons may be:
- Loses sight of the objectives
- **Restricts further scope changes** and creativity from developers
- May **overlook** **costly processes** that consume time and resources
- Doesn’t come naturally

## Time Management
### Estimating activity durations
- **Analogous** - How long it took **last time** (adjusted to this project)
- **Parametric** - As above, but with a statistical model
- **Team-based** - by the people doing the work (maybe poll/crowd-source)
- **Three-point** - $m$ mean, $a$ min, $b$ max

![[Pasted image 20251020161339.png]]

Very useful to also have a measure of **uncertainty**

![[Pasted image 20251020161359.png]]

### Sequencing activities
- Relationships
- Dependencies
- Resource constraints
- Milestones

![[Pasted image 20251020161458.png]]

#### Gantt Charts
Graphical visualisation of the project, showing:
- **temporal** schedule of activities
- **Dependencies** between activities
- **progress** of activities

![[Pasted image 20251020161707.png]]

#### Project network diagram
A graphical way to view a project's tasks, dependencies and the **critical path**

![[Pasted image 20251020161859.png]]

![[Pasted image 20251020161906.png]]

#### Critical path method
**Critical path** - A **sequence of activities** starting from the first activity of the project and ending with the last. Activities on a critical path **cannot be delayed** *without extending* the **project duration**

Terminology:
- Duration (D) - Duration of the activity
- Earliest start (ES) - Earliest time an activity can start
	- = maximum EF from immediate predecessors
- Earliest finish (EF) - $ES + D$
- Latest finish (LF) - Latest time an activity can finish without delaying the project
	- Minimum LS from immediate successors
- Latest start (LS) - $LF - D$

![[Pasted image 20251020162458.png]]

Algorithm:
- Construct **Project Network Diagram**
- Forward pass:
	- $ES =$ Maximum EF from immediate predecessors
	- $EF = ES + D$
- Backwards pass:
	- $LF =$ Minimum LS from immediate successors
	- $LS = LF - D$
	- $TF = LS - ES = LF - EF$

**Total Float** (TF) represents the **slack** that a task has
- The total amount of time an activity can be delayed without changing the **project finish date**

$TF = LF - EF=LS-ES$

![[Pasted image 20251020163316.png]]

**Free Float** (FF) represents the amount of time that an activity can be delayed without delaying the early start date of any **subsequent activities**
- $FF=ES_{next}-EF$

![[Pasted image 20251020163323.png]]

$FF \leq TF$

**Drag time** is the amount of time a critical task **adds** to the project duration
- The time it would **need to be shortened by** in order to **no longer be critical**
- The minimum of: $D$ *or* the **minimum $TF$ of parallel tasks**

![[Pasted image 20251020163752.png]]

**Crashing** is 'a schedule compression technique in which costs and schedule trade-offs are analysed to determine how to obtain the greatest amount of compression for the least incremental cost'

**Crash duration** - Shorted possible time for which an activity can be schedules to **speed up the whole project**
- $D-Drag$

### Program Evaluation and Review Technique (PERT)
Takes a **skeptical view** of time estimates
Variation on CPM using **three-point** estimation
- **Shortest** possible time each activity will take $\alpha$
- **Most likely** length of time ($m$)
- Longest time, if the activity takes **longer** than expected ($b$)

![[Pasted image 20251020164320.png]]

