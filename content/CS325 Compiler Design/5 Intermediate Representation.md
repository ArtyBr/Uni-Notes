In the process of translating a source program into target code, a compiler may construct *one or more* **intermediate representations (IRs)**
- The IR is the **central data structure** that *encodes* all the compiler's **knowledge** of the program

**Syntax trees** are *one form* of IR - produced during syntax and semantic analysis
- The symbol table can also be considered a form of IR
Can also be thought of as a program for an abstract machine

*Last phase* of the compiler that is **machine independent**

We can broadly consider **three properties** of an IR:
1. **Structure**
	- **Graphical**
	- Tend to be large - memory consuming
		- *Parse Trees*
			- The parse tree is a graphical representation for the derivation (parse) that corresponds to the input program
			- Large relative to the source text  - represents a complete representation
				- There is a node for each grammar symbol in the derivation
				- Requires memory allocated for each node and each edge
				- Must traverse all those nodes and edges during compilation
			- ![[Pasted image 20260512140607.png]]
			- AST retains the eddential structure of the parse tree but eliminates most of the non-terminal nodes (more compact representation)
		- *Directed Acyclic Graphs* (DAGs)
			- A DAG for an expression identified **common sub-expressions**
			- **Merges** nodes represneting **identical sub-expressions**
			- ![[Pasted image 20260512140704.png]]
			- Explicitly **encodes redundancy**
			- Basic optimisation that reduces the cost of evaluation - made explicit by the use of a DAG
				- Example DAG construction from SDD:
				- ![[Pasted image 20260512140812.png]]
		- *Control Flow Graphs* (CFG, not context-free grammar)
			- Models the *flow of control* between **basic blocks** in a program
				- A **basic block** is a maximal-length sequence of a straight-line (or branch-free) code
			- A CFG is therefore a **directed graph** $G=(V,E)$
				- Each node $v\in V$ corresponds to a basic block
				- Each edge $e=(v_{i},v_{j})\in E$ corresponds to a possible *transfer* of control from block $v_i$ to block $v_j$
				- ![[Pasted image 20260512141058.png]]
			- Typically used in conjunction with other IRs
				- e.g. CFG represents the *relationships* between blocks, while the operations *inside* the block are represented using another IR
			- CFGs are also used as an IR to support other phases of the compiler - Optimiser, Instruction Scheduling, Global Register allocation
	- **Linear**
	- Consist of **sequences of instructions** that execute in their **order of appearance**
	- Imposes a clear and useful ordering on the sequence of operations
		- (Graphical IRs may allow different orders of execution)
	- Easier to rearrange, varying levels of abstraction
		- *One-address codes* - model the behaviour of accumulator machines and stack machines
			- Models the behaviour of **accumulator machines** or **stack machines** (e.g. JVM)
				- Assumes the presence of a **stack of operands**
				- Most operations **take** their operands from the stack and **push** their results back onto the stack
			- ![[Pasted image 20260512141522.png]]
			- Stack-machine code is *simple* to generate and execute
				- Java uses Bytecode which is a compact IR similar to stack-machine code
				- Opcodes are limited to one byte or less - hence name bytecode
				- Either run in an interpreter or are translated into target machine code just prior to execution (JIT)
				- Compact code - useful where code is transmitted over slow communication links
		- *Three-address codes* - Most operations take two operands and product a result
			- `i = j op k`
				- There is **at most one operator** on the right side of an instruction
				- Which menas no built-up arithmetic expressions are permitted
			- ![[Pasted image 20260512141725.png]]
			- Essentially **unravels** the multi-operator arithmetic expressions and flow-control statements
			- The code is **compact** 
				- Operations require 1 or 2 bytes
				- Names usually represented by **integers** or **table indices**
					- needs usually 4 bytes
			- The compiler generated immediate names allow three-address code to be rearranged easily - very desirable for **optimisations** and **target code generation**
			- Can also view Three-address code as a linearised representation of a syntax tree or a DAG
				- ![[Pasted image 20260512142000.png]]
			- **Addresses** - an address can be one of the following:
				- A **name**: for convenience source-program names are allowed to appear as addresses in three-address code. In practice a source name is replaced by a pointer to its symbol-table entry, where all info about the name is kept
				- A **constant**: e.g. floating point $4.6$ or integer $10$
				- A **compiler-generated temporary**: Useful, especially in optimising compilers to create a **distinct name** each time a temporary is needed. These temporaries can be combined, if possible, when registers are allocated to variables.
				- ![[Pasted image 20260512142232.png]]
			- **Instructions** - Possible instructions for 3AC
				- *Assignment instructions* of the form `x = y op z`
					- `op` is a binary arithmetic or logical operation and `x, y and z` are addresses
				- *Assignments* of the form `x = op y` where `op` is a **unary** operation
					- minus, logical negation (!), shift operators and conversion operators (int(), str())
				- *Copy instructions* of the form `x = y`
					- `x` is assigned the value of `y`
				- An *unconditional jump* `goto L`
					- The three-address instruction with label `L` is the **next** to be executed
				- *Conditional jumps* of the form `if x goto L` and `ifFalse x goto L`
					- Execute instruction with label `L` next if `x` is true or false
					- *Conditional jumps* such as `x relop y goto L`, which apply a **relational operator** `relop` (<, $==$, >= etc.)
				- *Procedure calls* such as `p(x1, x2, ..., xn)`, implemented as:
					- Where `param x` for parameters
					- `call p,n` for a procedure call
					- `y = call p,n` for a function call
					- The integer `n` indicates the number of actual parameters in `call p,n`
					- ![[Pasted image 20260512145140.png]]
					- Procedure calls can be **nested**
						- So some of the first `param` statements could be parameters of a call that comes after `p` returns its value; that value becomes another parameter of the later call
				- *Indexed copy instruction* of the form `x = y[i]` and `x[i] = y`
					- `x = y[i]` sets `x` to the value in the location `i` memory units (bytes) beyond location `y`
					- `x[i] = y` sets the contents of the location `i` units beyond `x` to the value of `y`
				- *Address and pointer assignments* of the form `x = & y`, `x = * y`, `* x = y`
					- `x = &y` sets the value of `x` to be the address location of `y`
					- `x = *y` sets `x` to be equal to the contents of that location pointed to by `y` (which is a pointer)
					- `*x = y` sets the contents of the location pointed to by `x` to the value of `y`
		- **Representing Linear IRs**
			- Look into the **data** **structures** representing three-address code
			- Represented as **objects** or **records** with fields for the **operator** and **operands**
			- Three such representations:
				- **Quadruples** (quads)
					- Unary operators like `x = minus y` or `x = y` do not use *arg2*
					- `param` does not use *arg2* or *result*
					- Conditional and unconditional jumps put the *target* label *in result*
					- ![[Pasted image 20260512165126.png]]
				- **Triples**
					- Has only three fields *op, arg1, arg2*
					- Refer to the result of an operation `x op y` by its **position** rather than an explicit temporary name
					- `x[i] = y` requires **two entries** in the triple structure
						- `x` and `i` in one triple and `y` in the next
					- ![[Pasted image 20260512165313.png]]
					- *Disadvantage*: when performing optimisations, instructions cannot be easily moved around
						- Moving an instruction may require to change all references to that result
				- **Indirect triples**
					- Consist of a **listing** of **pointers to triples**
						- Rather than a listing of triples themselves
					- ![[Pasted image 20260512165414.png]]
					- If optimisations require moving instructions, this can be done by reordering the instruction list, without affecting the triples themselves
	- **Hybrid**
	- Combine elements of both, attempting to capture their strengths and avoid weaknesses
