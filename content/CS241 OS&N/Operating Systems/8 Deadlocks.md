A **set** of processes is in a **deadlock** when each process in the set is waiting for an **event** that can be caused only by **another process** in the **set**
- Event **never occurs**
The **events** we are interested in are **acquisition** or **release** of some type of resource e.g.
- *Mutex locks, semaphores*
- *CPU*
- *File*
- *I/O devices*

![[Pasted image 20231127093348.png]]

We need a systematic way to **detect** deadlocks
#### System Model
- System consists of different **types** of **resources** $R_{1}, R_{2}, ..., R_{m}$
	- *Mutex locks, CPUs, I/O devices*
- **Each** resource type $R_{i}$ has $W_{i}$ instances
	- e.g. *3 mutex locks, 4 printers etc.*
- A set of **processes** $\{P_{0}, P_{1}, ..., P_{n}\}$
- Each process utilizes a resources as follows:
	- **Request**
	- **Use**
	- **Release**
#### Necessary conditions for a deadlock
**Mutual exclusion**
- Only **one** process can use an instance of a resource at a time
**Hold and wait**
- There must be a process **holding** some **resources** while waiting to acquire **additional** resources held by **other processes**
**No preemption**
- A resource can be **released** only **voluntarily** by the process **holding it**, after that process has **completed** its task
**Circular wait**
- There must exist $\{\tilde{P_{0}}, ..., \tilde{P_{n}}\}\subseteq \{P_{0}, ..., P_{n}\}$ such that:
- $\tilde{P_{0}}$ is waiting for $\tilde{P_{1}}$
- $\tilde{P_{1}}$ is waiting for $\tilde{P_{2}}$
- ...
- $\tilde{P_{n}}$ is waiting for $\tilde{P_{0}}$
### Resource allocation graph - detecting deadlocks
A **directed** graph $G=(V,E)$ where
- $V$ is **partitioned** into **two** types
	- $P = \{P_{1}, P_{2}, ..., P_{n}\}$
		- The set consisting of all of the **processes** in the system
	- $R = \{R_{1}, R_{2}, ..., R_{n}\}$
		- The set consisting of all of the **resources** in the system
- **Request edge**
	- Directed edge $P_{i}\rightarrow R_{j}$
- **Assignment edge**
	- Directed edge $R_{j}\rightarrow P_{i}$

![[Pasted image 20231127094428.png]]
#### Examples

![[Pasted image 20231127094446.png]]

![[Pasted image 20231127094455.png]]

![[Pasted image 20231127094749.png]]
#### Rules for detecting deadlocks
- If graph contains **no cycles** $\implies$ **no deadlock**
- If graph contains a cycle $\implies$ need to look further
	- For **small** examples, we can **manually** detect deadlocks
	- For larger, we need an algorithm
### Deadlock Detection Algorithm

![[Pasted image 20231127094955.png]]

## Handling deadlocks
Ensure that the system **never enters** a deadlock state
- Avoid
- Prevent
### Deadlock Prevention
Ensure that at least on of the necessary conditions for deadlock **doesn't hold**

- **Mutual exclusion**
	- **Cannot** be prevented for non-sharable resources
- **Hold and wait**
	- Must **guarantee** that whenever a process requests a resource, it does not hold any **other** resources
		- Ensure a process either gets **all** or **none** of its required resources
- **No preemption**
	- If a process, **holding** some resources, **requests** additional resources that **cannot** be immediately **allocated** to it, then all resources currently being held by the process are **released**
- **Circular wait**
	- **Number** the resources and require that each process requests resources in an **increasing** order of enumeration
		- Process **holding** resource $n$ **cannot** request a resource with a number **less** than $n$

#### However, deadlock prevention leads to a more **restrictive** system
**Harmless** requests could be **blocked**

Consider the following system:
- $P_{1}$ and $P_2$ each **require** **resources** $R_1$ and $R_2$
- Resources are numbered: $n(R_2)>n(R_1)$
	- Each process must request resources in **increasing** order
- If $R_2$ is already **held** by $P_1$ then the request of $R_1$ by $P_1$ will be **blocked** (not granted)
- But it is **safe** to grant this request since there will be **no cycle** in the resulting system

![[Pasted image 20231221181339.png]]

### Deadlock avoidance
Deadlock **avoidance** is **less restrictive** than deadlock prevention
- It determines if a request should be granted based on if the **resulting** allocation leaves the system in a **safe state**

![[Pasted image 20231221181607.png]]

- A safe state is a state where deadlock can **never occur**, no matter what future requests arrive

How do we **determine** if a state is safe?
#### Deadlock avoidance algorithm
- Needs advanced **information** on resource requirements
- Each process **declares** the maximum number of instances of each resource type that it may need
- Upon **receiving** resource request, the deadlock avoidance algorithm **checks** if granting the resource immediately leaves the system in a **safe state**
- If so, grant the request immediately, otherwise **wait** until the state of the system changes to a state where the request can be granted **safely**
#### How to determine if a state is safe?
In the last example the state was safe because there was no cycle
- But in general the **presence** of cycles does **not guarantee** deadlocks
So to determine the safety of state we need **more general algorithm**
##### Banker's safety algorithm
- Let:
	- $n$ = number of process 
	- $m$ = number of resource types
- **Available:** Vector of length $m$
	- If available $[j]=k$, then there are $k$ instance of resource type $R_j$ available
- **Max**: $n \times m$ matrix
	- If $Max[i,j]=k$, then process $P_i$ may request **at most** $k$ instances of resource type $R_j$
- **Allocation**: $n \times m$ matrix
	- If $Allocation[i,j]=k$ then $P_i$ is currently allocated $k$ instances of $R_j$
- **Need**: $n \times m$ matrix
	- If $Need[i,j]=k$ then $P_i$ may need at most $k$ more instances of $R_j$ to complete its task
		- $Need[i,j]=Max[i,j]-Allocation[i,j]$

e.g. 

![[Pasted image 20231221182853.png]]

1. Let $Work$ and $Finish$ be vectors of length $m$ and $n$ respectively. Intialise:
	- $Work = Available$
	- $Finish[i] =$ false for $i = 0, 1, ..., n-1$
2. Find an $i$ such that both:
	- $Finish[i] = false$
	- $Need_{i}\leq Work$
	- If no such $i$ exists, go to step 4
3. $Work = Work+Allocation_i$
	$Finish[i]=true$
	Go to step 2
4. If $Finish[i]==true$ for all $i$, then the system is in a **safe** state. Otherwise, in an **unsafe** state
#### Resource Request Algorithm
Tells us if granting a resource, the **resulting** system will be in a **safe state**?

**Algorithm**: **Pretend** the request is **granted**, **determine** if the **resulting** state is safe using **Banker's safety algorithm**
- If so, grant the request
- Otherwise keep the request **pending** until state change

![[Pasted image 20231221183756.png]]


