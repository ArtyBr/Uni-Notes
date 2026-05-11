Threads are useful because **thread** creation has **less overhead** than **process** creation
- A thread shares code, data, and files with the process which creates them
### What is a thread?
A **thread** is a **unit** of CPU execution
- **Single-threaded process**: **One chain** of execution running each line **sequentially**
- Fine if we have to perform a **single task**

![[Pasted image 20231025090911.png]]

What if we have **multiple** **similar** tasks?
- One solution is to run the **same** function in a **loop** for each task - still single-threaded
- To achieve **concurrency**, create a **separate** process for each task. But not efficient
	- Each process requires its **own address space** in memory
	- Code, data could be shared

We can make a process **multi-threaded**
- Each thread can perform a **separate** task
Threads share more things with their parent than processes do
- e.g. *code, data, heap, opened files, signals etc.*
Each is **comprised** of:
- A **thread id**
- **Program counter**
- A **register set**
- **Stack**

![[Pasted image 20231025091204.png]]
### Where are threads used?
Most modern applications are **multithreaded**
**Separate** tasks can be implemented with **separate** threads
- *Update display*
- *Responding to keystrokes*
- *Spell checking*
#### Multi-threaded web server
Multithreading is a common approach to server architectures
- A busy webs server may have **thousands** of clients **concurrently** accessing it
- If it ran as a **single thread** it would only be able to process **one** request at a time

![[Pasted image 20231025091722.png]]

- Server **listens** to client **request** call
- **Hands over** process to **thread**, which processes the client
- Server can now **listen** for **another** client request without having to process the last request
- Much **quicker** than single-threaded server
### Benefits of Threads
- **Economy** - Cheaper than process creation, thread switching lower overhead than context switching
- **Scalability** - Large number of concurrent tasks
- **Responsiveness** - May allow continued execution of one thread even when another thread is blocked
- **Resource sharing** - Threads share resources of process, easier than shared memory or message passing
### Concurrency vs Parallelism
**Concurrency** supports more than one task making progress 
- A single CPU system may appear to be running tasks **concurrently** by **interleaving** their **execution** 
	- In single core - interleaving the tasks like this make it **seem like** multiple tasks are making progress simultaneously
- Parallelism implies that a system can perform **more than one** task **simultaneously**
- It is possible to have concurrency **without** parallelism

![[Pasted image 20231025092729.png]]

In a **single core** system we can have **concurrency** but **not parallelism**
In a **multi core** system we can have **both**
#### Data parallelism and task parallelism
- **Data Parallelism** - Distributes subsets of the same data across **multiple cores**, performing the **same** operation on each core. 
	- E.g. Summing the contents of an array
- **Task Parallelism** - **Splits** threads performing **different** tasks across **multiple cores** 
	- E.g. Performing two different statistical calculations on an array
Applications can have a **mix** of **both** types

![[Pasted image 20231025093203.png]]

##### Counting speedup: Amdahl's law

![[Pasted image 20231025093325.png]]

### Pthreads
A POSIX standard (IEEE 1003.1c) API for thread creation and management 
- Specification, not implementation. 
- May be provided either as user-level or kernel-level 
- Common in UNIX operating systems (Solaris, Linux, Mac OS X)

![[Pasted image 20231025093732.png]]

![[Pasted image 20231025093742.png]]

Take a number $N$ as input from the use and compute the sum from $1$ to $N$
### Thread synchronisation
- When multiple threads write to the **same location**, we must **synchronise** them
- **Synchronisation** ensures that one thread does not **overwrite** the **contents** written by **another thread**

##### Example:
We would like to perform
![[Pasted image 20231029190405.png]]
But we would like to do so by running two threads in **parallel** each thread doing **half** of the summation

![[Pasted image 20231029190438.png]]

![[Pasted image 20231029190450.png]]

Steps:
- First copies the value of **sum** into **register**
- Adds the value of $i$ to **register**
- Copies the value from **register** back to **sum**

![[Pasted image 20231029190505.png]]

These operations can occur in **any random sequence**, so you can't predict when they will occur.
- In this example, the **first 2 steps** of **thread 1** occur, then **all steps** of **thread 2**, then the **final step** of **thread 1**
- This means **thread 1**, in the end, overwrites the contents prescribed to **sum** by **thread 2** when it writes its value of register1 to **sum** 
#### Race condition
Threads engage in a **race** to become the last one to write on the shared variable `sum`
- Only **one** of the values is **preserved**
- **Race conditions** should be avoided by proper synchronisation between the threads
#### Mutex (mutual exclusion) locks
Each thread must first acquire a **lock** to perform **updates** on **shared variables**

![[Pasted image 20231029190713.png]]

However, when implementing this in the program it can become **much slower** than even using a single thread
- We are essentially **serializing** the program (making it work the same as a single thread)
- **And** **tripling** the number of operations with the locking operation

**Solution**:
- Implement a **local variable** in the for loop which is updated **without locking**, then only doing the lock **once** **outside the loop** when adding the **temporary variable** to the **global one**
### Models of multi-threading
#### User level and kernel level threads
**User-level threads**
- Exist within **user processes** if they are multi-threaded
- For a user-level thread to **execute** on a CPU, it must be **associated** with a **kernel-level thread**

![[Pasted image 20231030094022.png]]

**Kernel level threads**
- **Kernel** **itself** may be multi-threaded
- Some kernel threads provide **services** to the users and others are used to **run** user **processes**
- Kernel can **schedule** them on different CPUs
#### One-to-One model
**Each** user-level thread **maps** to a **kernel thread**
*Linux, Windows*

