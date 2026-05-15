Before optimising and generating code, we need to **understand** the **target environment** to which we are aiming to optimise for an generate code for.
- Runtime organisation is also the **abstraction** that a compiler sees when reasoning about the execution of the program
- This abstraction sits *on top* of the OS, which does the **actual management** of the **physical resources** (memory allocation etc.)

The compiler *assumes* that the executing target program runs in its *own* **logical address space** in which each program value has a location
- The operating system *maps* the **logical addresses** into **physical addresses**, which are usually spread out throughout memory

When a program is invoked:
- The OS **allocates** a **memory block** for the program
- Loads the code to that memory space
- Executes a jump to the **entry point** of the program

The compiler is responsible for:
- Deciding what the **layout of the data** used by program should be
- Generating the code that **correctly manipulates the data**

There are references to the data in the code, thus:
- Code generation and layout of data needs to be designed together

---

The compiler assumes memory for a program comes in **blocks of contiguous bytes**
- Byte = 8 bits, Word = 4 bits
- Multibyte objects are stored in consecutive bytes and given the address of the first byte

![[Pasted image 20260513105546.png]]

- **Code** - *static* - Generated target code. Fixed size, determined at compiled time
- **Static** - *static* - Program data with size that can be determined at compile time (global constants)
- **Heap** - *dynamic* - Data that may *outlive* the call to the procedure that created it
	- e.g. memory allocated/dealocated at runtime (via `malloc() or free()`)
- **Stack** - *dynamic* - Data *local* to the procedure
	- Also stores data structures called **activation records** that get generated during procedure calls - gets created and destroyed at runtime

We say that data is **word aligned** if it *begins* at a **word boundary**

![[Pasted image 20260513105956.png]]

Most machines have some alignment *restrictions*, or *performance penalties* for poor alignment

![[Pasted image 20260513110042.png]]

Padding is not part of the string, it's just **unused memory**
- When space is at a premium, a compiler may *pack data* so that no padding is left (but this would decrease runtime performance)
- Having aligned memory becomes further important in some compilers if it wants to do optimisations such as **automatic vectorisation**

The two adjectives **static** and **dynamic** distinguish between **compile time** and **run time**
- Compile time - storage allocation made by the compiler is called **static** if it is made *only* by looking at the **program text**
- Run time - Storage decision is **dynamic** if it can be decided *only* while the **program is running**

Compilers use some combination of the following **two strategies** for **dynamic storage allocation**
- **Stack storage** - Stores data local to a procedure and support the normal call/return policy for procedures via **activation records**
- **Heap storage** - Stores data that may *outlive* the call to the procedure.
	- Allows objects/other data elements to, at runtime, obtain storage when they are created and to return that storage when they are invalidated.
	- Heap management also involves **garbage collection**

The compiler makes a couple **assumptions** about the **stack allocation of space**
1. *Execution* is **sequential**
	- Control moves from one point of the program to another in a **well defined order**
	- Implied *no* **concurrent** or **parallel** execution - i.e single thread of execution
2. When a procedure is called, control **always returns** tot he point **immediately after the call**
	- *No exceptions* (e.g.)

Procedures, subroutines, functions or methods used as units of user-defined actions are managed at least in part in run-time memory as a **stack**

When a procedure is called:
- Space for its **local variables** is *pushed* onto a stack
- When the procedure **terminates**, that space is *popped off* the stock

Allows space (the stack) to be **shared** by procedure calls whose durations *do not overlap* in time
- Can compile code for a procedure so that the relative address of its nonlocal variables are *always the same*, regardless of the sequence of procedure calls
- Stack allocation is made possible by procedure calls, or activations of procedures **nesting in time**

Example of **nesting time**:

![[Pasted image 20260513110922.png]]

Lifetimes of procedure activations are **properly nested** (fully contained)
- Thus, we can represent the **activations of procedures** during the running of an entire program by a tree, called an **activation tree**

Example for quicksort:

![[Pasted image 20260513111114.png]]

The activation tree *depends* on the **runtime behaviour** of the program
- The activation tree may be *different* for every **program input**
- We can see how a **stack** can be used to *keep track* of the **currently active procedure** in this properly nested activation tree:

