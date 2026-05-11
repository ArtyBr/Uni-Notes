- A program is a **passive** entity, stored on the disk
- A process is an **active** instance/execution of a program, stored in memory
- **One** program can lead to **several** processes
### A process in memory
Made of different attributes:
- **Text** - Contains the instructions for the process
- **Data** - Contains the global variables, accessible by everything in the process
- **Heap** - Dynamically allocated memory
- **Stack** - Contains the local variables and parameters (e.g. return address) of functions being executed at a time
- The space between the Stack and the Heap allows them to grow or shrink during the program run-time

![[Pasted image 20231005112039.png]]
#### Process state changes
A process goes through the following states in its lifecycle

![[Pasted image 20231005103705.png]]

**New** - Process is being created
**Ready** - Process is waiting to be assigned to a processor
**Running** - Instructions are being executed
**Waiting** - Process is waiting for some event to occur
**Terminated** - Process has finished execution
### Process Control Block
A process' information is contained in a **data structure** called the *"process control block"*
- This is a **kernel-level** data structure - used to track and manage a process
This consists of:
- **Process state** - Running, waiting, ready etc.
- **Process number** - Used by OS to identify the process
- **Program counter** - Stores the location of which instruction should be executed next
- **CPU scheduling information** - Priorities, scheduling queue pointers
- **Memory management information** - Memory allocated to the process
- **Accounting information** - CPU used, time since start
- **I/O Status** - list of open files, I/O devices etc.

![[Pasted image 20240401161200.png]]

This is how the CPU switches between processes

![[Pasted image 20231005105517.png]]

Known as a **context switch**
#### Context switch
When the system decided to switch to another process:
- It saves the state of the current process in the process control block
- It loads the saved state of the new process
Context switching is an **overhead** - the CPU isn't doing anything useful while switching
- More complex OS and PCB (process control block) - Longer context switches
So there is a balance to keep between good **time-sharing** (allowing processes to run while others are waiting) and **lower overhead** due to context switches being an overhead