2. **Abstraction** - Near-source representation or Low-level representation
	- The level of detail exposed in an IR influences the **profitability** and **feasibility** of different *optimisations*
		- ![[Pasted image 20260512165542.png]]
	- Structural IRs are usually considered **high level**, e.g. Concrete Syntax Trees and ASTs are implicitly related to the source code
	- Linear IRs are usually considered **low level**
		- (not necessarily true)
3. **Naming discipline** - Schemes used to name values in an IR have a direct effect on the compiler's ability to optimise the IR and to generate quality assembly code from the IR
 - Static single-assignment form (SSA) is an intermediate representation that facilitates certain **code optimisations**
	- In SSA form, names correspond uniquely to specific definition points in the code
		- i.e. each name is defined by one operation, hence 'static assignment'
		- ![[Pasted image 20260512170417.png]]
	- Given the above requirement, then different control paths will require different subscripts for the same variable
		- ![[Pasted image 20260512170500.png]]
	- SSA uses a notational convention called $\phi$-function to **combine** the two definitions of the variable
		- Here $\phi(x_{1},x_{2})$ has the value $x_{1}$ if the control flow passes through the true value of the conditional and the value $x_{2}$ if the control flow passes through the false part

---

We will next discuss *issues* related to the actual translation of statements into its intermediate representation

