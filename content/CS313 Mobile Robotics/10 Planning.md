We would like to have:
- Collision-free trajectories
- Robot should reach the goal as quickly as possible
- Take potential uncertainties into account

**Motion planning**

Given an **initial pose** (position and orientation) and the goal pose of a robot, find a **path**, in the form of a continuous sequence of robot poses that:
- Do not collide or contact with any obstacle
- Allow the robot to move from its starting pose to its goal pose
- Report failure if such a path does not exist

![[Pasted image 20260302141104.png]]

Input:
- Start position of the robot
- Desired goal pose
- A (geometric) description of the robot
- A (geometric) description of the environment

Output:
- A (minimum cost) path that moves the robot from the start to the goal while never colliding with obstacles

**Configuration space**

Find some space where every point of the robot is specified in the space, with no possible collision

The configuration space represents all possible positions and orientations a robot can achieve in its environment
- Regardless of the geometry of the robot

For a **rigid body** robot:
- We can **transform** the robot and the obstacles in the workspace into the configuration space
- The **C-space transformation**

![[Pasted image 20260302141327.png]]

Essentially we can move/transform the obstacles to be large in order to be able to represent the robot as a point with larger obstacles rather than as a circle with some diameter and a smaller obstacle. All relative, basically, for easier representation

This might not be as easy as the previous example:

![[Pasted image 20260302141714.png]]

Next we can start to deal with our robot as a **point**, allowing us to plan a path in the 'C free' (free space)

![[Pasted image 20260302141818.png]]

**Discretised** C-space

We would like to represent the space in different ways in order to make this computable:

![[Pasted image 20260302142010.png]]

There are some examples of such representations of C-space

![[Pasted image 20260302142024.png]]

There are some drawbacks or goals we may look for when constructing a path

![[Pasted image 20260302142032.png]]

**Road maps**

We would like to try and find this path by constructing a graph (or network of paths) within the free space

![[Pasted image 20260302142145.png]]

**Visibility graph**

This is a **non-directed graph**
- Linking nodes by edges of obstacles or lying completely in the free space (Cfree)

![[Pasted image 20260302142249.png]]

The way we can **reduce** the visibility graph is by **keeping** *supporting* lines but **removing** *separating* lines

![[Pasted image 20260302142417.png]]

Visibility graphs have the following properties, which can be useful:

![[Pasted image 20260302142437.png]]

However, this means we get very close to obstacles, and with the uncertainty that comes with sensors this can be dangerous

![[Pasted image 20260302142633.png]]

---
**Voronoi diagrams**

Obstacles are points within the space, and cells are equally far apart from the obstacles, and we would like to find a way in the **middle** of obstacles (as far from the obstacles as possible)

![[Pasted image 20260302142816.png]]

They also have good properties, with computational complexity scaling with the edges rather than nodes, and scaling better even:
- However won't be particularly practical in terms of distance travelled due to obstacle avoidance

![[Pasted image 20260302142843.png]]

![[Pasted image 20260302143140.png]]

How do we do this cell decomposition, in order to get discrete cells which don't overlap?
- We can use **exact cell decomposition**

![[Pasted image 20260302143319.png]]

![[Pasted image 20260302143404.png]]

![[Pasted image 20260302143412.png]]

Alternatively, we can use **approximate cell decomposition**

![[Pasted image 20260302143430.png]]

![[Pasted image 20260302143437.png]]

![[Pasted image 20260302143444.png]]

![[Pasted image 20260302143451.png]]

![[Pasted image 20260302143458.png]]

![[Pasted image 20260302143506.png]]

---

**Sample-based approaches**

Instead of explicitly constructing the C-space, these approaches **sample** oconfigurations and connect them to find feasible paths
- Effective for handling high-dimensional C-space and complex obstacle shapes

![[Pasted image 20260302144434.png]]

A couple main approaches:
- **Probabilistic road maps** (PRM) - suitable for multiple queries, pre-computes a road map of feasible paths
	- ![[Pasted image 20260302144446.png]]
	- ![[Pasted image 20260302144455.png]]
	- ![[Pasted image 20260302144722.png]]

- **Single-Query planners** - Designed for one-time path-finding, such as rapidly-exploring random trees
	- ![[Pasted image 20260302144737.png]]
	- We use **rapidly-exploring random trees**:
		- ![[Pasted image 20260302144921.png]]
		- ![[Pasted image 20260302144938.png]]
---

Finally, we have **potential fields**

All previously discussed methods are **global** methods
- WE try to capture the **global connectivity** of $C_{free}$ beforehand, i.e. *offline*

Potential field methods are **local methods**
- They make decisions based only on nearby information rather than considering the entire environment at once

And can be performed online
- Planning and execution happen in real time, without needing a pre-computed connectivity map

