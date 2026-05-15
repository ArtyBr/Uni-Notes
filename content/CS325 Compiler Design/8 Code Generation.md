Takes as **input** the IR produced by the front end of the compiler, and the symbol table.
- IR could be:
	- Three address code
	- Virtual machine representations: Bytecodes, stack machine code
	- Graph based: AST, DAG

**Outputs** a *semantically equivalent* target program, which could be:
- RISC - many registers, three address instructions, simple addressing modes, simple instruction set
- CISC - few registers, two address instructions, various addressing modes, variable length instruction set
- Stack-based machine - Pushing operands on to a stack

Target program it generates must:
- **Preserve** the *semantic meaning* of the source program
- Make **effective use** of the **available resources** of the target machine
- Itself **run efficiently**

---

Main tasks:
- **Instruction selection**
	- Choosing appropriate target-machine instructions to implement the IR statements
- **Register allocation**
	- Deciding what values to keep in which registers and what values to keep in memory
- **Instruction ordering/scheduling**
	- Deciding in what order to schedule the execution of instructions - reordering operations to hide latencies

Register allocation and instruction scheduling are both **NP-complete problems**
- In practice, use **heuristic techniques** to generate good (but not necessarily optimal) code

---

# Instruction generation
Code generator must **map** the IR program into a code sequence that can be executed by the target machine
- This is therefore a **pattern matching problem**
- Depends on the **level of the IR**, ISA of the machine and desired quality of the code

The **simplest solution** - *translate* each IR instruction into one or more machine-code instructions
- Code generated is *not very efficient*

**Quality** of the generated code is usually determined by its **speed** and **size**
- **Speed** - execution time of machine instruction sequence
- **Size** - number of machine instructions
- Other qualities could be energy efficiency

![[Pasted image 20260515094552.png]]

Therefore it is important to know **instruction costs** in order to design good code sequences, however:
- cost information is **not readily availability** or difficult to obtain
- cost also depends on the **context** in which that set of IR code appears

Target ISA will also significantly affect instruction selection

## Method 1 - Simple code generator
Treat **each** three address instruction as a **macro**

![[Pasted image 20260515094735.png]]

Results in **inefficient code** - **repeated** load/store of registers
- But very simple to implement

## Method 2 - smart pick registers
A *variation* on method 1 - **keep track** of what values are in what registers - aim is to **avoid unnecessary loads and stores**
- Code generates for **one basic block** at a time:
- **Algorithm**
	- Considers *each* three-address instruction in turn
		- Decide **what loads** are necessary to get the needed operands **into registers**
		- Generate the target code for the required loads
		- Generated the target code for the **operation itself**
		- Generate target code for the **store**, if the result needs to be stored in memory
	- To make the above decisions in the algorithm, the following **is required**:
		- **Register descriptor** - for each register, keeps track of the variable names whose current value is in that register: `<register, variable name>` pairs
		- **Address descriptor** - keeps track of the location or locations where the current value of that variable can be found: `variable name, locations>`
			- Location can be a **register**, a **memory address**, a **stack location** or a combination of these

Example:

![[Pasted image 20260515095351.png]]

![[Pasted image 20260515095359.png]]

... Same for each instruction

So far we have assumed a very *loose* register selection criteria
- Assumed that there is an **unlimited number of registers**
- But also assume that we **reuse registers** when the value it holds is no longer needed

1. Is not realistic
2. Needs a proper strategy to select registers for re-use

Thus, we now design a function `getReg(I)` that, given a three address instruction `I : x = y op z`, will select the registers to be used in that instruction