Begin with determining **types** and **storage layout** for variables
- We know that **actual storage** is allocated at **runtime**
- But **relative addresses** can be computed at compile time for local declarations
	- The relative address of a name or a component of a data structure is an *offset* from the *start* of a data area

From the **type** of a name, we can determine the **amount of storage** that will be needed for the name **at run time**
- At compile time, these amounts can be used to calculate the **relative address** for variables
The *type* and relative *address* are *saved* in the **symbol-table entry** for the name
- Data of **varying length** is handled by reserving a known **fixed** amount of storage for a **pointer** to the data

We assume storage comes in **blocks** of **contiguous bytes**
- Multi-byte objects are stored in **consecutive bytes** and given the the *address* of the *first byte*

The **width** of a type is the **number of storage units** needed for objects of that type
- Basic types - chars, ints, floats require an integer number of bytes
- For easy access, arrays and classes are allocated in one contiguous block of bytes

An expression with more than operator, such as `a + b * c` will translate into instructions with **at most one** operator per instruction, in three address form
- We can use an SDT as follows to generate three-address code for expressions:

![[Pasted image 20260512175005.png]]

**Addressing array elements**
Array elements are usually stored in a **block** of **consecutive memory locations**

The width of each array element is $w$ (in bytes), then the address of `A[i]` can be calculated by:
$$\text{base + (i - low)} \times w$$
Where $low$ is the *starting* *index* of the array
- 0 for C/C++, Java,
- 1 for Fotran
And $base$ is the *relative address* of `A[low]`

This can be rewritten as:
$$i\times w + (\text{base - low} \times w)$$
- Where $(\text{base - low} \times w)=C$ is some constant that can be **pre-calculated** at compile time
	- $C = \text{base}$ when $\text{low} = 0$
- `C` is saved to the symbol table entry for `A`, then the relative address of `A[i]` is obtained by simply adding $(i\times w)$ to `C`

We can generalise the address of array elements to $K$ dimensions:
$$\text{base}+(i_{1}\times w_{1})+(i_{2} \times w_{2})+\dots+(i_{k} \times w_{k})$$
- If the total number of elements in dimension $2$ is $n_{2}$
- $$n_{2}=w_{1}/w_{2}$$
- Then:
- The relative dimension of `A[i1][i2]` = 
- $$\text{base}+(i_{1}\times n_{2} + i_{2}) \times w_{2}$$
Generalising to $k$ dimensions, we get: `A[i1][i2]..[ik]`
$$\text{base}+(((\dots(i_{1}\times n_{2}+i_{2})\times n_{3}+i_{3})\times n_{4}+i_{4})\dots n_{k}+i_{k})\times w_{2}$$
---

Recalling types - type checking, inference and conversion
The compilers job is to:
- **Assign** a type expression to *each component*
- Determine that these type expressions **conform to the type system** (the rules) of the language

We can introduce semantic action for type conversion to the grammar $E\rightarrow E_{1}+E_{2}$

![[Pasted image 20260513093946.png]]
- Where $max(t_{1},t_{2})$ take two types and returns the maximum of them in the widening hierarchy
	- Declares an error if they they aren't in that hierarchy

---

Next we can consider the **translation** of **control flow statements** such as `if-else` or `while` to intermediate representation

![[Pasted image 20260513094138.png]]

Boolean expressions are used in **two ways** in a program:
- *Alter* the *flow* of control - `if (E) S`, statement `S` is reached if the expression `E` is `true`
- Computing logical values - A boolean represents a `true` or `false` value, can be evaluated as arithmetic expressions with logical operators (`AND, OR, NOT`)

We consider boolean expressions generated by the following grammar:

