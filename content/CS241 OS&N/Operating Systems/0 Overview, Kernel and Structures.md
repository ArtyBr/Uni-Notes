An OS is a software acting as an **intermediary** between the **user** of a device and the **hardware** of the device

Hardware devices include:
- CPU
- Memory
- I/O devices
- Storage

The OS is responsible for **allocating these resources** to user processes and **control their execution**

**Goal**: **Avoid failures and errors**
- Error - e.g. two programs attempting to write at the same memory location simultaneously

*Structure*

![[Pasted image 20240324121847.png]]

The system must be able to:
- **Load** a program into **memory**
- **Execute** the program
- **Stop** the program

The OS provides **system calls** to allow the user to perform actions that interact with hardware

*Communication*

Some processes need to **communicate** with other processes - this is done by **shared memory** or by **message passing**

![[Pasted image 20240324122104.png]]

*Error handling*

The OS also needs to **handle errors** in cases of **hardware** failures or **user program** failures
- Handled by either invoking an **error handling routine** or **shutting down the process** causing the error

*Resource allocation*

When there are multiple **users** or multiple **jobs** running at the same time, resources must be **allocated** to each of them
- In scheduling CPU jobs the OS must consider the **speed** of the processor, **number of processors** available, the **jobs needed to be executed** etc.

*Accounting*

OS needs to keep track of **which** processes are running and **how much computing resources** they consume
- Needed for **system administration** or **billing purposes** etc.
- The OS gathers this information through **process control blocks** (PCBs)

*Protection and Security*

When separate processes execute **concurrently** they should not be able to interfere with **other processes** or with the **OS itself**
- Security from outside attacks is also important:
	- **Encryption**
	- **Authentication**

#### Kernel
The kernel is the **core** of an OS
- Loaded into the **main memory** at system startup
- It is the process running at **all times** on the computer
- Functions **only a kernel** can perform
	- Memory management
	- Process scheduling
	- File handling
	- etc.

**Kernel space** is the part of the memory where the **kernel** **executes**
**User space** is the section of memory where **user processes run**
- Kernel space is kept **protected** from the user space
- Kernel space can be **accessed** via user processes through **system calls**

When a user processes requires a service from the kernel, it invokes a **system call**
- e.g. reading from a file or writing to a file
System calls are **required** since user processes cannot perform certain **privileged operations**
- System calls are **low-level functions** provided by the OS
- They provide a consistent interface for common operations

**Dual mode operation** is a mechanism to distinguish between OS and user operations
- Hardware operates in two modes:
	- **User mode**
	- **Kernel mode**
- **Mode bit** (0 or 1) indicates kernel or user mode
- Some instructions designates as **privileged** - only executable in **kernel mode**
	- **System calls** by a user asking the OS to perform some function changes from **user mode** to **kernel mode**

![[Pasted image 20240324154257.png]]
#### Structures
A common approach for OS's is to partition the task into **small components**, rather than have a huge monolithic system

*Monolithic system*

Early OS's did not have well-defined structures - **all functionalities were built into the kernel**.
- This resulted in a **monolithic kernel**

- Monolithic kernels make it **difficult to debug the kernel mode**
- But the **benefit** is that there is **little overhead** in the **system call** interface

![[Pasted image 20240324154618.png]]

Early OS's also suffered due to their hardware
- Didn't support **dual mode operation**

*Layered approach*

**Dependencies** between different parts of the kernel code can be **reduced** using a **modular kernel design** approach
- Modular design can be achieved through **separation** of **different layers**
- The **bottom layer** is the **hardware** and the **top layer** is the **user**
- Layer $K$ **uses** the service of layer $K-1$ and **provides** service to layer $K+1$

**Pros**
- Simplicity of **construction**
- Ease of **debugging**
- Clear **interfaces** between layers

**Cons**
- **Defining** layers is difficult
- **Efficiency** - system calls access multiple layers to execute which **adds overhead**

![[Pasted image 20240324155403.png]]

*Microkernels*

**Remove** all **non-essential** components from the kernel and implement them as either **system** or **user-level** programs
- This results in a **smaller kernel**
	- Microkernels provide minimal process and memory management and inter-process communication

![[Pasted image 20240324160054.png]]

**Pros**
- **Extending** the operating system is easy
- **Security** and **reliability** (services are running in the user space)

**Cons**
- **Performance** hampers due to **increased system call overhead**

*Loadable Kernel modules*

**Most modern approach**
- Kernel provides **core services** while other services are implemented **dynamically** as the kernel is running

![[Pasted image 20240324160301.png]]

