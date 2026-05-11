Processes running concurrently may want to communicate with each other
There are two fundamental models of interprocess communication
- **Shared memory** - A region of memory that is shared by communicating processes is established
- **Message passing** - Messages are exchanged between the communicating processes
#### Short comparison
- Both methods are commonly implemented in operating systems
- **Message passing** is useful for passing **smaller** amounts of data as no conflicts need to be avoided. It is more easily implemented in **distributed** systems
- **Shared memory** can be **faster** as message passing systems are typically implemented using system calls, which rely on kernel intervention - less **overhead**
### Producer-consumer paradigm
A process can be both a producer and a consumer - something that both produces information or consumes the produced information

- The shared buffer/memory space can be filled by the producer and emptied by the consumer
- **Consumer** must wait when the buffer is **empty**
- **Producer** must wait when the buffer is **full**

![[Pasted image 20231010103723.png]]

##### Pseudo-code example of both:
###### Producer:
```c
item next_produced; 
while (true) { 

	/* produce an item in next produced */
	while (((in + 1) % BUFFER_SIZE) == out) ; 
		/* do nothing */ 
		
	buffer[in] = next_produced; 
	in = (in + 1) % BUFFER_SIZE; 
}
```
###### Consumer:
```c
item next_consumed; 
while (true) { 

	while (in == out) ; 
		/* do nothing */ 
		
	next_consumed = buffer[out]; 
	out = (out + 1) % BUFFER_SIZE; 
	
	/* consume the item in next consumed */ 
}
```
### Shared Memory
- Processes interact by writing to or reading from a shared part of the memory
- Kernel is involved only in establishing the shared part of memory; communication is entirely handled by the communicating processes - **overhead is minimised**

![[Pasted image 20231010103241.png]]

- Shared memory resides in the **address space** of one of the communicating processes
- Other processes need **permission** to access it
- **Kernel** is required to setup shared memory and grant necessary permissions
- Once shared memory is established, processes are responsible for maintaining proper syncronisation - i.e. don't write when the memory is full
#### `mmap`
- A parent can give data to the child using `mmap` - this creates a shared block of memory in the **heap** of the **parent** process.
- `mmap` works like `malloc` or `calloc` which returns the **base** address of the block of memory assigned to the shared memory
- `mmap` is more primitive than `malloc` or `calloc` - you have more control over the properties of the assigned memory space

![[Pasted image 20231016093004.png]]

```c
Output:
Child: 20
Parent: 20
```
Despite the fact that the parent and child usually have **separate** address spaces, here the variable `ptr` is shared between the parent and child and so both have access to its value
### Message Passing
A message passing system should at least provide two operations:
- **send(message)**
- **receive(message))**
These are generally implemented using system calls

![[Pasted image 20231010104015.png]]

Implementation of send() and receive() depends on:
- **Link implementation**: Direct or Indirect
- **Synchronisation** between send() and receive()
- **Buffer size** representing the communication link
#### Link implementation
##### Direct communication
Processes must name each other explicitly:
- **send(P, message)**
- **receive(Q, message)**
Properties of communication link:
- Links are established **automatically**
- A link is associated with exactly **one** pair of communicating processes
- Hardcoding the processes identifier may not be ideal since every time a process is run its id can change
##### Indirect communication
Messages are directed and received from **mailboxes** (aka **ports**)
- Each mailbox (port) has a unique id
- Both the sender and receiver need to know this unique id
Properties of communication link:
- Link established only if processes share a **common** mailbox
- A link may be associated with **many** processes
	- OS needs to make sure correct receiver gets the correct message
- Each pair of processes may share several communication links
#### Synchronisation
**Blocking** is considered synchronous
- Blocking send - the sender is blocked until the message is received
- Blocking receive - the receiver is blocked until a message is available
**Non-blocking** is considered asynchronous
- Non-blocking send - the sender sends the message and continues
- Non-blocking receive - the receiver receives a valid message or a null message
Different combinations of send and receive are possible
You might use a nonblocking send() in combination with a blocking receive()
- This allows the sending process to keep working without waiting for the receive to process the message
#### Buffering
Communication link is a **buffer**. Implementation of send() and receive() depends on the **capacity** of this buffer
- **Zero capacity** - the queues has a maximum length of zero, so sender must block until recipient receives the message, as soon as they send just 1 message
- **Bounded capacity** - The queue has a finite length, when full the sender must be blocked
- **Unbounded capacity** - The queue's length is potentially infinite; sender never blocks
#### Ordinary Pipes
Ordinary pipes in UNIX allow simple **one-way communication** through message passing
- A pipe connects the **output** of one process to the **input** of another process
- In UNIX systems, pipes are commonly created in bash using `|`

- Ordinary pipes **only** exist while the processes are **communicating**, they cease to exist when the processes **exit**
- Ordinary pipes are **not** accessible **outside** the process which **created** it
###### Pipe between 2 children
![[Pasted image 20231016093434.png]]
###### Pipe between parent and child
![[Pasted image 20231016094156.png]]

`write` syntax - write(**file description** - read/write [0/1], **pointer** to **start** of where to read from, **how many** bytes to read **from here**)

![[Pasted image 20231016094226.png]]
#### Named pipes
**Named pipes** are much more powerful

![[Pasted image 20231016094250.png]]

- Looks very similar to a **file**
	- **Created** and **works** very similar to how a file is created/works
- `mkfifo` creates the pipe - once created, it sits in the system just like a file would
- Kernel recognises it as a pipe - `prw-r--r--` the `p` at the start distinguishes it as a pipe
- In the case above, when `./prime 32 ... 8 > namedpipe` is 'run', it is **blocked** as no process is accessing `namedpipe`
	- It will only run after you hit enter on `cat namedpipe` - both processes run at the same time at this point
- Same file has to be opened in both the **read** and **write** mode
- Named pipe will **still exist** in the system as a 'file' - can be reused in the system again