![[Pasted image 20260513094327.png]]

Consider the following grammar for flow of control statements:

![[Pasted image 20260513094352.png]]
- `B` is a boolean expression and `S` represents a statements

What we need is to generate the following sequence of code from the above grammar:

![[Pasted image 20260513094434.png]]

- Within $B.code$ are jumps based on the value of $B$
- These jumps will have the form: `goto label`

Syntax-direction definition for `if (B) S1`

![[Pasted image 20260513094615.png]]
- `label(L)` attached label `L` to the **next three-address instruction** to be *generated*
- `||` represents concatenation

In this case, the semantic rules create a new label `B.true` and attach it to the first three-address instruction generated for the statement `S1`
- Thus, jumps to `B.true` within the code for `B` will go to the code for `S1`
- By setting `B.false` to `S.next`, we ensure that control will skip the code for `S1` if `B` evaluates to false

![[Pasted image 20260513095058.png]]

![[Pasted image 20260513095106.png]]

Next we can consider the boolean expressions *themselves* and create the three address code for evaluating the boolean expressions

![[Pasted image 20260513095149.png]]

Example:

![[Pasted image 20260513100308.png]]

---

**Backpatching**

Consider the translation of the boolean expression `B` in `if (B) S`

![[Pasted image 20260513100609.png]]
- It contains a **jump** for when `B` is false to the instruction *following* the code for `S`
	- But `S` has *not* been *generated* yet
- Need a **second pass** (after `S` has been generated) to find out where to jump to when `B` is false
- However ideally we would like to do the IR generation in **one pass** - how?

Backpatching
- When a jump is generated the **target** of the **jump** is temporarily left *unspecified*
- Each such jump is put on a **list of jumps** whose labels are to be filled in when the proper label can be determined
- The list of jumps are passed as **synthesised attributes**

As code we generated for boolean expression `B`, jumps to the `true` and `false` exits are **left incomplete** with the label field *unfilled*

![[Pasted image 20260513100911.png]]

These incomplete jumps are place on lists pointed to by `B.truelist` and `B.falselist`

![[Pasted image 20260513100935.png]]

**Attributes** `truelist` and `falselist` of nonterminal `B` are used to **manage labels** in the `goto` statements that we generate for boolean expressions.
- `B.truelist` - a list of jump or conditional jump instructions into which we insert the label to which control goes if `B` is `true`
- Same but for false

The aim is to insert the **target label** for the instructions in the `B.truelist` and false list during a single pass such as a **single bottom-up parse**

The following is the SDT for backpatching:

![[Pasted image 20260513101105.png]]

With the previous example:

![[Pasted image 20260513101208.png]]
- The remaining empty jump labels will have their target filled in later when it is seen what must be done depending on the truth or falsehood of the **entire expression**

---

**Symbol table**

We saw during semantic analysis that we need an *efficiently addressable* **central repository** for **facts** 

Essentially, **symbol tables** are data structures that are used by compilers to hold information about source-program constructs
- **identifiers/variables** - its character string, data type, storage class, lexical level of its declaring procedure, base address and offset in memory etc.
- **arrays** - number of dimensions and upper/lower bounds of each dimension
- **records/structs** - list of the fields and information on each field
- **functions and procedures** - number of parameters, their types and the types of any returned values
Table entries may *initially* be created at lexical analysis or syntax analysis phases, but it is accessed, updated and manipulated throughout **every phase** of the compiler

Symbol table **localises information** derived from potentially distant parts of the source code
- No need to traverse annotated parse trees
- No need to search linear IR code to discover about variable declarations
- In fact, *no need* to have declarations in the **linear IR** form of the program **at all**
- Information can be efficiently read/written
Essentially and efficiency hack

So, implementation must ensure:
- Easy of **access**
- Efficient and graceful methods for **expanding** the the symbol table
	- Since size of the table can't be pre-determined
Usually use **hash-tables** for implementation

The **scope** of a declarations it the portion of a program to which the declaration applies
- As such, scopes are implemented by setting up a **separate symbol table** for **each scope**

![[Pasted image 20260513102210.png]]

![[Pasted image 20260513102221.png]]

Lectures show an example that details an SDT that shows how the symbol table can be used and manipulated including different tables for different levels of scope