**Algorithm**:
- Pick a register `Ry` for `y`
	1. If `y` is currently in a register, use that register. No load from memory is required
	2. If `y` is not currently in a register, but an empty register is available, pick one such register as `Ry`
	3. When `y` is not in a register, and no register is currently empty, then need to **pick register to reuse safely**
		- Assume a candidate register `R` that currently holds the value of variable `v`
		- Need to make sure that:
			- `v`'s value either is **not really needed**
			- There is **somewhere else** we can go to get the value of `v`
		- Possibilities are:
			1. If the address descriptor for `v` says that `v` is somewhere *besides*`R`, then OK to use `R`
			2. If `v` is `x`, (the value being computed by instruction `I`), and `x` is also *not* one of the other operands of instruction `I` (`z`), then OK to use `R`
			 **Otherwise**:
			3. If `v` is not used later after the instruction `I` in the block, then again OK to use `R`
			4. If **not OK** by 1. or 2. above then need to generate a store instruction: `ST v, R` to place a copy of `v` in its own memory location (called a **spill**)
		- As `R` may hold several variables currently (i.e. each variable has the **same value** that is *stored in* `R`), then repeat the above for each such variable `v`.
			- Count the **number of stores** (score) needed generating, and *pick* a register with the **lowest score**
## Peephole optimisation
A statement-by-statement code generation strategy often generates **naive code**
- The generated code can be improved by carrying out '*optimisations*' on the **target code**
	- Not guaranteed to produce a better executable by any mathematical measure
	- But many simple transformations can significantly improve the running time or space requirement of the target program

**Peephole optimisation**
- Examine a **short sequence** of (usually contiguous) target instructions (called the **peephole**)
- *Replacing* these instructions by a **short** or **faster sequence** whenever possible

![[Pasted image 20260515100524.png]]

- The peephole is a small, **sliding window** on a program
- Each improvement may create new optimisation opportunities - thus need to do **multiple passes** over the target code
- Peephole optimisation can be applied even to the IR form of the code

### Eliminating redundant loads and stores
Consider the following instruction sequence:

```
LD a, R0
ST R0, a
```

Can **delete** the store instruction because the first instruction will ensure that the value of `a` has already been loaded into a register `R0`
- But note that if the store instruction had **label** (i.e. a destination for a jump), it is *not certain* that the load will be executed before the store - thus cannot remove the store instruction

```
    LD a, R0
L1: ST R0, a
```

Thus the two instructions **have to be in the same block** for this transformation to be safe
- The simple code generation method 2 will not produce such redundant loads and stores, but method 1 will

### Eliminating unreachable code
Consider the following:

```
    if debug == 1 goto L1
    goto L2
L1: print debugging information
L2: 
```

An obvious peephole optimisation is to **remove jumps over jumps**
- Thus replace above by:

```
    if debug != 1 goto L2
    print debugging information
L2: 
```

If debug is set to `0` at the beginning of the program, **constant propagation** would transform this sequence into:

```
	if 0 != 1 goto L2
	print debugging information
L2: 
```

- Constant propagation can be done by a global 'reaching definitions' data-flow analysis

Now the conditional always evaluates to true, so **replace it** with `goto L2`
- Then all statements that print debugging information are **unreachable** and can be eliminated one at a time

### Flow of control optimisations
Simple intermediate code-generation algorithms frequently produce **jumps to jumps**, **jumps to conditional jumps** or **conditional jumps to jumps**
- Eliminate these unnecessary jumps with peephole optimisation

Jump to a jump:

![[Pasted image 20260515101840.png]]

If no jumps to `L1` then, now we an eliminate the statement `L1 : goto L2` provided it is **preceded by an unconditional jump**

Another case:

![[Pasted image 20260515101917.png]]

Suppose there is only one jump to `L1` and `L1` is **preceded** by an unconditional `goto`

![[Pasted image 20260515101952.png]]

Number of instructions in the two sequences is the same
- But we sometimes **skip the unconditional jump** in the second sequence, but never in the first

### Algebraic simplification / strength reduction
- AS - simplify instructions such as `x = x + 0` and `x = x * 1`
- Reduction in strength - replace expensive operations by equivalent cheaper ones
`y = x ** 2 => y = x * x`
- Fixed-point multiplication or division by a power of two is cheaper to implement as a shift.
- Floating-point division by a constant can be **approximated** as multiplication by a constant, which may be cheaper

### Use of machine idioms
The **target machine** might have hardware instructions to implement certain **specific instructions efficiently**
- The peephole can be used to detect situations that permit the use of these instructions, resulting in significant improvements
- Example - (some machines have) auto-increment and auto-decrement addressing modes - **add** or **subtract** 1 from an operand before or after using its value
	- Improve the quality of code when pushing or popping a stack
	- Can be used in code for statements such as: `x = x + 1`