![[Pasted image 20260513111225.png]]

Procedure calls and returns are usually managed by a run-time stack called the **control stack**
- Each **live activation** has an **activation record** (aka a *frame*) on the control stack
- The activation record of the procedure where control currently resides has its activation record at the top of the stack

Within this, we have:
- **Actual parameters** - used by the calling procedure
	- Usually these are placed in registers for greater efficiency, but included here for generality
- **Return values** - values returned by the calling function, if any.
	- Sometimes values will be placed in a register for efficiency
- **Control link** - pointing to the activation record of the caller
- **Access link** - To locate data needed by the called procedure but found elsewhere (e.g. another activation record)
- **Saved machine status** - Information about the state of the machine *just before the call to the procedure*
	- Includes the **return address** (value of the program counter), contents of **registers** that were used by the calling procedure which must be restored when the return occurs
- **Local data** - belonging to the procedure whose activation record this is
- **Temporaries** - such as those arising from the evaluation of expressions, in cases where those temporaries cannot be held in registers.
In that order:

![[Pasted image 20260513115101.png]]

We can see a snapshot of the downward-growing runtime stack:

![[Pasted image 20260513114948.png]]

---

Procedure calls are implemented by:
- a **Calling sequence** - code that *allocates* an **activation record** on the stack, enters information into its fields
- A **Return sequence** - code to *restore* the **state** of the machine so the calling procedure can continue its execution after the call

The code in a calling sequence is divided between the **calling procedure** (the *caller*) and the **called procedure** (the *callee*)

Example Calling sequence:
1. The caller evaluates the **actual parameters** - place them *on top* of **its own activation record**
2. The caller stores a **return address** and the **old value** of `top_sp` into the **callee's activation record** 
3. Caller *increments* `top_sp` past the caller's **local data** and **temporaries** and the callee's parameters and status fields
4. The callee saves the **register values** and other **status information**
	- A different calling sequence may entail the caller saving the register values instead
5. The callee initialises its **local data** and begins execution

![[Pasted image 20260513115609.png]]

We can see the division of runtime tasks between the caller and callee:

![[Pasted image 20260513115635.png]]

![[Pasted image 20260513115652.png]]

Example return sequence:
1. The callee places the **return value** *next to the parameters*
2. Using information in the machine-status field, the callee *restores* `top_sp` and other **registers**
3. Callee *branches* to the **return address** that the caller placed in the **status field**
4. Now the `top_sp` has been decrement, but the caller knows where the **return value** is, relative to the current value of `top_sp`
5. The caller may use that value to get the return value

![[Pasted image 20260513115836.png]]

**Caller-saves** - Before a function is called, the caller *saves* all values in *registers* that must be preserved
**Callee-saves** - The called function saves the contents of the registers that need to be preserved and restores these immediately before the function returns
- It may not be necessary to store all registers that may potentially be used to hold variables
- Save only the registers that the function actually *uses* to hold its local variables

So we can refine both:
- **Caller-saves** - save *only* the registers that **hold live variables**
	- That is **variables live** *after the function call*
	- **Caller** saves these variable *before the function call*
	- Disadvantage - save a live variable on the stack even though the callee does not use the register that holds this variable
- **Callee-saves** - save *only* the registers that the **function actually uses** in its body
	- That is **registers** used in the called **function's body**
	- **Callee** must save the values in these registers *before using* the registers within its body
	- Disadvantage - might save some registers that do not actually hold live values
*Cannot avoid* these unnecessary saves - each function is compiled independently and hence does *not know* the **register usage** of their callers/callees
- Can mix these to optimise

---

Memory for data **local to a procedure** whose size *cannot be determined* at **compile time** may be allocated on the **stack**
- Memory allocated on the stack becomes **inaccessible** (and freed) when the **procedure returns**
- Allocating on the stack (vs the heap) avoids the expense of **garbage collection** of heap space

Let's assume a procedure $p$ has three local arrays, whose sizes cannot be determined at compile time
- Activation record of $p$ does not hold storage for these arrays - only a pointer to the beginning of each array appears in the activation record itself
- When $p$ is executing, these pointers are at known offsets from the top-of-stack pointer
The target code can access array elements through these pointers

