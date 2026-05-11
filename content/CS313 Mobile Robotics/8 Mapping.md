A map is the **model** of the **environment** that allows us to:
- Understand and limit the **localisation error** by recognising previously visited places
- Better perform other tasks, such as obstacle avoidance and path planning

The mapping **problem**:
- Given the sensor measurements, what does the environment look like?

Chicken and egg situation:
- Map + Sensor data = Robot Pose
- Robot Pose + Sensor data = Map

The **difficulty** of the mapping problem depends on:
- Size
- Noise in perception and actuation
- Perceptual ambiguity
- Cycles

Different map representations:
- **Dense matrix map**
- **Feature-based metric map**
- **Topological map**

![[Pasted image 20260223141530.png]]

Finally, we can use **Topological-metric** maps:

![[Pasted image 20260223141857.png]]

Given the observed sensor data and control input:
$$d=\{u_{1},z_{1},u_{2},z_{2},...,u_{n},z_{n}\}$$
What is the **most likely map** $m^*$?
$m^{*}=argmax_{m}P(m|d)$

Mapping with **known poses**:
- Assuming we know the pose, we can **drop** the **control input** (replace with $x_{t}$)

$m^{*}=argmax_{m}P(m|x_{1},z_{1},...,x_{t},z_{t})$

So, we can use **probability occupancy grid mapping**:

![[Pasted image 20260223142404.png]]

Occupancy grid maps:
- Represent the workspace by a **grid of cells**
- Grid structure is **rigid** and uniform
- Each cell is assumed to be *either* **occupied** or **free**
- Large maps require substantial memory resources
- Do not rely on a feature detector

We make a **few assumptions**:
**Assumption 1**: 
- The area that corresponds to a cell is either completely free or occupied.
	- This doesn't always hold, e.g.:
	- An object could be smaller than the grid size
	- Physical structure might not line up with the grid structure

![[Pasted image 20260223142655.png]]

This means that **each cell** can be **represented** by a **binary random variable** that models occupancy

![[Pasted image 20260223142728.png]]

We need to store for each cell a **probability** that it is **occupied**
- ![[Pasted image 20260223142830.png]]
	- Q: Tells you that the cell is fully occupied 80% of the time or with an 80% chance and unoccupied with 20% chance. Doesn't say anything about the amount of the cell being occupied

![[Pasted image 20260223142838.png]]

**Assumption 2**:
- The state is assumed to be **static**
![[Pasted image 20260223143043.png]]

**Assumption 3**:
- The cells (the random variables) are **independent** of each other

![[Pasted image 20260223143118.png]]

So to summarise:
- Robot positions are known
- The area that corresponds to a cell is either free or occupied
- The world is static
- The cells are independent of each other
- The **probability distribution** of the map can be given by the **product** over the cells:
	- ![[Pasted image 20260223143206.png]]

We can **update** each individual cell using the **Binary Bayes filter**

![[Pasted image 20260223143233.png]]

Lots of terms here are hard to estimate:

![[Pasted image 20260223143801.png]]

![[Pasted image 20260223143807.png]]

We can use log odds notation to make it additive instead of multiplicative:

![[Pasted image 20260223144243.png]]

So what is the **inverse sensor model?**

![[Pasted image 20260223144257.png]]

Examples in the slides

For **sonar**:

![[Pasted image 20260223144722.png]]

![[Pasted image 20260223144731.png]]

![[Pasted image 20260223144741.png]]

In the edges we are less certain about our measurements so in the center we might adjust by a higher value than the outer ones

![[Pasted image 20260223144936.png]]