The potential field method **treats** the robot as a **point** under the **influence** of an **artificial potential field**, $U(q)$
- The APU $U$ is the **sum** of an **attractive** and a **repulsive** potential function
- $U(q) = U_{att}(q)+U_{rep}(q)$

![[Pasted image 20260303150840.png]]

An attractive potential can, for example, be defined as a **parabolic function**

![[Pasted image 20260303150911.png]]

- Where $||q-q_{goal}$ denotes the **Euclidian** **distance** from the robot to the goal
- This attractive potential is **differentiable**, leading to the attractive **force**

![[Pasted image 20260303151006.png]]

This force **increases** as the robot moves **farther** from the **goal**
- The **closer** the robot gets, the **weaker** the **attractive force** becomes

The **repulsive force** is designed to **push the robot away** from **obstacles**.
- This repulsive potential should be very strong when the robot is **close** to the object, but should not influence its movement when the robot is far from the object.
- e.g.:
- ![[Pasted image 20260303151132.png]]

The repulsive potential function is **positive or zero** tends to **infinity** as $q$ gets closer to the object.
- If there are many obstacles in the environment, then the **total** of **repulsive potential field** is the **sum** of all the obstacles' repulsive potential field

![[Pasted image 20260303151241.png]]

![[Pasted image 20260303151256.png]]

The **whole potential** is a **vector sum** of the attractive and replusive potential functions:
$$U(q)=U_{att}(q)+U_{rep}(q)$$

The **negative gradient** of the potential is **force**
- We can find the **path** by following the **negative gradient** of the combined potentials

![[Pasted image 20260303151455.png]]

In practice we can use the force $F(q)$ as a **control input** for the robot

![[Pasted image 20260303151512.png]]

![[Pasted image 20260303151555.png]]

It is possible to find a **local minimum** where attractive and repulsive forces **balance**, meaning the robot can make **no progress**
- Can avoid this using appropriate **escape techniques**

So, we have the summary:
- **Completeness**
	- **Not complete** - given that a *local minimum* can exist
- **Optimality**
	- **Not optimal** - also due to *local minimum*
- **Computational complexity**
	- Analytical potential field is local, so **much cheaper**.
	- For numerical approaches, complexity depends on how to **escape from local minima**

---

**Search algorithms**

---

**Uninformed search**

There is **no additional information** about the goal
- **Blind** search
- Expand different nodes in the hope of finding the goal at some point

In the special case that every individual edge in the graph assumed the **same traversal cost** (e.g. occupancy grid), can use **depth-first** and **breadth-first searches**
- Dijkstra's algorithm and variants allow for the computation of optimal paths in **non-uniform** cost maps

Breadth first:
- Start at the initial node and explore all neighbouring nodes in layers

In mobile robots, this looks like the **grassfire** (or wavefront) expansion:
- **Starts** from the **goal** and propagates **outwards**
	- This is so you only have to create the map once and they can find the optimal path from any cell afterwards, kind of like dijkstra
- Each cell is marked with its Manhattan distance to the goal
- This process continues until the initial robot position is reached
- The trajectory is found by linking adjacent cells moving closer to the goal.

![[Pasted image 20260303152749.png]]

![[Pasted image 20260303153403.png]]

**Dijkstra**'**s algorithm** expands the nodes with least cost first, and gives you the optimal solution from all of the other nodes.

It's **greedy**

![[Pasted image 20260303153456.png]]

Performance may depend on **obstacles**

![[Pasted image 20260303153514.png]]

---

**Informed search**

If we **have information** about the **goal**
- Know that one neighbour is "*more promising than the others*"
- Rely on some heuristic function $h(n)$ (= cost estimate) to guide the search

**Greedy** best-first:
- Just chooses the first best node, **ignoring** the **cost so far**

![[Pasted image 20260303153824.png]]

Instead, we can use $A^*$

A* keeps a memory of the **exact cost** of the current path $g(n)$ and an **estimated** cost of each node $n$ to the goal, $h(n)$, and adds both costs to come up with a better cost function $f(n)=g(n)+h(n)$

![[Pasted image 20260303154111.png]]

![[Pasted image 20260303154117.png]]

Typical **assumptions**:
- Robots **knows** its own **location**
- Robot computes path based on a **correct map**
- The correct motion commands are executed

Potential **problems**:
- Robot slightly delocalised
- Shorted path often guides robot along trajectory close to obstacles
	- Can expand/inflate obstacles and this is ok if we have the clearance in the environment
- Alignment of the planned trajectory and the grid structure of the map
	- Discretise C-space into 45deg

Whereas, in **local search**:
- We don't worry about the path
	- If reaching the goal is the only priority, and the path to the goal doesn't matter
	- Can represent the workspace as a potential field
	- Move to only neighbours based on gradient

![[Pasted image 20260303154908.png]]