![[Pasted image 20260513121152.png]]

The activation record for $q$ begins after the arrays of $p$
- Any variable-length arrays of $q$ are located beyond the activation record of $q$
- `top` - the actual top of the stack, points to the position at which the next activation record will begin
- `top_sp` - used to find local, fixed-length fields of the top activation records
- `top_sp` - points to the end of the machine-status field

The code to **reposition** `top` and `top_sp` can be *generated* at **compile time** in terms of sizes that will **become known** at runtime
- When $q$ returns, `top_sp` can be restored from the saved **control link** in the activation record for $q$
	- The new value of `top` is (the old un-restored value of) `top_sp` *minus* the **length** of the **machine-status, control and access link, return value and parameter** fields in $q$'s activation record
	- This length is known at compile time to the caller, but it may depend on the caller, if the number of parameters can **vary** across calls to $q$

A couple mechanisms for finding data used **within a procedure** that *does not belong* to the procedure
- **Static scope** (aka lexical scope) - find required data/declaration in **enclosing text**, usually in most closely nested scope
- **Dynamic scope** - leave decision to run time (dynamic binding), at which point look for **closest activation record on the stack** that has the required data/declaration

New concept:
- The **lifetime** of a variable $X$ is the *portion* of the *execution* in which $X$ is *defined*
- Lifetime is a **dynamic** (runtime) concept

There are two cases in modern programming languages:
1. Data access *without* nested procedures
	- All variables are defined wither **within a single function** or outside any function, i.e. *globally*
	- No procedure declarations within another procedure - i.e. no procedures whose scope is entirely within another procedure
	- A global variable $V$ has a **scope** *consisting of all the functions* that *follow the declaration*
	- Variables declared within a function have a scope consisting of that function only, or port of it **if the function has nested blocks** that *re-declare* the **same name** at any depth
2. Data access in languages that *support* nested procedures
	- Can consist of **nested function declarations** - function declared *within* a function
	- Functions can take **functions as arguments** and *return* **functions as value**

**Global variables** are allocated **static storage**
- Locations remain **fixed** and **known** at compile time
- Simply use the statically determined address to access any non-local variable to the current executing procedure
Any *other name* **must be local** to the activation at the top of the stack
- Access *these* variables through the `top_sp` pointer

---

**Access links**

Scoping rules for **nested functions** can be implemented by adding a pointer called the **access link** to each activation record
- If procedure $p$ is nested *immediately within* procedure $q$ in the source code, then the access link in any activation of $p$ points to the most recent activation of $q$
- The **nesting depth** of $q$ must be **exactly one less** than the **nesting depth** of $p$
- Access links form a **chain** from the activation record at the top of the stack to a sequence of activations at progressively lower nesting depths
- Along this chain are all the activations whose data and procedures are accessible to the **currently executing procedure**

*Assume* procedure $p$ at the top of the stack is at **nesting depth** $n_{p}$
- $p$ needs access to $x$, and elemnt defined within some procedure $q$ that *surrounds* $p$ and at nesting depth $n_q$
- To **find** $x$, *start* at the **activation record** for $p$ at the top of the stack and *follow* the access link $n_{p}-n_{q}$ times, from **activation record** *to* **activation record**
- Finally, we wind up at an activation record for $q$, and it will always be the **most recent (highest) activation record** for $q$ that currently appears on the stack, this will have the declaration of $x$
- As the compiler knows the layout of activation records, $x$, will be found at **some fixed offset** from the position of $q$'s activation record that we cab *reach* by *following* the **last access link**

![[Pasted image 20260513123321.png]]

![[Pasted image 20260513123349.png]]

Lexical scope rules apply *even when* a nested procedure is **passed as a parameter**

![[Pasted image 20260513123659.png]]

![[Pasted image 20260513123707.png]]

---

**Displays**

