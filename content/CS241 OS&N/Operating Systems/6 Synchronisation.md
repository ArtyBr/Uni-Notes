#### Why Synchronise?
- **Concurrently** running threads/processes enter into a **race condition** when trying to **update shared variables**
- Only **one** of the values is **preserved**
	- A **race** to see which one is **last** (preserved)
- Should be **avoided** through proper **synchronisation**
#### Terminology
- **Process** refers to both **threads** and **processes**
	- **Both** need synchronisation
- Part of the code where a process **updates shared variables** is going to be called a **critical section** 
	- A process can have **multiple** critical sections 
- When **one** process is in a **critical** section, **no other** process should be allowed to be in it’s **critical** section. (**mutual exclusion**)
## The Critical Section Problem
Consider a **group** of processes $\{P_0, P_1, .. P_N\}$
- Each process has a critical section of code where they update some shared variables 
- **Critical section problem**: design a **protocol** such that: 
	- **No** two processes can **concurrently** **execute** their **critical sections** (mutual exclusion)

![[Pasted image 20231106091146.png]]

Each process, **before** entering its critical section, first has its **entry section** and **after** goes through its **exit section**
- These sections define the **protocol** that the process goes through in order to deal with the **critical section problem**

**Ideal solutions** to the problem must **satisfy** three criteria:
- **Mutual Exclusion** - If a process 𝑃 is executing its critical section, then no other processes can be executing their critical sections
- **Progress** - If **no process** is **executing** in its critical section and there exist **some processes** that wish to **enter** their **critical section**, then **one** of the **waiting processes** must be **able** to **enter** into its critical section
- **Bounded Waiting** – **No one** process should have to:
	- **Wait** **indefinitely** to **enter** its **critical section** while
	- **Other processes** are being **allowed** to **enter** and **exit** their critical sections **continually**

When checking for requirements:
- **Progress** can be satisfied while **bounded waiting** isn't
- However, if **progress** is **not** satisfied, **bounded waiting** **cannot** be satisfied
i.e. **Progress** is **necessary** but **not sufficient** for **bounded waiting**
#### False solution
![[Pasted image 20231106092457.png]]

$P_0$
- The moment turn becomes 0 it breaks the while loop (busy wait) and executes critical section
- Vice versa for $P_1$
- Processes **alternate** between each other, executing their critical sections

![[Pasted image 20231106092507.png]]

**Progress** -
- Lets say first turn = 1
- $P_1$ sets turn to 0
- CPU switches from $P_1$ to $P_0$
- $P_{0}$ breaks out of busy wait loop
- Executes, then comes back and waits for turn to become 0 again
- No further context switch has happened, so $P_1$ will not enter its critical section again
- Therefore progress **not satisfied**

**Bounded wait** - 
- Can happen that $P_1$ exits its external while loop if some condition is met, meaning it no longer sets turn to 0 and $P_0$ doesn't execute its critical section again
### Peterson's Algorithm
- Gary L. Peterson (1981) proposed an algorithmic solution for the critical section problem 
- The original algorithm was for two processes but this can be extended to any number of processes.

![[Pasted image 20231106093722.png]]

- Array of length 2 for 2 processes, but can be any $n$ for $n$ processes

- **Flag** - shows which process **wants** to execute
- **Turn** - shows which process' turn it is to execute
- Processes do **busy wait** if **flag** and **turn** are set to that of the **other process**

- **Mutual exclusion** satisfied
	- If $P_{0}$ is in its critical section
	- Therefore it must be so as it broke out of the busy wait while loop
	- **Case 1**
		- **Flag[1]** is **false**
		- Therefore $P_1$ has executed its critical section, or hasn't expressed its wish to execute it - therefore $P_1$ isn't in its critical section
	- **Case 2**
		- **Turn** is 0
		- **Flag[0]** must have been set to **true**
		- $P_1$ will keep spinning in its while loop since neither property is satisfied to break it
	- **Both** processes cannot execute their critical section at the **same time**