# Optimal code generation for expressions

Use the **abstract syntax tree** of an expression to generate an **optimal code sequence**
- *Proven* to generate the **shortest sequence of instructions**
- The algorithm assumes there is only a **fixed number of registers available**

## Sethi and Ullman algorithm

**Step 1** - **Ershov numbers**
- Label any leaf 1
- The label of an **interior node** with **one child** is the label of its child
- The label of an interior node with **two children** is:
	- *The larger* of the labels of its children, if different
	- *One plus* the label of its children if labels are the same
- If we assume that:
	- All operands must be in registers
	- Registers can be used by both an operand and the result of an operation
- Then the label of a node is the **fewest registers** with which the expression can be evaluated using **no stores of temporary results**

![[Pasted image 20260515103231.png]]

**Step 2**: `gencode()` - generating code from a labelled expression tree
- **Input**: a labelled tree with **no common subexpressions**
- **Output**: an optimal sequence of machine instructions to evaluate the **root into a register**
- **Method**: 
	- *Start* at the **root** of the tree
	- Algorithm applied to a node with label $k$ will use only $k$ registers
	- Let $b \geq 1$ be base of the registers so that **actual registers** used are $R_{b}, R_{b+1}, \dots, R_{B+k-1}$ 
	- The result always appears in $R_{b+k-1}$
		1. To generate code for interior node with label $k$ and two children with **equal labels** (i.e. $k-1$), do:
			- Call `gencode(right child)` using **base** $b+1$: result appears in $R_{b+k}$
			- Call `gencode(left child)` using **base** $b$: result appears in $R_{b+k-1}$
			- Generate the instruction: `OP R_b+k, R_b+k-1, R_b+k` where `OP` is the appropriate operation of the interior node
		2. To generate code for interior node with label $k$ and children with **unequal labels** where **big child** has label $k$ and **small child** has label $m<k$:
			- `gencode(big child)` using **base** $b$: result appears in $R_{b+k-1}$
			- `gencode(small child)` using **base** $b$: result appears in $R_{b+m-1}$
			- `if (big child == right child)` generate the instruction: `OP R_b+k-1, R_b+m-1, R_b+k-1`
			- `if (big child == left child)` generate the instruction: `OP R_b+k-1, R_b+k-1, R_b+m-1`
		3. For a leaf representing operand `x`, if the base is $b$ generate the instruction `LD R_b, x`

Example:

![[Pasted image 20260515103942.png]]

![[Pasted image 20260515103952.png]]

## Evaluating expressions with an insufficient supply of registers

If the number of **registers available** < **label of the root of the tree**, we need extra store (to memory) instructions
- This is called **register spilling**
- Then need to **load those value back into registers** as needed