The access link approach to nonlocal data is **inefficient** if the **nesting depth** gets **too large** - need to follow a long chain of links to reach the data required
- Faster access to nonlocals can be obtained using an array `d` of **pointers** to **activation records** called a **display**
- `d[i]` is a pointer to the **highest activation record** on the stack for any procedure at nesting depth `i`
If procedure `p` is executing, and it needs to access element `x` belonging to some procedure `q` then:
- Look (only) in `d[i]`, where `i` is the **nesting depth** of `q`
- Follow the pointer `d[i]` to the **activation record** for `q`
- `x` is found at a **known offset** within that activation record
The compiler *knows* what `i` is, so can generate code to access `x` using `d[i]` and the offset of `x` from the top of the activation record for `q`
- The code never needs to follow a long chain of access links

Example for quicksort:

![[Pasted image 20260513124051.png]]

![[Pasted image 20260513124120.png]]

![[Pasted image 20260513124132.png]]

![[Pasted image 20260513124141.png]]

---

**Dynamic scope**
Under **dynamic scope**, a new activation *inherits* the **existing binding** of nonlocal names to storage
- A nonlocal name `a` in the called activation refers to the **same storage** that it did in the calling activation
- New bindings are set up for **local name** of the called procedure: the names refer to storage in the new activation record

![[Pasted image 20260513171900.png]]

**Two strategies** for implementing dynamic scope, since compiler can't know which variable is being referred to until the program is actually running.
- This is because the levels of scope of the program may differ based on the actual execution of the program, so need a way to manage this

**Deep access**:
- Note that we get dynamic scope if access links point to the same activation records that control links do (i.e. up the calls stack of activations)
- Thus, *dispense* the **access links** and use the control link to search into the stack
- Look for the first activation record containing storage for the nonlocal name
- *Deep access* - the search may go **deep into the stack**, depth depends on the input to the program and can only be **determined at runtime**
- Deep access takes *longer* to access a nonlocal
- But, **no overhead** associated with beginning and ending an activation
- A more **straightforward implementation** given by deep access when functions are passed as parameters and returned as results
**Shallow access**:
- Hold the **current value** of each name in **statically allocated storage**
- When a **new activation** of a procedure `p` occurs, a local name `n` in `p` takes over the storage statically allocated for `n`
- The previous value of `n` can be saved in the activation record for `p` and must be restored when the activation of `p` ends
- Allows nonlocals to be **looked up directly**
- But **time** is taken to **maintain these values** when activations begin and end
	- (overhead)

---

**Parameter passing**

The usual methods of *communication* between procedures are through:
- **Nonlocal names**
- **Parameters** of the called procedure

**Actual parameters** - the parameters used in the **call** of the procedure
- The **actual value** that is passed into the procedure by a caller

**Formal parameters** - the parameters used in the procedure **definition/declaration**
- Identifier used in a procedure to **stand-in** for the value that is passed into the procedure by a caller
- When a procedure is called, the formal parameter is temporarily 'bound' to the actual parameter
- Bound to an actual value only as long as their **procedure is active**
- When a procedure **returns** to its caller, the formal parameters **no longer contain any value**

Several common methods of parameter passing:
- **Call by value**
- **Call by reference**
- **Copy-restore**
- **Call by name**
The result of a program can *depend* on the method used

Why are there so many methods of parameter passing?
- Differing interpretations of what an expression represents:
- `a[i] := a[j]`
- Expression `a[j]` represents a **value**
- Expression `a[i]` represents a **storage location** into which the value of `a[j]` is *placed*
The decision to use the **location** of the value is determined by the expressions appearance on the **left** or **right** of the assignment symbol (=)
- We say that the term:
- **l-value** refers to the **storage** *represented* by an expression
- **r-value** refers to the **value** *contained* in the storage
- The prefixes **l-** and **r-** come from the 'left' and 'right' side of the assignment

**Call by Value**
- The actual parameters are evaluated and their **r-values** are passed to the called procedure
	- i.e. place it in the activation record of the **called procedure**

Call by value can be implemented in the following way:
- Treat a formal parameter as a **local name**, so the **storage for the formal parameters** are in the activation record of the called procedure
- The **caller evaluates** the actual parameters and places their **r-values** in the activation record of the caller
In call by value, operations on the formal parameters **do not affect values** in the activation record of the caller