- **Progress** satisfied
	- Assume **none** of the processes are in their critical section
	- **Both** have already executed their critical sections
		- Both **flags** are **false**
	- **Case 1** - **Only** $P_0$ wants to enter its critical section
		- $P_0$ could go back to line 1 - setting **flag**[0] to **true**
		- turn $=1$
		- $P_0$ checks busy wait condition
		- **Flag[1]** has **not** been set to **true**
		- $P_0$ is able to enter its critical section
	- **Case 2** - **Both** processes want to enter critical sections
		- **Both processes** set their **flags** to **true**
		- Each process will try to update their turn variable
		- **No synchronisation** for turn
			- **Race condition** is **allowed** - they will enter a race
		- **One process** wins the race - turn variable is set accordingly
		- One process is able to execute its critical section as turn is set to this variable
- **Bounded wait** satisfied
	- Lets say $P_0$ is executing forever
		- Finding that **flag[1]** is **true** and turn = 1
	- **Can't happen** - When $P_1$ finishes its critical section, it sets **flag[1]** to **false** in which case $P_0$ will break its busy wait loop
#### Problems with Peterson's algorithm
Satisfies all 3 criteria, but **not perfect**
- Employs **busy wait**
	- Wastes CPU cycles so not efficient
- May **fail** in modern architectures
	- In modern computer architectures independent **read** and **write** operations may be **reordered** to make programs run more **efficiently**
	- ![[Pasted image 20231107140906.png]]
	- Context switch after turn=1 in $p_{0}$
	- $P_1$ does operations 1 - 3, and executes critical section
	- Context switch to $P_{0}$ before flag[1] = false
	- $P_0$ also executes critical section

## Solutions to CS problem using locks
All solutions here based on idea of **locking**
- Two processes cannot have a lock simultaneously

![[Pasted image 20231107141403.png]]

**Locking** and **unlocking** must be performed **atomically**
- **Atomic** = non-interruptable
	- CPU cannot perform a context switch during this operation

Modern machines provide special atomic hardware instructions to implement locks
One type: `test_and_set`
### `test_and_set` Instruction
**Definition**:
- Returns the **original value** of passed parameter `target`
- Set the **new value** of passed parameter `target` to `TRUE`

Needs to be implemented **atomically** using hardware

![[Pasted image 20231107141900.png]]

^ this is just an example, actually implemented in **hardware**
#### Solution using `test_and_set()`
![[Pasted image 20231107142001.png]]

A process can finish its critical section, unlock the lock, and then go back to the start and claim the lock again. This is why bounded waiting isn't satisfied
#### Better solution using `test_and_set()` for $n$ processes
![[Pasted image 20231107142543.png]]

![[Pasted image 20231107142550.png]]

- If a process $i$ wants to enter CS, sets waiting[i] to true
- key - local status of the lock
- When either key or waiting becomes false:
	- Breaks out and sets waiting to false
- Enters critical section
- Finds the **next process** still waiting to enter critical section
- If found, make that process non-waiting by setting its waiting value to false
### Synchronisation primitives
Hardware based solutions are generally **inaccessible** to programmers
- OS designers build **software tools** to solve critical section problem
	- **Mutex locks**
	- **Condition variables**
	- **Semaphores**
#### Mutex Locks
![[Pasted image 20231107143228.png]]

![[Pasted image 20231107143237.png]]
#### Semaphores
- Semaphores can have **integer values** 
	- Makes them more **powerful** than mutex locks 
- A zero value indicates that the semaphore is not available
- Positive value indicates that it is available 
- Can only be accessed via two indivisible (atomic) operations `wait()`and `signal()`

- `wait()` checks if the value of the semaphore is ≤ 0 and if so it makes the calling process wait until it becomes positive. 
- Once the the semaphore value is positive wait() function **decrements** it by 1. 
- `signal()` function **increments** the value of the semaphore by 1. 
- Both `wait()` and `signal()` must be performed **atomically**

![[Pasted image 20231107144358.png]]

![[Pasted image 20231107144417.png]]

![[Pasted image 20231107144428.png]]
### Common synchronisation issues
There still exist some issues while using primitives:
- **Deadlock**
- **Starvation**
- **Priority Inversion**
#### Deadlock
**Deadlock** - Two or more processes **waiting indefinitely** for an event that can **only be cause** by the **waiting processes**
- $A$ waits for $B$ to do something. $B$ waits for $A$ to do something. $A$ and $B$ are in a deadlock