Algorithm: `modified_gencode()`
- **Input**: A labelled tree with **no common subexpressions** and a **number of registers** $r\geq 2$
- **Output**: An optimal sequence of machine instructions to evaluate the **root into a register** using no more than $r$ registers, name $R_{1}, R_{2}, \dots, R_r$
- **Method**: *Start* at the **root** of the tree, with base $b=1$
	0. For a node $N$ with label $k \leq r$ use `gencode()` algorithm
	1. For an interior node $N$ with label $k > r$, work on each side of the tree separately and **store the result of the larger subtree**
		- Examine labels of node $N$'s children. If there is at least one child with label $l \geq r$, pick the larger child (noted as *big child*), or either if their labels are the same
	2. **Recursively generate code** for the *big child*, using base $b=1$
		- The result will appear in register $R_r$
	3. Store result from *big child* in memory, generate instruction:
	   `ST t_k, R_r`
		- $t_k$ is now a temporary variable in memory
	4. Now generate code for *little child*
		- If *little child's* label $j \geq r$, use base $b=1$
		- If *little child's* label $j\leq r$, use base $b=r-j$
		- Recursively apply `modified_gencode()` to *little child*
			- The result will appear in register $R_{r}$
	5. Stored result is brought back into a register just before node $N$ is evaluated
		- Load result from *big child* back from memory into register $R_{r-1}$, generate instruction:
			`LD R_r-1, t_k`
	6. 
	- `if (big child == right child of N)`, 
		- generate instruction `OP R_r, R_r, R_r-1`
	- `if (little child == right child of N` 
		- generate the instruction `OP R_r, R_r-1, R_r` `

Example:
![[Pasted image 20260515115338.png]]

![[Pasted image 20260515115348.png]]

![[Pasted image 20260515115355.png]]

# Target machine architecture
The above algorithm works fine with **instruction sets that implement simple steps** (i.e. RISC)
- **RISC** - *Reduced* instruction-set computing architecture
	- One instruction per cycle - **same execution time** per instruction
	- Pipelining
	- Load/store architecture: operations on registers
	- Few forms of addressing
	- **Many registers**, register windows for procedures

But CISC machines allow **several steps** to be condensed into **one instruction**
- **CISC** - *Complex* instruction-set computing architecture
	- Complex forms of addressing (for data structures)
	- Operands of different lengths, different combinations of operands etc.
	- **Different execution times** for instructions
	- **Few registers**
	- Large number of possible instruction sequences

## Instruction selection by Tree Rewriting
For CISC machines, we can treat instruction selection as a **tree-rewriting problem**
- That is, machine instructions implement fragments of IR trees
- To code generate, **match tree patterns with instructions** rather than having a one-to-one correspondence

Assume input to the code-generation process to be a **sequence of trees** at the **semantic level of the target machine**
- To obtain these trees, insert run-time addresses into the intermediate representation
- Note how the leaves of the trees contain information about the **storage types** of their labels

![[Pasted image 20260515120048.png]]

Assignment to `a[i]` is an **indirect assignment** where the **r-value** of the location `a[i]` is set to the **r-value** of the expression `b + a`

![[Pasted image 20260515120225.png]]

- To *simplify* **array-address calculations**, assume all values are one-byte characters
- `ind` operator treats its argument as a memory address

Target code is generated by applying a **sequence of tree-rewriting rules** to reduce the input tree to a single node
- Each tree-rewriting rule has the form:
$$replacement \leftarrow template \{ action \}$$
- Where $replacement$ is a **single node**
- $template$ is a **tree**
- $action$ is a **code fragment**

Example - register to register add instruction

![[Pasted image 20260515120507.png]]
- If the tree consists of a subtree with a root labelled + and left and right child are registers

Call this replacement a **tiling of the subtree**
- The **tiles** are the set of **tree patterns** corresponding to legal machine instructions, and the goal is to **cover the tree with non-overlapping tiles**
- However, **more than one template** may *match* a subtree at a given time

Tree representations of target instructions have been used effectively in **code generator generators** which *automatically* *construct* the **instruction selection phase** of a code generator from a high-level **specification of the target machine**

![[Pasted image 20260515121910.png]]

A tree-translation scheme:
- Given an input tree, the **templates in the tree-rewriting rules** are applied to **tile the subtrees of the input tree**
- If a **template matches**, the matching subtree in the input tree is **replaced with the replacement node of the rule**
- Then the **action** associated with the rule is **carried out**
- Action usually consists of **emitting a sequence of machine translations**
- Repeat the above **until no more templates match**

At the end of the translation, the tree will be **reduce to a single node** and
- a sequence of machine instructions will be generated as the output of the scheme

Example:

![[Pasted image 20260515122125.png]]

...

![[Pasted image 20260515122137.png]]

Issues to be considered when code generating by tiling an input tree:
- **How** is the tree-pattern matching to be done?
	- Efficiency of the code generation process depends on the tree matching algorithm
- What if **more than one template** matches at a given time?
	- Choosing one template over another will lead to different machine code
	- Will affect the performance of the runtime of the generated code
	- Two ideas: **Maximal munch** and **dynamic programming**
- If **no template matches**, then the code-generation process *blocks*
	- Can prevent blocking by having **one or more target-machine instructions** to **implement each operator** in the intermediate code
- Also need to **guard against** the possibility of a **single node being rewritten indefinitely**
	- e.g. generate an infinite sequence of register move instructions or an infinite sequence of loads and stores

Example:

![[Pasted image 20260515122423.png]]

![[Pasted image 20260515122448.png]]

The **best tiling** corresponds to an instruction sequence of **least cost** - the *shortest* sequence of instructions
- If the instructions take **different amounts of time** to execute, then the best tiling is the least-cost sequence that has the **lowest total execution time**

## Optimum and Optimal tiling
Assume that we could give each kind of instruction a **cost** (e.g. num clock cycles)
- An **optimum tiling** is defined as the one whose **tiles sum** to the **lowest possible value**
	- Optimum tiling is based on an **idealised cost model**
	- In reality, instructions are not self-contained with individual costs - **nearby instructions interact** in many ways
- On the other hand, an **optimal tiling** is one where **no two adjacent tiles can be combined** into a **single tile of lower cost**
	- If a tree pattern (template) can be split into several smaller tiles that has a lower combined cost, then that pattern should be removed from our set of tile patterns (templates) before beginning to find an optimal tiling for a tree
- For **RISC** machines - tiles are *small* and of uniform cost, there is usually **no difference at all** between optimum and optimal tilings (advantage: can use the simpler tiling algorithms, e.g. maximal munch)
- For **CISC** machines - tiles are *large*, so there is sometimes a **noticeable difference** between optimum and optimal tilings

*Every* **optimum** tiling is *also* **optimal** but not vice versa

![[Pasted image 20260515123730.png]]

### Maximal Munch
An algorithm for optimal tiling

**Algorithm**:
1. *Start* with the **root** of the tree
2. *Find* the **largest tile** (one with the most nodes) that fits: cover the root node - and perhaps several other nodes near the root - with this tile
3. *Generate* the **instruction** corresponding to the tile
4. Repeat from 1. above for any subtrees that remain

As we start from the root and go towards the leaves, this is a **top down** algorithm
- The algorithm generates the instructions in **reverse order** as we generate the instruction for the root node first
- If **two tiles** of **equal size** match the root, then the choice between them is **arbitrary**
- Maximal munch will always completely tile the whole tree without getting stuck at some subtree if for **each node-type** int he tree, there exists a **single-node tile pattern**

### Dynamic Programming (BURS)
While maximal munch always find an optimal tiling, it's not necessarily the optimum tiling (greedy)
- A **dynamic programming** algorithm *can* find the **optimum**
- We discuss the **Bottom-Up Rewrite System** algorithm (*BURS*)

Idea is to find the optimum solution for the whole problem based on the optimum solution to each sub-problem
- For the tree tiling problem, the **sub-problems** are the **tiling of the subtrees**

Generate code for the following tree using tiles (templates) and instructions using BURS:
- Assume each instruction take 1 cycle to execute

![[Pasted image 20260515124329.png]]

- *Start* at the **bottom** of the tree
- The only tile that matches each leaf nodes is `ADDI`
- This instruction means that you can **move a constant to a register** by simply **adding it to a register**
- Therefore the cost for `CONST1` = 1 and cost for `CONST2` = 2

![[Pasted image 20260515124448.png]]

The next node is the `+`, there are **three tiles** that match a node rooted at `+`
- The `ADD` tile has **two leaves** but the `ADDI` tile has **only one leaf**
	- Leaves are represented as edges whose bottom ends exit the tile
	- The leaves of a tile are places where subtrees can be attached
- Using the `ADD` pattern to match the `+` node will incur the cost of the `ADD` and cost of carrying out the two leaves `CONST1` and `CONST2`
- Using either one of the `ADDI` patterns to match the `+` will incur the cost of the `ADDI` and the cost of carrying out one leaf node `CONST1` or `CONST2` (the cost of which is 1)
- The optimum cost for matching a `+` node is therefore 2

![[Pasted image 20260515124714.png]]

The next node is `MEM`, there are **three tiles** that match a node rooted at `MEM`
- Each of the candidate tiles have **one leaf**
- Using the first `LOAD` pattern to match the `MEM` node will incur the cost of the `LOAD` and the cost of carrying out the subtree attached to its leaf, which has an optimum cost of 2 (from our previous calculation for node `+`) - in total 3
- Using the second or third `LOAD` pattern to match the `MEM` will incur the cost of the `LOAD` and the cost of carrying out one leaf node `CONST1` or `CONST2` (the cost of which is 1) - in total 2
- Thus the optimum cost for matching a `MEM` node (and in this case the root of the tree) is 2

Once the cost of the root node (and thus the entire tree) is found, the instruction generation phase begins:

![[Pasted image 20260515124954.png]]

- Note that `generate_code(n)` **does not recur** on the children of node `n`, but on the **leaves of the tile** that matched at `n`.

![[Pasted image 20260515125026.png]]

No instruction is emitted for any tile rooted at the `+` node, because this was **not a leaf of the tile** matched at the root

Complexity of the algorithms:

![[Pasted image 20260515125107.png]]

# Register allocation
Deciding **what values** to keep in **which registers** and what values to keep in memory
- Registers are the fasteust **computational memory unit** on the target machine
- But not enough of them to hold all the values in a program
- Values not in registers will need to be kept in memory and appropriately loaded from memory / stored back to memory - i.e. generate load/stores

Thus, **efficient utilisation of registers** is critical in generating good code
- Candidate values for holding in registers - variables, temporaries, large constants
- When the code has **more live values** than registers, then **spill** registers to memory
	- Register spill is **costly** - loads and stores inserted by the register allocator are spill code
- In fact, register allocation can be viewed as a **low-level optimisation**

Register allocation solves **two distinct problems**
- **Register allocation** - *select* the **set of variables** that will reside in registers at each point in the program
	- **NP-complete** problem
	- Complexity is an issue as multiple data size for an item, non-uniform memory access costs, extended scope of register allocation across basic blocks
- **Register assignment** - pick the **specific register** that a variable will reside in
	- Can be **solved in polynomial time**

Practical processors have **different register classes**
- Usually there are **general-purpose** registers and **floating-point** registers
- Some processors may have others

Example register allocation:

![[Pasted image 20260515135507.png]]

By using a **different ordering** of the instructions, we can **avoid** the **register spill**

![[Pasted image 20260515135541.png]]

## Register allocation by Graph Colouring
A better idea for register allocation is to allocate registers based on their **liveness**
- Such an allocation can be used **across basic blocks**
- Variables `a` and `b` can **share** the **same register** *if* at any point in the program **at most one of** `a` or `b` is **live**

**Step 1**: Compute **live variables** for each point

![[Pasted image 20260515135718.png]]

There are **four rules** for computing live variables across basic blocks:
- **Rule 1**
	- `x` is **alive** at the end of statement `P`, if `x` is **used** by at least one of the basic blocks coming from it
	- ![[Pasted image 20260515135843.png]]
- **Rule 2**
	- `x` is **alive** at the start of a basic block if it is just about to be used
	- ![[Pasted image 20260515135953.png]]
- **Rule 3**
	- `x` is **not alive** if it is just about to be assigned a new value
		- i.e. value of `x` *before* the statement is **never going to be used**
	- ![[Pasted image 20260515140001.png]]
- **Rule 4**
	- If **no** `x` in `S` then **liveness** of `x` at the end of `S` is the **same** as the liveness of `x` at the beginning of `S`
	- ![[Pasted image 20260515140044.png]]

So to compute the live variables for each point:
1. Let liveness of **all variables** be **false** initially at each point of the program
2. Repeat until every statement satisfies rules 1 to 4

Here we assume that `b` is **live on exit** of these basic blocks:

![[Pasted image 20260515140152.png]]

**Step 2**: Interference Graph
- **Each variable** will become a **node** in the graph
- If **two variables** are **live** at some point in the program, then create **edge** connecting these nodes

![[Pasted image 20260515140247.png]]
- e.g. `a` and `c` **cannot be** in the **same register**
- `a` and `b` **can** be in the same register

**Step 3**: Colour the Interference graph
- **Assign colours** to the nodes
- Nodes **connected by an edge** *cannot have* the **same colour**

![[Pasted image 20260515140351.png]]

If a graph can be coloured with $k$ colours, we say it is $k$-colourable
- For register allocation, **colours=registers**
- A $k$-colourable graph uses **no more than** $k$ registers

Can get the following target code:

![[Pasted image 20260515140449.png]]

However, computing graph colouring is an NP-hard problem
- Solution - **use heuristics**

**Claim**
- *Pick* a **node** $t$ with **fewer** than $k$ neighbours in the interference graph
- *Eliminate* $t$ and its edges from the interference graph
- *If* the resulting graph **is $k$-colourable** then **so is the original graph**

**Reason**
- Recall that $t$ has **fewer than** $k$ **neighbours**
- Let $C_{1},C_{2},\dots, C_{n}$ be the colours assigned to the neighbours of $t$ in the **reduced graph**
- Since $n<k$, we can **pick some colour** for $t$ that is **different** from **those of its neighbours**

![[Pasted image 20260515140744.png]]

**Algorithm** - to colour an interference graph $G$ with $k$ colours
- **Phase 1** - find $k$ "colourability" of the graph
	- While $G$ has some node $t$ with neighbours **less than $k$**
	- *Pick* a **node** $t$ with **fewer than** $k$ neighbours
	- *Put* $t$ on a **stack** and **remove it** from $G$
	- *Repeat* until the graph $G$ is **empty**
- If **all nodes removed** then graph is $k$-colourable
- Else, graph $G$ **cannot be coloured** with $k$ colours
- **Phase 2** - assign colours to nodes
	- *Start* with the **top** of the stack
	- *Add* the **node** on the **stack top** to the graph including its edges
	- *Pick* a **colour different** from those assigned to **already coloured neighbours**
	- *Repeat* until **stack is empty**

Example:

![[Pasted image 20260515141058.png]]

At the end of phase 1:

![[Pasted image 20260515141135.png]]

Now after phase 2:

![[Pasted image 20260515141153.png]]

What happens if the graph colouring **fails to find a colouring**? (for a given $k$)
- This means that **all the values** *cannot* be held in registers
- Some values need to be **spilled** to memory

Spilling *will occur* when **each node** in the graph has $k$ or more neighbours

![[Pasted image 20260515141310.png]]

Solution - *pick* a **candidate node** for **spilling**
- Spilled value will be held in memory

![[Pasted image 20260515142204.png]]

Now we continue with the algorithm as before, which will result in a 3-colouring of this simplified graph

![[Pasted image 20260515142229.png]]

Eventually we need to **load** `f` to a register - i.e. assign a colour to `f`
- **We could get lucky** - at this point we may discover that among the four neighbours of f, we actually use use **less than 3** colours - *Optimistic colouring*

However this wasn't the case for this example.
- Now we have to spill the value to memory
- Allocate a memory location for `f` (typically on the stack) - call this `fa`
- Generate code to load `f` to a temporary before each operation that reads 
	- `f : f = load fa`
- Generate code to store `f` back to memory after each operation that writes `f`
	- `f : store f, fa`

![[Pasted image 20260515142506.png]]

Re-compute liveness

![[Pasted image 20260515142523.png]]

Note how `fi` (that is `f1, f2, f3`) is only live:
- *between* `fi = load fa` and the next instruction where it is used
- *between* a `store fi, fa` and the preceding instruction

The effect of this is to **greatly reduce** the live range of `f`
- In turn, reduces its interferences, which results in fewer graph neighbours

New interference graph:

![[Pasted image 20260515142711.png]]

We see that now running Chaitin's algorithm to completion  is possible as **there will be some node** in this new graph having **less than 3 neighbours** at each stage of the algorithm
- As such, this is now 3-colourable

But how do we *decide* **what value** to **spill** to memory?
- **Any choice** will be correct
- But choice will affect performance in some way

Possible heuristics:
- Spill values with **most conflicts** in the graph
- Spill values with **few definitions and uses** (will minimise CPU operations load and store)
- **Avoid** spilling in **inner loops** (as it affects performance significantly - most load and store operations)

# Summary
Intermediate code uses **too many temporaries** - (problem ignored at IR level)
- Thus it makes a **significant difference to performance** to get a good allocation of temporaries to registers

Register allocation discussed was *mostly applicable* for **RISC machines**

For **CISC machines**, the allocation is much more complicated
- Often has **restrictions** on *how* a register **can be used**
	- Some operations work only with some registers
	- Registers with different sizes that will limit the type of values that can be held in them
	- The graph colouring algorithm is **adapted** with 'extra steps' to manage these additional restrictions in CISC machines