**Call by reference**
- The caller passes to the called procedure a **pointer to the storage address** of each actual parameter
	- If an actual parameter is a **name** or an **expression** having an l-value, then that l-value itself is passed
		- Remember an l-value means an address
	- If the actual parameter is an expression, like `a+b` or `2`, that has no l-value, then the expression is *evaluated* in a **new location** and the **address of that location** is passed

**Copy-Restore**
- A hybrid between call by value and call by reference

The following steps occur:
- Before control flows to the called procedure, the **actual parameters** are **evaluated**
- The **r-values** of the **actual parameters** are passed to the called procedure, as in call-by-value
- Additionally, the **l-values** of those **actual parameters**, having l-values, are determined before the call
- When control returns, the current **r-values** of the formal parameters are copied back into the l-values of the actuals, using the l-values computed before the call.
- Only actual parameters having l-values are copied

In other words:
- **"Copy in"** - the values of the actual parameters into the activation record of the called procedure (into the storage for the formal parameters)
- **"Copy out"** - the final values of the formal parameters into the activation record of the caller (into l-values computed from the actual parameters before the call)

**Call by name**
- The procedure is *treated* as if it were a **macro**
	- Its body is **substituted** for the call in the caller
	- With the actual parameters **literally substituted** for the formal parameters
	- Such a literal substitution is called a macro-expansion or in-line expansion

The local names of the called procedure are **kept distinct** from the names of the calling procedure
- We can think of each local of the called procedure being **systematically renames** into a **distinct new name** *before* the **macro-expansion** is done

The actual parameters are surrounded by parentheses if necessary to preserve their integrity

Additionally, we could do **Procedure in-lining** (inline expansion)
- This replaces a procedure call with the **body of the procedure** - similar to call by name, *but*:
	- Turns **parameter passing** and **result passing** into **assignments**
	- Manages **variable scoping** correctly - by renaming variables where appropriate

Procedure in-lining is actually an **optimisation** that may **improve the execution time** of the program
- Will cut down the *overhead* of calling a procedure
- Will enable other optimisations to take place a procedure call is not blocking their visibility
Overall **code size** *increases*, thus affecting performance of the instruction cache

---

**The Heap**

A value that **outlives** the procedure that creates it **cannot be kept**in the **activation record**
- The heap is the portion of the memory store that is used for data that **lives indefinitely** or until the program explicitly **deletes it**
- In contrast to local data that typically becomes inaccessible when their procedure ends, data on the heap are *not tied* to the **procedure activation that creates them** 

Created with `malloc()`, deleted with `free()`

The **memory manager** is the **subsystem** that **allocates and deallocates** space in the heap
- Serves as an interface between application **programs** and the **OS**
- For languages like C and C++, the memory manager is responsible for implementing the deallocation of memory when the programmer calls `free` or `delete` explicitly
- For languages like Java, it implements **garbage collection** - the process of **reclaiming** the spaces in the heap that are **no longer used** in the program

**Memory allocation**
- Produces a chunk of **contiguous heap memory** of the requested size
- If possible, satisfies an allocation using **free space** in the heap
- Else, attempts to **increase the heap storage space** by getting **consecutive bytes** of **virtual memory** in the OS
- If memory is exhausted, then report this back to the program that requested the memory

**Deallocation**
- Returns deallocated space to the pool of **free space**
- Typically does not return memory to the OS, even if the program's heap usage drops

**Space efficiency**
- Should **minimise** the total heap space needed by a program
- Achieved by *reducing* **fragmentation**

**Program efficiency**
- Should make good use of the memory subsystem to allow **programs** to *run faster*
- Difficult - can't be solely left to eh memory or compiler itself, need programmer to write efficient code to exploit locality (spacial and temporal)

**Low overhead**
- Memory allocation and deallocation are frequent - thus they should be as efficient as possible - operating with *minimum overhead*
- Programmer can help by doing less frequent allocations, doing larger memory allocation or reusing memory

**Fragmentation** occurs during the running of the program, as it allocates/deallocates memory, the space is **broken up** into free and used chunks of memory, which may end up not being contiguous, and this having uneven holes which can't be easily filled by new memory allocations
- When a memory chunk is deallocated, free the memory of this chunk *and* **combine** that chunk with adjacent free chunks of the heap to form a larger chunk