![[Pasted image 20231107145328.png]]

After 4, no process can process as both are waiting for each other
- Since both have set each other to 0

To **avoid** deadlock:
- **Re-order** the operations so that process 1 waits on $S$ first instead of $Q$ 
	- **Only 1** of the processes can execute wait($S$), preventing deadlock
#### Starvation
Starvation occurs when a specific process has to **wait** indefinitely while others make progress
- **Opposite** of **bounded waiting**
Occurs when multiple processes are waiting on a **semaphore** and the signal call **wakes up** the **same** process again and again
- To **avoid** such starvation signal should randomly pick the process to wake up
#### Priority inversion
Sometimes the OS schedules processes based on priorities
- Schedules **higher** priority processes **before** lower priority processes
- **Inversion** happens when the **opposite** of this happens

Happens when:
- Higher priority process **requires** a **lock** which a **lower priority process** has **already taken**
 
- Higher priority process $H$ is scheduled when time gets to it
- Lower priority process $L$ is put in ready queue
- Discovers that it needs to acquire same mutex lock that $L$ has already taken, so gets **blocked**
- Once $H$ blocked, $L$ can run again!
- Medium priority process $M$ becomes **active** - doesn't require the lock
- Runs without any problems, but still preventing higher priority process from running since $L$ isn't finishing and can't unlock the mutex

![[Pasted image 20231112220605.png]]

- Solved by **priority-inheritance protocol**
	- All processes contending for same resource are **converted to the highest priority that needs the lock**
	- e.g. $L$ priority changed to **high** since this is the **highest** priority that **needs** the lock
### Common synchronisation tests/problems
There are some classic problems that can be used to **test** newly proposed synchronisation schemes
#### Bounded Buffer Problem
- $n$ buffers, each can hold **one** item
- Producer **produces** an item and writes to a buffer while the consumer **consumes** an item from a buffer

Make sure:
- Producer should **not** produce when **all** buffers are **full**
- Consumer should **not** consume when **all** buffers are **empty**
- Only **one** process can **access** the buffer at a given time

**Producer:**

![[Pasted image 20231112221021.png]]

- By issuing a wait on empty, producer is blocked from writing

**Consumer:**

![[Pasted image 20231112221035.png]]
#### Readers and Writers Problem
A **data set** is **shared** among a number of **concurrent** processes
- **Readers** - Only **read** from the data set
	- Do **not** perform any **updates**
- **Writers** - Can **both** read and write
Rules:
- Allow **multiple readers** to read at the same time
- **Only one** single **writer** can access the shared data at the same time

Different versions exist, but in the one we study:
- **Readers** are given **preference** over writers when **no process** is active
- **Writers** may **starve**

**Writer process**:

![[Pasted image 20231112223943.png]]

**Reader process**:

![[Pasted image 20231112224245.png]]
#### Dining Philosophers Problem
Philosophers spend their lives **alternating** **thinking** and **eating**
- **Don't interact** with their **neighbours**, occasionally try to pick up 2 chopsticks (one at a time) to eat from a bowl
	- Need both to eat, then release both when done
- In the case of 5 philosophers:
	- Bowl of rice (**data set**)
	- Semaphore **chopstick[5]** all initialised to 1
	- Two neighbouring philosophers cannot eat at the same time

![[Pasted image 20231112224229.png]]

![[Pasted image 20231112224307.png]]

**Possibility of a deadlock**
- Suppose that all five philosophers become hungry at the **same time** and each grabs the **left** chopstick

**Deadlock handling**:
- Allow at most **4** philosophers to be sitting simultaneously at the table
- Allow a philosopher to pick up the chopsticks only if **both** are available
	- **Picking** must be done in a **critical section**
- Use an **asymmetric** solution
	- An **odd**-numbered philosopher picks up **first** the **left** chopstick and **then** the **right** chopstick
	- **Even**-numbered philosopher picks up **first** the **right** and **then** the **left** chopstick