![[Pasted image 20231030094537.png]]

**Advantages**
- Kernel is **aware** of all the threads running within the user process
- Hence, the user process can **completely rely** on the kernel to **manage** (*synchronise and schedule*) its threads
**Disadvantages**
- **Creating** each user-level thread involves the kernel.
	- **More expensive**
- User process is **limited** by **thread management policies** implemented by the OS
#### Many-to-One model
**All** user-level threads **within a process** are **mapped** to a **single** kernel thread

![[Pasted image 20231030094610.png]]

**Advantages**
- **Less overhead**
- **More flexibility** in thread management
	- Done by user level **thread library**
**Disadvantages**
- **Multiple** user-level threads cannot run in **parallel**
- **One** blocking user-level thread causes the **whole process** to **block**
#### Many-to-Many model
**Mixture** of *One-to-One* and *Many-to-one* models
- Allows user-level threads to be **multiplexed** onto **smaller** or **equal number** of kernel level threads

![[Pasted image 20231030094847.png]]

**Advantages**
- Kernel threads can run in **parallel**, blocking call by one user-level thread does **not block** the **entire process**
- Programmers can **decide** how many kernel threads to use and how many user level threads should be **mapped** to **each one**
	- **Less overhead**
## Threading strategies
A **threading strategy** determines the interaction **among** threads **within a process**
- Here we consider two threading strategies:
### One thread per request strategy
Server creates a **separate thread** to handle **each** client **request**

**Advantages**:
- Client **doesn't** need to **wait** for other threads to finish for their request to be **processed**
**Disadvantages**:
- **High overhead** for each thread
- **High traffic** will create **large number of threads**, *slowing down* the system by burdening it
	- Since **each** thread takes **resources** from the system

![[Pasted image 20231031141141.png]]
### Thread Pool
Creates a queue of threads **before** the client arrives, and assigns clients to the queue upon receiving requests

- **Main thread** in the system starts running **first**
- Main thread creates a *pool* of workers **before** it receives any connection requests
- As long as the server program is **running** **all threads** in the pool **exist**
- ![[Pasted image 20231031141519.png]]
- **New** connection requests get **enqueued**
- As worker threads become **freed** up they **dequeue** the **pending requests** and **process** them

**Advantages**:
- Usually **faster** to **service** a request with an **existing** thread than creating a new thread
- Allows the number of threads in applications to be **bound** to the **size** of the **pool**
	- This size is dictated by the programmer taking into account the **resources** of the server
**Disadvantages**:
- A request may have to **wait** in a queue if **all** workers are **busy**
	- However probability of this is small with enough threads
- Deciding the **number** of workers in the thread-pool is **not easy**
	- May have to be **dynamically adjusted** depending on the **load**
##### Main server thread
```c
int main(){ …… 
	for(i=0; i<NUM_THREADS;i++){ // create worker threads
		pthread_create(tid[i], NULL, handle_connection, NULL);
	 }
	 /* Create TCP socket, bind to specific IP and port,
	 and start listening on the port */
 …
	 // keep accepting connections and adding them to queue
	 while(1){
		 conn_sock=accept(…,…,…);
		 pthread_mutex_lock(&queue_mutex);
		 enqueue(work_queue, conn_sock);
		 pthread_mutex_unlock(&queue_mutex);
	 }
 }
```
##### Worker threads
```c
void *handle_connection(void *arg){ 
	…… 
	while(1){ 
		if(!empty(work_queue)){
			pthread_mutex_lock(&queue_mutex);
			dequeue(work_queue, conn_sock);
			pthread_mutex_unlock(&queue_mutex); 
			/* process conn_sock */ 
			… 
		}
	}
}
```
- The worker keeps on **checking** the queue even when there is **no work** to be done
- **Wastage** of **CPU cycles** + **energy**
#### Condition variables
Help solve the problem of **busy wait**
- Used to **synchronise** the **actions** of different threads
A thread can use `wait(&cond_var, ...)` to **wait** on a **condition variable**
- Until some **other** thread **signals** the variable using `signal(&cond_var)` or `broadcast(&cond_var)`
	- `signal()` only wakes up **one** thread
	- `broadcast()` wakes up **all** threads

![[Pasted image 20231031143345.png]]
## Signal Handling
Types of signals:
- **Synchronous** - **Internally** generated by a process 
	- e.g. *division by 0, illegal memory access*
- **Asynchronous** - **Externally** generated
	- e.g. *Terminating process using `ctrl+c` send signal `SIGINT`*

**All** signals, whether synchronous or asynchronous, follow the **same pattern**:
- A signal is **generated** by a particular **event**
- A signal is **delivered** to the process to which it applies
- Once delivered the signal must be **handled**

Signal is **handled** by one of two signal handlers:
- **Default** - Every signal has default handler that kernel runs when handling signal
	- e.g. *for `SIGINT` the kernel terminates the process*
- **User-defined** - Can **override** default

![[Pasted image 20231031145120.png]]

**Signal safe** operations:
- Can't use something like `printf` as **not** signal-safe
	- Gives errors
#### In a **multi-threaded program**:
Signal can be **delivered** to:
- **All** threads
	- Some **asynchronous** signals like `SIGINT` should be sent to **all** threads
- **Specific** threads
	- **Synchronous** signals are generally delivered to thread **generating** the signal
	- To deliver to specific thread:
		- `pthread_kill(pthread_t tid, int signal)`