Best-fit object placement:
- Allocate the requested memory in the **smallest available hole** that's large enough
- Spares larger holes for subsequent larger allocation requests

Next-fit object placement:
- Same as best-fit *but*
- Allocate in the hole that has **last been split**, if enough space for the new allocation request is available in that hole
	- Improves spacial locality
	- Chunks quickly allocated/deallocated (at similar time) are likely to be accessed close together (in time) so would be more efficient to be close to that place

In languages like C, management of dynamic memory is left to the programmer
- However, this can be a difficult task - lots bugs (unused memory not freed, dangling pointer dereference error, overwriting parts of data accidentally)

Solution - **automatic memory management** (*garbage collection*)
- When an object is created, **unused memory space** is **automatically allocated**
- Eventually you run out of unused space - thus need to reclaim space
- What space can be **reclaimed?**
	- The observation is that some space is occupied by objects that will **never be used again**
	- The memory space occupied by these objects are called **garbage**
	- This space can be **freed** to be *reused* again

e.g. 
```
myClass A = new class();
myClass B = new class();
A = B;
```

![[Pasted image 20260513181943.png]]

Here, `A` becomes **unreachable**
- The the space for `A` can be reclaimed and used for another object

How to determine if objects are reachable?
- Object `x` is **reachable** *if and only if*:
	- A **register contains a pointer to `x`**
	- Or, another **reachable object `y`** *contains* a **pointer to `x`**

All **reachable objects** can be *found* by *starting* from registers and **following all the pointers**
- Unreachable objects can never be used - garbage

![[Pasted image 20260513182115.png]]

However just because an object *is reachable*, doesn't guarantee that it will be used again in the future
- i.e. reachability is only an approximation (not an exact answer)

However, once an object becomes unreachable, it cannot become reachable again

Every garbage collector has the following steps:
- Allocate space **as needed** for new objects
- When space runs out:
	- Compute which objects **are reachable** starting from a **root** set of registers
	- Free space used by objects **not found** from above
Some garbage collectors activate even before the space actually runs out

**Mark and sweep**
- When memory runs out, the mark and sweep strategy executes:
	- **Mark** phase - Figure out reachable objects and '*mark*' them
	- **Sweep** phase - Collect and **free** the garbage objects
Every object has an **extra bit** - the **mark bit**
- Used only for garbage collection
- Marked as 0 initially (unreachable)
- Marked by 1 if found (reachable)

For this algorithm to work, we need:
- To be able to **visit all the variables** in the heap
- Must know the **size of each heap variable**
- 1-bit to hold the mark-bit

The above can be achieved by extending each heap variable with a **size field** and a **link field**
- **Link field** - will be used to connect all heap variables into a **single linked list** that will permit them to be visited

Another issue - we're running this when memory has **already run out**
- Yet we need memory to construct the *unscanned list*
	- And the size of the list is unbounded - can't reserve space for it a priori
- Solution - use a trick called **pointer reversal**
	- When a pointer is followed to get to its reachable object, it is **reversed** to point to its **parent**
	- This maintains an **implicit stack** to enable a depth-first search of all the objects that are reachable

![[Pasted image 20260513183438.png]]

After a garbage collection cycle, new objects are allocated memory from the **freelist**
- Pick a large-enough block and allocate memory of the required size from it
- Put the remaining free memory *back* into the **freelist**
This leads to **memory fragmentation**
- Need to **merge free memory blocks** whenever possible, during the **sweep phase**

---

**Summary**
- The `Code` area contains **object code** - fixed size for many languages, and read only
	- Created at **compile time**
- The `Static` area contains **data with fixed addresses** (i.e. global data)
	- Fixed size, could be read or write
- The `Stack` contains **activation records** for each currently active procedure
	- Holds local variables, each activation record is usually a fixed size
- The `Heap` contains data that outlives procedures that created them
	- In C, the heap is managed by the programmer *explicitly* using `malloc` and `free`
	- In Java, memory is allocated on the heap using `new` and reclaimed using *garbage collection*
- Both the Heap and Stack grow - need to take care they do *not* grow **into each other**
- Solution - start the heap and the stack at the **opposite ends of memory** and let them **grow towards each other**