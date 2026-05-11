#### Process Creation
- Every process is the **child** of another process, and has a parent process which created it
- The **process structure** forms a **tree** with an initial process (with PID=0) at the root

An OS must provide ways for users to create new processes 
e.g.
- By clicking an icon
- Typing a command
A system process *(parent)* creates a user process *(child)*

What if a user process wants to create another user process?
- The OS provides **system calls** for a user process to create another user process
In UNIX-based OS's, processes can be created using the **fork()** system call
##### fork()
The UNIX fork() command creates a **direct replica** of the **parent** process which becomes the **child** process.
- If fork()  is **unsuccessful**, then it returns a **negative** value
- If **successful** fork() will **return**:
	- 0 for the **child**
	- The positive pid of the child for the **parent**
- fork() will create a child process which is **identical** to the parent
The new process will have a **new** memory address space, and so **global and local variables** of the parent are **separate** from those of the child.

``` c
int main(){
	pid_t pid;
	pid = fork(); // child process is created with a copy of the address space
	if (pid < 0){ // unsuccessful fork
		printf("Error: Child could not be created")
	}
	if (pid == 0){ // pid of the child will be 0
		execlp("./program", "program", NULL, NULL) //execlp overwrites the code of the child, and so it will not execute any more code from the parent anymore
	}
	if (pid > 0){ // pid of the parent will be the pid of the child
	printf("I am the parent");
	
	}
}```
#### Process termination
- Processes terminate automatically after executing their last statement
- A process can also be terminated using the **exit()** system call
- Returns a status value (exit code) to parent
- All resources of the process are released by the OS

- After a **child** has been terminated and before its exit status is collect, it's said to be a **zombie process**
- During this phase all resources of the child are released but its entry still remains in the process table
- Once the parent receives the exit status, the entry is released from the process table

A parent may terminate the execution of child processes using the **abort()** system call
Usually done if:
- Child has exceeded its allocated resources
- Task assigned to child is no longer required
- The parent is exiting and the OS does not allow a child to continue if its parents terminates; **Cascading termination**

**Orphan** (parent finishes before the child) processes are assigned a parent of **init** and so orphan processes don't exist in UNIX for a long time