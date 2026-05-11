The OS maintains different **queues** for processes in different **states**
- Processes in the **ready state** are put in the **ready queue**
	- Not necessarily FIFO
	- Can be a priority queue or linked list based on **scheduling algorithm**
- The **CPU scheduler**/**short term scheduler** processes from the **ready queue** to be executed on the CPU

![[Pasted image 20231122091153.png]]

**Goal** of CPU scheduling: 
- **Increase** CPU **utilisation**
- Maximise % of time that the CPU is **busy**
#### CPU and I/O bursts
**Life** of a process: 
- CPU burst, followed by I/O burst
- ![[Pasted image 20231122091224.png]]
- When a process is in an I/O burst, the CPU can be scheduled to some **other process**

#### Performance Measures
**CPU utilisation** 
- Fraction/percentage of **time** CPU remains **busy** when there are jobs in the ready queue
**Throughput**
- **Number** of processes that **complete** their execution per unit of time 
**Turnaround time**
- Amount of time to **complete** a process
**Waiting time**
- Amount of time a process spends **waiting** in the **ready queue**
**Response time**
- Amount of time it takes from when a **request** was **submitted** until the **first response** is **produced**
### Types of scheduling
**Non-preemptive**
- Once the CPU is given to a process, the process **holds on** to the CPU until its current CPU burst finishes
**Preemptive**
- The execution of a process is **interrupted** in the middle to schedule another process
- **Preemptive** algorithms are typically **faster** and **more responsive**
#### Problems with preemptive scheduling
Can cause **race-conditions**
- If a process is pre-empted while it **updating** some **shared data**
	- Leaves data in an **inconsistent state**
- Another process is scheduled which **accesses** the **same data**
	- See the data in an inconsistent state
- Shared data should be updated within **critical sections** using proper **synchronisation primitives** 
## Scheduling algorithms
### First Come, First Served (FCFS) Scheduler
Processes are assigned to the CPU in **order** of their **arrivals**

![[Pasted image 20231122092355.png]]

Diagram on the right is the **Gantt chart**

![[Pasted image 20231122092834.png]]

**Performance** of FCFS **varies** greatly based on the **arrival sequence**
- If the **shorter jobs** arrive first, then performance is **better**
- FCFS is **non-preemptive**
### Shortest Job First (SJF)
The process with the **shortest** next CPU burst is selected
- (If there is a tie, then FCFS)

![[Pasted image 20231122093301.png]]

SJF is **provably optimal**, in that it gives the **minimum average waiting time**
- Moving a short process before a long one decreases the waiting time of the shorter process **more** than the **increased** waiting time of the longer process
The **problem** is knowing **how long** the next CPU burst will be.
- Can only **estimate**
- Pick the process with the **shortest predicted** next CPU burst
#### Exponential Moving Average

![[Pasted image 20231122093840.png]]

![[Pasted image 20231122093855.png]]
#### SJF: Two versions
Can be either **preemptive** or **non-preemptive**
- New **shorter** process arrives when a process is already being executed
- **Preemptive** SJF - Switch to newly arrived process
- **Non-preemptive** SJF - Allow currently existing job to finish

**Preemptive SJF Example**:

![[Pasted image 20231122094746.png]]

However there is an **overhead** with context switching every time for pre-empting jobs
### Priority Scheduling
SJF is a **special case** of **priority scheduling**
- A priority is associated with each process and CPU is allocated to the **highest** priority process
- Priorities can be indicated by numbers
- In SJF, priorities are the next **CPU burst times**

One **problem** with priority based scheduling is **starvation**
- Very **low priority** processes may **never** get scheduled
A solution to this is **aging**
- Gradually **increase** the priority of processes that **wait** in the system for a long time
### Round Robin (RR) Scheduling
Each process gets a **small unit** of CPU time (**time quantum** $q$)
- **After** this time has **elapsed**, the process is preempted and added to the **end** of the **ready queue**
- Generally, the scheduler visits the process in the order of their **arrivals**
- If there are $N$ processes in the **ready queue** and the time quantum is $q$, then **each process** gets $1/N$ of the CPU time in chunks of at most $q$ time units at once
- No process **waits** more than $(N-1)*q$ time units for its next turn

RR is **preemptive**
- Time **interrupts** every quantum to schedule next process
**Performance**
- $q$ large - FCFS
- $q$ small - too many **context switches**
$q$ is usually between 10ms and 100ms (context switch <10$\mu$s)

![[Pasted image 20231122095821.png]]