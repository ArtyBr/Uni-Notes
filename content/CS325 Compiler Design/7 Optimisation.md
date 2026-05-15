Optimisations basically mean **code improvement**
- Replace one sequence of instructions with **improved** sequences

What to improve?
- Running time
- Size of code
- Memory usage
- Num of memory accesses
- Num of messages sent over network
- Power consumption
- Disk accesses
However must preserve meaning of code

Two phases:
1. Processing **intermediate code**
	- **Machine independent** optimisations
	- But, exposes optimisations opportunities that are sufficiently low level
2. Post-processing **target code**

---

**Basic blocks** partition the IR program into **maximal sequences** of **consecutive** three-address instructions
- Flow of control can only enter a basic block through the first instruction of the block
- No jump in the block (`goto`)
- Flow leaves block or halts at last instruction

*Finding* basic blocks involves:
- Finding the **branch instructions**
- Identifying **targets** of these branches
- Then **everything in between** *are* basic blocks

Basic blocks become the **nodes** of a **flow graph**, whose **edges** indicate which blocks can **follow** which other blocks

A **control flow graph** (CFG) is a directed graph, with:
- Basic blocks as nodes
- There is an **edge** from block $B$ to block $C$ if and only if it is possible for the first instruction in block $C$ to immediately follow the last instruction from block $B$

The CFG is a way of **summarising** the interesting **decision points** in a piece of code
- The body of a procedure can be represented as a CFG

---

We look at a few **granularities** of optimisations
1. **Local** - apply to a basic block isolation
2. **Global** - apply to a **control flow graph**
	- Whole method/function/procedure body in isolation (but not entire program)
3. **Interprocedural** - apply across procedure/method/function boundaries
	- Attempt to optimise a collection of functions/procedures as a whole
	- But much more difficult to implement

There is a **payoff** - is it *worth* taking the **time and effort** to implement some optimisations, based on the **performance improvements** you get in return?

---

# Local Optimisations
## 1. Algebraic Simplification
*Replace* **complex** operations with **simpler** ones
- Some statements can be **deleted** (reflexive):

```
x: int
x = x + 0
x = x * 1
```

Some statements can be **simplified**
- Even if both of the above statements may take the same time, assigning a constant will facilitate **other optimisations**
	- `x = x * 0` => `x = 0`
- The following will **definitely improve runtime** - exponentiation increases overhead by accessing math libraries
	- `y = y ** 2` => `y = y * y`
- **Bit-shifts** are cheaper than multiplication
	- `x = x * 8` => `x = x << 3`
	- However on some modern machines integer multiplication is just as fast

**Reduction in strength** optimisations

## 2. Constant Folding
Compute operations on **constants** at compile time

```
x = 2 + 2         ->   x = 4
if 2 > 0 goto L   ->   goto L
if 2 < 0 goto L   ->   (delete)
```

Dangers to watch out for during constant folding - **Cross Compilation**

![[Pasted image 20260514100843.png]]

- e.g. Machine $Y$ may be an embedded system (weak, limited)
- Beneficial to develop program on machine $X$ which is faster/more powerful

If $X$ and $Y$ are **different architectures**, floating-point representations may **differ**
- Replacing the result of a floating point expression (made up of constants) at compile time on machine $X$ may be **different** from how that expression is evaluated on machine $Y$

## 3. Eliminate unreachable basic blocks
Code that is **unreachable** from the **initial basic block**
- Basic blocks that are not the target of any jump or 'fall through' from a conditional

Makes:
- Code **smaller**
- Run **faster**
	- Code fits in the instruction cache
	- Increased spatial locality - instructions physically closer together

How can a program end up having *unreachable* basic blocks?
- `#define` statements - e.g. remove debugging code defines with `#define`

![[Pasted image 20260514101208.png]]

- **Libraries** - many functions/methods are provided by libraries, but only a few of them are used in given code
- Result of **other optimisations** may *create* unreachable basic blocks

Some optimisations are simplified if intermediate code is written in **Static Single Assignment** form
- Each variable occurs **only once** on the left hand side of an assignment
- All subsequent re-uses are given subscripts to differentiate them

![[Pasted image 20260514101404.png]]


If a basic block is in SSA form, a definition/assignment `x =` is the **first use** of `x` in the block

## 4. Common subexpression elimination
Eliminates instructions that compute a value that has already been computed
- If basic blocks are in SSA form, then two assignments having the **same RHS** compute the **same value**

![[Pasted image 20260514101533.png]]

## 5. Copy propagation
Propagating copies through the code

If basic blocks are in SSA form, then if a statement of the form `u = v` appears in the block, all **subsequent uses** of `u` can be **replaced** by `v`

![[Pasted image 20260514101653.png]]

No effect on performance on its own, but *enables* **other optimisations** such as:
- Constant folding
- Dead code elimination (e.g. `a = x` may be deleted if there is no use of `a` subsequently)

If the propagated value is a **constant**, we call this **constant propagation**

![[Pasted image 20260514101749.png]]

## 6. Dead code elimination
Code that **does not contribute** to the program's **result** can be eliminated
- If `x = RHS` appears in a basic block, and if `x` **does not appear** anywhere else in the program:
	- The statement `x = RHS` is dead and can be eliminated

![[Pasted image 20260514101854.png]]

---

Each local optimisation does **very little** on its **own**
- But typically these optimisations will **interact** - performing one optimisation *enables another*
- Thus, compilers will **repeat** individual optimisations until **no further improvements possible**

e.g.
```
a = x ** 2   => a = x * x            
b = 3                       => (delete)
c = x                       => (delete)
d = c * c    => d = x * x   => d = a     => (delete)
e = b * 2    => e = 3 * 2   => e = 6     => (delete)
f = a + d                   => a + a
g = e * f                                => 6 * f
```

---

However, there can be a **serious problem** with the above optimisations - what to do with pointers?
- The *problem* is **aliasing** - **two names** for the **same variable**

```
*p = 1;
*q = 2;
x = *p;
```

- Without knowing if `p` and `q` point to the same memory location, i.e. whether they **can be aliased**, we cannot conclude that `x` is equal to `1` at the end of the block
	- (could be that the pointers p and q point to the same place, meaning when assigning `*q=2`, this may re-assign the value where `p` is pointing to, making the answer `2` rather than `1`)

Procedure parameters, array accesses and indirect references all may have aliases, and it is not easy to tell if a statement is referring to a particular variable `x`
- Optimisations discussed further **assume non-aliased variables**

---

# Global Optimisations

Throughout this, we use the following example CFG to demonstrate each example.

![[Pasted image 20260514104557.png]]

## 4. Common Subexpression Elimination
First, we can perform a local subexpression elimination on $B_5$

![[Pasted image 20260514104635.png]]

Then, we can see that going from $B_3$ to $B_{5}$, there is no change to `j` and no change to `t4`, so `t4` can be used if `4*j` is needed

![[Pasted image 20260514104731.png]]

Next, we can see that `t5` is assigned to `a[t4]` which is now the same thing as `t9` therefore can replace any instance of `a[t4]` with `t5`

![[Pasted image 20260514104936.png]]

Can notice same sort of thing with `t2` (`4*i`) and `t3` (`a[t2]`), can replace this in $B_{5}$ where `t6 = 4*i` and `x = a[t6]` => `x = t3` etc.

Finally, an example here: in $B_{1}$ we have `v = a[t1]` and in $B_{6}$ `t14 = a[t1]`. However since there are assignments to `a` in $B_{5}$ before reaching $B_{6}$, can't do common subexpression elimination there.

## 5. Copy propagation
Copy statements are usually **introduced** *during* common subexpression elimination
- These are assignments of the form `u = v`

In the example below, to eliminate the common subexpression `d+e`, we must use a **new variable** `t` to **hold** the value `d+e`

![[Pasted image 20260514105332.png]]

The **idea** behind copy-propagation is:
- Given an assignment `u = v`, *replace* later uses of `u` *with* `v`, provided there are no intervening assignments to `u` or `v`
	- i.e. use `v` for `u` wherever possible after the copy statement `u = v`
- Thus, the value of variable `t`, instead of the expression `d+e`, is assigned to `c`

![[Pasted image 20260514105453.png]]

In this case it doesn't give a shorter sequence of instructions, but it does facilitate **further optimisations**

## 6. Dead code elimination
One advantage of **copy propagation** is that it often turns the **copy statement** *into* **dead code**
- But when doing dead code elimination in a CFG, need to make sure that the variable is **not live** outside of the basic block

![[Pasted image 20260514105712.png]]

### Next use and Live variables
Knowing when a value of a variable will be **used next** is essential for carrying out optimisations
- The **use** of a name in three-address code

Assume three-address statement `i` assigns a value to `x` - `i : x = value`
- If statement `j` has `x` as an operand - `j : z = x op y`
- And control can flow *from* statement `i` to `j` along a path that has **no intervening assignments** to `x`,
- Then we can say that `j` **uses** the value of `x` computed at statement `i`

![[Pasted image 20260514105948.png]]

We further that is `x` is **live** after the statement `i`
- In other words, a variable is **live** at a particular point in the program if its **value** at that point **will be used** in the **future** (*otherwise*, **dead** at that point)
- Therefore, to compute liveness at a given point, we need to **look forward** into the future and **work** **backwards**

Algorithm:
- **Input**: Basic block $B$. All non temporaries in $B$ set to **live** at exit of basic block
- **Output**: For each statement `i : x = y op z` in $B$, attach to `i` the **liveness** and **next-use** of `x`, `y`and `z`
	- Note that we are **actually attaching** liveness to the **point** *just before* **statement** `i`
- **Method**: Start at the last statement of $B$, *scan* statements **backwards** going to the beginning of $B$

For **each statement** `i : x = y op z` do:
1. Attach to `i` the current information in the symbol table regarding next use and liveness of `x`, `y` and `z`
2. In the symbol table, set `x` to **not live** and **no next use**: i.e. `x` is assigned a new value
3. In the symbol table, set `y` and `z` to **live** and next uses of `y` and `z` to `i`

![[Pasted image 20260514110438.png]]

After dead code elimination, we get this:

![[Pasted image 20260514110618.png]]

## 7. Reduce strength in induction variable calculations
In a loop, a variable whose value is *derived* from the **number of iterations** that have been executed by an enclosing loop is called an **induction variable**

![[Pasted image 20260514110810.png]]

Induction variables can be optimised by computing it with a **single increment** (**addition** or **subtraction**) *per loop iteration*

When there are **two or more** induction variables in a loop, it may be possible to **get rid** of **all but one**
- In this case, we are **unable** to eliminate the induction variable `i` as it is **used later**
- This optimisation involves a **strength reduction** - *replacing* an **expensive operation**, such as **multiplication**, by a **cheaper one**, such as **addition**

![[Pasted image 20260514111004.png]]

When optimising processing loops, useful to **work "inside-out"**
- *Start* with the **inner loops**, and
- *Proceed* to progressively **larger, surrounding loops**

In our example we *started* with the **inner loops** $B_{2}$ and $B_{3}$
- Reduced strength in induction variable calculations
- Attempted to eliminate induction variables
Next, we look at the induction variables in the context of the **outer loops**

## 8. Induction variables elimination
Look at our example:

![[Pasted image 20260514111551.png]]

We can see that `i` and `j` are **only used** to determine the **outcome** of the **test in block** $B_{4}$
- But we know that the values of `i` and `t2` satisfy the relationship `t2 = 4*i`
- We also know that the values of `j` and `t4` satisfy the relationship `t4 = 4*j`
- Thus, the test `t2 >= 4` can substitute for the test `i >= j`
- Once this replace is made, `i` in block $B_{2}$ and `j` in block $B_{3}$ become **dead variables**
- The **assignments** to them in these blocks become **dead code** that can be eliminates

![[Pasted image 20260514111538.png]]

## 9. Code motion
Loops are a key place for optimisations - particularly **inner loops** where the bulk of the time is spent doing **computations**
- Can optimise a loop by **decreasing** the **number of iterations** in an inner loops, even if we increase the amount of code outside that loop

**Code motion** takes an expression that **yields the same result** *independent* of the **number of times** that loop is executed (loop invariant code) and move it **outside the loop**

![[Pasted image 20260514111756.png]]

## Revisiting global constant propagation

![[Pasted image 20260514111849.png]]

If there is **no assignment** to `X` in block $B_{2}$, we can **propagate** the **constant value** of `X` from block $B_{1}$ to the use of `X` in block $B_4$
- But, as soon as **one other block** on the path to block $B_{4}$ **assigns** to `X`, we can **no longer** do this
- It is correct to apply constant propagation to a variable `X` from an assignment statement `A : x = ..` to a **given use of** `X` in a statement `B` *only if* the **last assignment** to `X` is in **every path** to `B` from `A`
	- i.e. the value (definition of `X`) that **reaches** its use in `B` **on all paths** is the **value defined in** `A`

Carrying out the **gloval constant propagation** is **more complicated** for this CFG
- There is an **unbounded number** of **execution paths**
- The first time program point (5) is executed, the value of `a` is `q` due to definition $d_1$
	- We say that $d_{1}$ **reaches** point (5) in the first iteration
- In subsequent iterations, $d_{3}$ **reaches** point (5) and the **value** of `a` is 243

# Data Flow Analysis

Checking **all paths** in a CFG will include paths around **loops** and through branches of **conditionals**
- This is *not* a trivial task - need to do an analysis of the **entire control-flow graph** for the procedure body

Essentially, we are attempting to *determine* some property $P$ at a particular point in program execution
- For the constant propagation example, property $P$ can be informally written as: "The definitions of variables **reaching** the use of each variable"

This is typical for optimisations that depend on some property $P$ at a particular point in program execution

**Data-flow analysis** refers to a body of techniques that derive information about the **flow of data** along program execution **paths**

The **basic idea** is to express the analysis of a complicated program as a **combination of simple rules** relating the **change in information** *between* **adjacent statements**
- That is, we 'push' or *transfer* information from one statement to the next, *adhering* to a set of **constraints**
	- Constraints based on the **semantics of the statements** ('transfer functions')
	- Constraints based on the **flow of control**
- Then, for each statement `s`, we end up with information about the data-flow values related to the property $P$ we are interested in, immediately before and after `s`
	- `IN[s]` - values *before* `s`
	- `OUT[s]` - value *after* `s`
- Here, the data-flow values for property $P$ come from an abstract domain, containing the values we care about, values computed statically by our analysis
	- The set of all possible data-flow values is called the **domain** of property $P$

The data-flow values before and after a statement are *constrained* by the **semantics** of the statement

![[Pasted image 20260514115548.png]]

This relationship between the data-flow values *before* and *after* a statement `s` is known as a **transfer function**
- In a transfer function, information may *propagate* in **two directions**
	- **Forward** along execution paths - transfer function `f_s` takes the data-flow value before the statement and produces a new data-flow value after the statement
	- `OUT[s] = f_s(IN[s])`
	- **Backwards** up the execution paths - transfer function `f_s` converts a data-flow value after the statement to a new data-flow value before the statement
	- `IN[s] = f_s(OUT[s])`

*Within* a basic block, control flow is simple
- If a block $B$ consists of statements `s1, s2, ..., sn` in that order, then the control-flow value *out* of `si` is the **same** as the control-flow value *into* `si+1`
	- `IN[si+1] = OUT[si]` for all `i = 1,2,...,n-1`

Control flow edges **between basic blocks** create more complex constraints between the **last statement** of *one* basic block and the first **statement** of the *following* basic block

![[Pasted image 20260514120033.png]]

- The set of definitions **reaching** the leader statement of a basic block is the **union of the definitions** after the last statements of each of the predecessor blocks

A **data-flow schema** involves data-flow values for a given property at **each point in the program**
- But we can restate the schema in terms of data-flow values *entering* and *leaving* the basic blocks, due to the simplicity of what occurs inside a basic block

Given a basic block $B$ consisting of statements `s1, s2, ..., sn` in that order, then:
- If `s1` is the **first statement** of $B$, *then*:
	- `IN[B] = IN[s1]`
- If `sn` is the **last statement** of $B$, *then*:
	- `OUT[B] = OUT[sn]`
- The **transfer function** of a basic block $B$, `fB` can be derived by **composing** the transfer functions of the statements in the block:
	$$f_{B}=f_{s_{n}}\circ \dots \circ f_{s_{2}\circ f_{s_{1}}}$$
- Then the **relationship** between the **beginning** and **end** of the block is:
	- `OUT[B] = fB(IN[B])`

The constraints due to **control flow** between basic blocks can be rewritten by substituting `IN[B]` and `OUT[B]` for `IN[si]` and `OUT[sn]` respectively
- Example: the **forward flow** problem of computing all the definitions that *may reach* a program point:
	- ![[Pasted image 20260514120622.png]]
- The **backward flow** problem of determining the **liveness** of variables at each program point:
	- ![[Pasted image 20260514120646.png]]

A **definition** of a variable `x` is a statement that **assigns** a value to `x`
- Definition `d` **reaches** a point `p` if there is a **path** from the point **immediately following** `d` to `p`, such that `d` is **not killed** along that path
- A definition of a variable `x` is **killed** if there is **any other definition of `x`** *anywhere along* the **path**

![[Pasted image 20260514120819.png]]

Note that the path may have loops, so we could come to another occurrence of `d` along the path which does **not kill** `d`
- Reaching definitions are used to do global constant/copy propagation

Example:

![[Pasted image 20260514120910.png]]

Consider the following definition

`d: u = v+w`

This statement:
- **Generates** a definition `d` of variable `u`
- **Kills** all the other definitions int he program that **define variable** `u`
- Leaves all the remaining incoming definitions unaffected

The **transfer function** of definition `d` thus can be expressed as:
$$f_{d}(x) = gen_{d}\cup (x-kill_{d})$$
Where
- $gen_{d}=\{d\}$ - set of definitions generated by the statement
- $kill_{d}$ - set of all other definitions of $u$ in the program
- $x$ = set of all definitions reaching statement $d$ (i.e. `IN[d]`)

The transfer function of a basic block can be found by **composing** the transfer functions of the statements contained within the basic block
- Suppose there are **two statements**, hence **two transfer functions**

![[Pasted image 20260514121239.png]]

- Then the compose transfer function will be:

![[Pasted image 20260514121252.png]]

- Here we have assumed that the statement for `f1` appears in the basic block before statement for `f2` appears
- Also note that the values in the parenthesis are computed first, where inner parenthesis take precedence over outer parenthesis

The composition of functions in this form is referred to as the '**gen-kill** form'

**gen-kill** form of a transfer function for a basic block extends easily to basic blocks with **any number** of statements
- If basic block $B$ has $n$ statements with transfer functions

![[Pasted image 20260514121814.png]]

The transfer function for block $B$ may be written as:

![[Pasted image 20260514121832.png]]

Thus, similar to a statement, a basic block **also generates a set of definitions** and **kills a set of definitions**
- The **gen** **set** contains all the definitions inside the block that are **visible** *immediately after the block* - we refer to them as **downwards exposed**
- A definition is **downwards exposed** in a basic block only if it is **not killed** by a subsequent definition to the **same variable** *inside* the **same basic block**
- A basic block's **kill set** is simply the **union** of all the **definitions killed** by the individual statements
Note that as a result, a definition may **appear** in **both** the **gen** and **kill** set of a basic block
- Then the fact that it is in **gen** *takes precedence* because in gen-kill form, the **kill set** is *applied* *before* the **gen set**

We assume that every CFG has **two empty basic blocks**:
- An `ENTRY` node, representing the **starting point** of the graph
- An `EXIT` node to which all exits **out** of the graph go

No definitions reach the beginning of the graph, thus the transfer function for the `ENTRY` block is `OUT[ENTRY] =`$\emptyset$ 
- This is called the **boundary condition** for reaching definitions

Given the following basic block, compute `OUT[B]`

```
d1: a = 3
d2: a = 4
```

- $gen_{B}=\{d_{2}\}$ since $d_{1}$ is **not downwards exposed**
- $kill_{B}=\{d_{1},d_{2}\}$ since $d_{1}$ *kills* **all other definitions** of `a` in the program AND $d_{2}$ *kills* **all other definitions** of `a` in the program

![[Pasted image 20260514123042.png]]

$kill_{B}$ is applied *before* the $gen_{B}$
- Thus, the reaching definitions at the end of basic block $B$ is $OUT[B]=f_{B}(IN[B]) = \{d_{2}\}$

Since a definition **reaches** a program point *as long as* there **exists at least one path** along with the **definition reaches** $OUT[P] \subseteq IN[B]$ whenever there is a **control-flow edge** from $P$ to $B$  
- However, since a definition cannot reach a point *unless* there is a path along which it reaches, $IN[B]$ needs to be no larger than the **union** of the **reaching definitions** of all the **predecessor blocks**

Thus, it is safe to assume:

![[Pasted image 20260514123346.png]]

- **Union** is called the **meet operator** for reaching definitions
- In any data-flow schema the **meet operator** is used to create a **summary** of the **contributions** from **different paths** at the *confluence* of those paths

Therefore, we use an **iterative algorithm** for computing reaching definitions
- **Input**: A CFG for which $kill_{B}$ and $gen_{B}$ have been computed for each block $B$
- **Output:** $IN[B]$ and $OUT[B]$, the set of definitions reaching the entry and exit of each block $B$ of the CFG
- **Algorithm**:
	- ![[Pasted image 20260514123606.png]]

The first two lines initialise certain data-flow values:
- Boundary condition $OUT[ENTRY] = \emptyset$
- Assume initially that **no definitions reach out of any block**

The algorithm then looks at **each block** $B$, in turn, and applies the **control-flow equation** and **transfer function** to the block
- We apply the equations to each block **until we see no change** to the OUT set of any block
	- Any change in any OUT set also means that there will be no change in an IN set
- At this point, the algorithm has **converged** on a solution and we exit out of the loop

Example:
- represent the seven definitions $d_{1}, ..., d_7$ in the flow graph by **bit vectors**, where bit $i$ from the left represents the definition $d_i$
- Union of the sets, $\cup$, is the **logical OR** of the corresponding bit vectors
- The **difference of two sets** $S - T = S \text{ AND } (!T)$

![[Pasted image 20260514124114.png]]

...

![[Pasted image 20260514123822.png]]

Note how every $B$, $OUT[B]$ **never shrinks** - i.e. once a definition is added, it stays there forever
- Since the set of all definitions is finite, eventually there will be a pass of the while-loop during which **noting is added** to any $OUT$, and the algorithm then terminates

The **number of nodes** in the flow graph is an **upper bound** on the number of times around the while loop
- Can carefully order blocks to reduce the number of times the while-loop is iterated over

With the use of **bit vectors** and operations on them implemented by **logical operations**, this algorithm is surprisingly efficient in practice

## Constant propagation
Is a use of a variable a constant?
- Check **all reaching definitions**
- *If* **all** *assign* the **variable** to the **same constant**
- Then use *is* in fact a constant
- Replace use of variable with constant

## Live variable analysis
In live variable analysis, we wish to know for variable `x` and point `p` whether the value of `x` at `p` could be used along some path int eh flow graph starting at `p`.
- A variable `x` is **live** at point `p` *if*
	- `x` is used along some path starting at `p`, and
	- no re-definition of `x` along the path before the use

![[Pasted image 20260514140529.png]]

- A variable `x` is **dead** at point `p` *if*
	- No use of `x` on any path from `p` to exit node, or
	- if all paths from `p` redefine `x` before using it

![[Pasted image 20260514140539.png]]

To compute liveness at a given point, we need to look at the use of variables **in future** and **work backwards**
- Live variable information is used in **register allocation** and **dead code elimination**

Let $use_{b}$ be the set of variables whose values may be used in $B$ **prior to any definition of the variable** (*upwards exposed*)

Let $def_B$ be the set of variables **defined** (i.e., definitely assigned values) in a basic block $B$

![[Pasted image 20260514140632.png]]

Due to the above definitions,
- Any variable in $use_{B}$ must be considered **live** *on entrance* to block $B$
- Any definitions of variables in $def_{B}$ are definitely **dead** *at the beginning* of block $B$

Essentially, membership in $def_{B}$ "kills" any opportunity for a variable to be live because of paths that begin at $B$

Given the $def_B$ set and $use_{B}$ set for a basic block $B$, we can come up with the transfer function that relates the live variables at the **beginning of the block**, $IN[B]$ based on the **live variables at the end of the block**, $OUT[B]$

![[Pasted image 20260514141033.png]]

A variable is **live coming into a block** if either:
- It is **used before redefinition** in the block (i.e. member of the $use_B$ set)
- It is **live coming out** of the block (i.e. member of $OUT[B]$) *and* is **not redefined** (not in $def_{B}$ set) in the block

A variable is **live coming out of a block** *if and only if* it is **live coming into one of its successors**

![[Pasted image 20260514141208.png]]

![[Pasted image 20260514141213.png]]

Thus, **Union** is the **meet operator** for live variable analysis

Liveness is calculated **backwards** starting from the exit node, as such the **boundary condition** for liveness is that no variables are live on exit from the program:

![[Pasted image 20260514141252.png]]

- **Input**: A CFG for which $def_{B}$ and $use_{B}$ are computed for each block $B$
- **Outputs**: $IN[B]$ and $OUT[B]$, the set of variables live on entry & exit of each block $B$ of the CFG
- **Algorithm**:

![[Pasted image 20260514141401.png]]

- The first line applies the **boundary condition**
- The second line initialises the solution by *assuming* that all variables will be **dead** at **entry** to a block
- The algorithm iterates by **starting from the final code** (i.e. EXIT) of the CFG and going **backwards**
- Again, can use **bit vectors** to represent variables that are live and converge to a solution

![[Pasted image 20260514141512.png]]

![[Pasted image 20260514141521.png]]

How to use this for **optimisations**?

**Register allocation** - If a variable is **dead**, can *reassign* its **register**

**Dead code elimination**
- Eliminate assignments to variables that are not read later
- But must not eliminate last assignment to a variable visible outside the CFG
- Can eliminate other dead assignments
- Start algorithm by initialising all **externally visible variables** to live on exit from CFG

A more conservative (**pessimistic**) initialisation would be to *assume* **all variables** are **live** at the **start of the analysis**
- Then the analysis finds variables that are dead
- Can stop analysis early and use current result

## Available expressions
An expression `x+y` is **available** at a point `p` if:
- Every path from the entry node to `p` must evaluate `x+y` *before* reaching `p`, and
- There are no assignments to `x` or `y` *after* the evaluation but *before* `p`

![[Pasted image 20260514142448.png]]

We say that a block **kills** expression `x+y` if it assigns (or *may* assign) `x` or `y` and does not subsequently re-compute `x+y`
- A block **generates** expression `x+y` if it **definitely evaluates** `x+y` and *does not* subsequently **define** `x` or `y`
- If expression is **available** at use, *no need* to **re-evaluate** it
- Available expression information can therefore be used to do **global common subexpression elimination**

Consider the following statement: `x = y + z`
- This statement:
	- *Generates* and expression `y + z`
	- *Kills* all the expression involving variable `x`
	- Leaves the remaining available expressions unaffected

To **compute** available expressions at the end of a single statement `x = y + z`
1. *Assume* the set of available expressions *before* the statement is $S$
2. *Add* to $S$ the expression `y + z`
3. *Delete* from $S$ any expression involving variable `x`

The steps must be done in the correct order, as `x` could be the same as `y` or `z`

![[Pasted image 20260514143050.png]]

Similar to a statement, a basic block also **generates a set of expressions** and **kills a set of expressions**
- Assume $U$ to be the **universal set of all expressions** appearing on the **right of one or more statements** in the program
- Then for each block $B$:
	- $IN[B]$ is the set of expressions in $U$ that are **available** at the point *just before* **the beginning** of $B$
	- $OUT[B]$ is the set of expressions in $U$ that are **available** at the point *following* **the end** of $B$
- Let
	- $e\_gen_{B}$ be the expressions **generated** by $B$ (and not subsequently killed, i.e. downwards exposed)
	- $e\_kill_B$ be the set of expressions in $U$ **killed** in B
- Then the following transfer function for the basic block *relates* $IN[B]$ to $OUT[B]$

![[Pasted image 20260514143404.png]]

As there are no available expressions at the exit of the ENTRY node, the **boundary condition** is given by:

![[Pasted image 20260514143643.png]]

An **expression** is **available** at the beginning of a block *only if it is* **available** at the **end** of **all its predecessors**
- Thus, it is safe to assume:

![[Pasted image 20260514143723.png]]

**Intersection** is the **meet operator** for available expressions

Algorithm:
- **Input**: A CFG for which $e\_kill_{B}$ and $e\_gen_B$ are computed for each block $B$
- **Output**: $IN[B]$ and $OUT[B]$, the set of expressions available at the entry and exit of each block $B$ of the CFG
- **Algorithm**:

![[Pasted image 20260514143838.png]]

In this case, we initialise the OUT of every block to indicate the **all expressions are available everywhere** - this is an **optimistic assumption**
- Reason is that initialising the OUT sets to $\emptyset$ is **too restrictive**

Example:

![[Pasted image 20260514144047.png]]

The data-flow equations for block $B_{2}$ are given by:

![[Pasted image 20260514143933.png]]

We can write these equations as *recurrences*:

![[Pasted image 20260514143950.png]]

Where $I^{j}$ and $O^{j}$ are the $j^{th}$ approximations for $IN[B_{2}]$ and $OUT[B_{2}]$ respectively
- Starting with $O^{0}=\emptyset$, we get $I^{1}=OUT[B_{1}]\cap O^{0}=\emptyset$
- But starting with $O^{0}=U$, we get $I^{1}=OUT[B_{1}]\cap O^{0}=OUT[B_{1}]$

Intuitively, the solution obtained by starting with $O^{0}=U$ is **more desirable**, because it correctly *reflects* the fact that expressions in $OUT[B_{1}]$ that are not killed by $B_{2}$ are available at the end of $B_{2}$
- In other words, the algorithm **does not** progress to give any **code improvements** when starting with $O^{0}=\emptyset$

**Available expressions** are used for global **common sub-expression elimination**
- Assume expressions are **available at the start of analysis** (*optimistic*)
- Analysis *eliminates* all that are **not available**
- **Cannot stop analysis early** and use current result (was done with live variable)

In contrast, the algorithm for **live variables** (for dead code elimination) is **pessimistic**
- Assume **all variable are live** at the **start** of the analysis
- Analysis finds variables that are **dead**
- **Can stop analysis early** and use current result

Dataflow setup is the same for both analyses
- But, the optimism/pessimism *depends* on the **intended use**

In summary:
- **Reaching definitions** are used for **constant propagation**
- **Live variable analysis** is used for **dead code elimination** and **register allocation**
- **Available expressions** are used for **common subexpression elimination**

# Loop optimisations
Loops are important for directing/applying optimisations
- Higher execution counts
- Repeated, related operations
- Much of **real work** takes place in loops

Several aspects to exploit for optimisations
- **Overhead** - decrease control-structure cost per iteration
- **Data locality**
	- Spatial locality - use of co-resident data (different data located nearby)
	- Temporal locality - use of same data several times (within a short period of time)
- **Parallelism**
	- Execute independent iterations of loops in parallel

**Loop restructuring** - Change the **structure** of the loop, but leave the computations performed by an iteration of the loop body and their relative order **unchanged**
- Reduces loop **overhead**
- Loop unrolling, loop coalescing, loop collapsing, loop peeling, loop normalisation

**Data-flow based loop transformations** - Based on data-flow analysis, which tracks the flow of data through the program's variable
- Loop-based strength reduction, induction variable elimination, loop invariant code motion, loop un-switching

**Loop reordering** - Transformations that change the relative order of execution of the iterations of loop nest or nests
- These transformations are primarily used to expose **parallelism** and improve **memory locality**
- Loop interchange, strip mining, loop distribution (fission/splitting), loop fusion

**Dependence analysis**
- For parallel loop optimisations, we need to understand **dependence analysis**
- A dependence is a relationship between two computations that places **constraints** on their execution order
	- **Control dependence** - If one statement *determines whether* another will be executed, then the two statements have a **control dependence**
		- ![[Pasted image 20260514150917.png]]
	- **Data dependence** - Two statements cannot be executed simultaneously due to conflicting uses of the same variable
		- Three types:
		- **Flow dependence**
			- ![[Pasted image 20260514150936.png]]
		- **Antidependence**
			- ![[Pasted image 20260514150950.png]]
		- **Output dependence**
			- ![[Pasted image 20260514151003.png]]
- Identify these constraints
- Use them to determine whether a particular transformation can be applied **without changing the semantics** of the computation

## Restructuring
### Loop unrolling
To **reduce overhead**, *replicate* the loop body
- Unrolling *replicates* the body of a loop some number of times called the **unrolling factor** ($u$)
- Then *iterates by* step $u$ *instead of* step 1

![[Pasted image 20260514151159.png]]

Improves performance by:
- **Less overhead** *per useful operation*
- **Longer** basic blocks for **local optimisation**

Most compilers for HPC machines will unroll at least the innermost loop of a loop nest

### Loop coalescing
Coalescing **combines a loop nest** *into* a **single loop**, with the original indices computed from the resulting single induction variable

![[Pasted image 20260514151337.png]]

Will improve the **scheduling** of the loop on a parallel machine
- On a machine with $P$ processors, the aim is to run each independent iteration simultaneously
- If `n` and `m` are slightly larger than $P$ then neither of the loops will schedule well, since executing the last $n-P$ iterations will take the same time as the first $P$
- Coalescing the two loops ensures that $P$ iterations can be executed every time except during the last (`nm mod P`) iterations

Coalescing may also reduce loop overhead of the original loop nest
- The **complex subscript calculations** introduced by coalescing **can often be simplified** to reduce the overhead of the coalesced loop

### Loop collapsing
Collapsing is a simpler, more efficient but **less general** version of coalescing in which the **number of dimensions of the array** is **reduced**

![[Pasted image 20260514170148.png]]

Collapsing eliminates the overhead of **multiple nested loops** and **multidimensional array indexing**
- It also increases the number of **parallelisable loop iterations**

Collapsing is best suited to loop nests that **iterate over contiguous memory** with a **constant stride**
- When more complex indexing is involved, **coalescing** may be a better approach

### Loop peeling
When a loop is peeled, a small number of iterations are **removed** from the beginning or end of the loop and **executed separately**

![[Pasted image 20260514170452.png]]

Peeling has **two uses**:
- For **removing dependence** created by the **first or last few loop** iterations, thereby enabling parallelisation
- For matching the iteration control of adjacent loops to **enable fusion**

Since peeling simply **breaks a loop into sections**, without changing the iteration order, it can be applied to **any loop**

### Loop normalisation
Normalisation converts all loops so that the induction variable is initially 1 (or 0) and is incremented by 1 on each iteration

![[Pasted image 20260514170623.png]]

This transformation can expose **opportunities for fusion** and simplify **inter-loop dependence analysis**
- It can also help to reveal which loops are **candidates for peeling** followed by **fusion**

## Data-flow based
### Loop-invariant code motion
When a computation appears inside a loop, but its **result** *does not change* between iterations, the compiler can **move that computation outside the loop**

![[Pasted image 20260514170906.png]]

Loop-invariant code motion can be applied:
- At a high level, **to expressions** in the source code
- At a low level, to address computations

LICM is sometimes also called **code hoisting**, but hoisting is a more general term referring to any transformation that moves a computation to an earlier point in the program
- An expression can be evaluated earlier to **reduce register pressure** or **avoid arithmetic unit latency**
- A **load instruction** might be moved upward to reduce the effect of **memory access latency**

### Loop unswitching
Applied when a loop contains a **conditional** with a **loop invariant test condition**

![[Pasted image 20260514171126.png]]

Unswitching involves **replicating the loop** inside each branch of the conditional
- Saves the (repeated) **overhead** of conditional branching inside the loop
- Reduces the code size of the loop body
- Possibly enable the parallelisation of a branch of the conditional
- Candidates for unswitching can be identified during the analysis for **loop invariant code motion**

## Reordering
### Loop interchange
Exchanges the position of two loops in a **perfect loop nest**, generally moving one of the **outer loops** to the **innermost position**

![[Pasted image 20260514171308.png]]

A loop nest is a set of loops, one inside the next
- The nest is called a **perfect nest** if the body of every loop other than the innermost loop consists of only the next loop in the nest

In the original code, the inner loop accesses array `a` with stride `n` (column-major order)
- By interchanging the loop, the inner loop is converted access `a` with a stride of 1

Done to:
- **Enable vectorisation** by interchanging an inner, dependent loop with an outer, independent loop
- Improve vectorisation by moving the independent loop with **largest range** to the **innermost position**

Reduce stride, ideally to 1
- Improve parallel performance by moving an independent loop outward in a loop nest to **increase the granularity** of each iteration

Increase the number of **loop-invariant expressions** in the inner loop

### Strip mining
Method of adjusting the **granularity** of an operation

![[Pasted image 20260514171701.png]]

**Clean-up code** is needed if the iteration length is not evenly divisible by the strip length
- Strip mining is commonly used to choose the number of independent computations in the innermost loop of a nest
- Once strip mined, this independent loop can be **vectorised**

### Loop tiling
**Tiling** it the **multidimensional generalisation** of strip mining
- (aka blocking), primarily used to **improve cache re-use** by dividing an iteration space into tiles and transforming the loop nest to iterate over them
- It can also be used to improve processor, register, TLB, or page locality

![[Pasted image 20260514171849.png]]

The **inner too loops** of a matrix **multiplication** have this structure
- As such, critical for achieving high performance in dense matrix multiplication
- Key transformation when optimising for communication avoidance (i.e. reduce accessing main memory by keeping current working set in cache)

### Loop fusion
*Merge* multiple loops with the **same iteration space** into a single loop

![[Pasted image 20260514172012.png]]

Fusing **improves register** and **cache locality**: After fusing, `a[i]` need only be loaded once
- *Reduces* **loop overhead** by a factor of 2
- With large $n$, the fused loop performs better on a modern (cache based) superscalar machines
- Fusion also **increases instruction parallelism** by increasing the ratio of floating-point operations to integer operations in the loop

To fuse two loops:
- Both loops must have the **same loop bounds** 
- There **should not exist** statements $S_{1}$ in the first loop and $S_{2}$ in the second loop such that $S_{2}$ **depends on** $S_{1}$ in the fused loop - existence of such dependencies means that fusion would alter the execution order illegally

### Loop distribution (loop fission/loop splitting)
Breaks a **single loop** *into* **many**
- Each of the new loops has the **same iteration space** as the original, but contains a **subset** of the statements of the original loop

![[Pasted image 20260514172325.png]]

Distribution is used to:
- Create **perfect loop nests**
- Create **subloops** with **fewer dependencies**
- Improve **instruction cache** and **instruction TLB locality** due to **shorter loop bodies**
- **Reduce memory requirements** by iterating over fewer arrays
- **Increase register re-use** by decreasing register pressure

### Procedure inlining
*Replaces* a procedure call with a copy of the **body** of the called procedure

![[Pasted image 20260514172452.png]]

Each occurrence of a formal parameter is replaced with a version of the corresponding actual parameter, modified to reflect the calling convention (e.g. call by value, call by reference) of the language
- **Renaming** is required for the local variables of the in-lined procedure
	- If they conflict with the calling procedure's variable names, 
	- or if the procedure is in-lined more than once in the same caller
- Inlining can be almost always performed - except when the procedure is called **recursively**
- Inlining removes the procedure call overhead
- Enables other optimisations - as now the code is within a single method and global optimisations can be performed
- May allow to parallelise a loop that has a procedure call in them

## Loop dependencies
So far we have examined dependence in the context of straight-line code with conditionals
- In straight line code, each statement is executed at most once
- Thus control, flow, anti and output dependence capture all the possible constraints

**Analysing loops** is a more complicated problem
- Each statement may be executed many times
- For many transformations it is necessary to describe **dependence** that exist **between iterations**
- These are called **loop-carried dependences**

![[Pasted image 20260514172855.png]]

- Can we run iterations in parallel?

To discover whether there is a **dependence** in the loop next, it is **sufficient** to determine whether **any of the iterations** can **write** a value that is **read or written** by any of the **other iterations**

The following has a **loop-carried flow dependence**:

![[Pasted image 20260514172954.png]]

When `i=1`, the read `p[1]` in statement 2 depends on `p[2]` written when `i=1` by statement 1

Flow dependence - one iteration writes to a location that a later iterations reads
- **Source** of dependence - earlier statement (`i=1`, statement 2)
- **Sink** of dependence - later statement (`i=2`, statement 1)

Cannot parallelise loop iterations - cannot guarantee that **read will be done after** the correct value is written to it

If there are no dependences, can parallelise the loop - no interference between iterations
- Can be used to determine if **other transformations are legal** as well:
	- **Loop interchange**
	- **Loop fusion/distribution**

Need a method to:
1. Represent dependencies in loops
2. Use representation to identify loop-carried dependencies

### Iteration space graphs
Dependencies in a 1D loop:

![[Pasted image 20260514173320.png]]

**Nodes** represent each **iteration** - not the array location
- Determine *which* array locations are **read and written to** in each iteration
- Draw arrows representing dependencies - can use different types of arrows

![[Pasted image 20260514173408.png]]

- Dependencies can **only go forward** in time - always from an earlier iteration to a latter iteration
	- A **backward dependency** in a loop mean that an iteration of the loop **depends** on the results of a **future iteration**

In a 2D loop:

![[Pasted image 20260514173456.png]]

**Distance vectors** are a **more compact** representation than iteration space graphs
- Represent how far apart **in terms of iterations** two dependent operations (read/write or write/write) occur

![[Pasted image 20260514173532.png]]

Distance vector: (2)
- Each dependence is 2 iterations forward

Captures the '*shape*' of the dependence, but **loses** *where* the dependence originates

Examples:

![[Pasted image 20260514173620.png]]

![[Pasted image 20260514173627.png]]

We can use **direction vectors** as a further compact representation
- Only save the **direction of the dependence**

![[Pasted image 20260514173653.png]]

Less precise, but sufficient for some transformations
- For loop parallelisation
- Loop interchange

Anything other than a 0 is a dependence - we get dimension of the dependence and direction

Can now use distance and direction vectors to identify whether a loop is **parallelisable**
- If there is a **loop carried dependence**, **cannot be parallelised**

![[Pasted image 20260514173801.png]]

![[Pasted image 20260514173813.png]]

Interchange doubly-nested loop to:
- Improve locality
- Improve parallelism - move parallel loop to outer loop (course grained parallelism)

Loop interchange is **not always legal**, because it **reorders** a computation
- Use distance/direction vectors to **determine** the legality

![[Pasted image 20260514173958.png]]

![[Pasted image 20260514173931.png]]

- Loop fusion - combining multiple loops into a single loop
- Loop distribution - splitting a single loop into multiple loops

Tests for legality:
- Every dependence into original loops should have a dependence in the optimised loop
- Optimised loop should not introduce new dependencies

![[Pasted image 20260514174054.png]]

# Summary
**Optimisations**
- Algebraic simplification
- Constant folding
- Unreachable basic block elimination
- Common subexpression elimination
- Copy propagation / constant propagation
- Dead code elimination
- Code motion (loop invariant code motion)
- Reduce strength in induction variable calculations
- Induction variable elimination (global optimisation)

**Data-flow analysis**
- Reaching definitions
- Live variable analysis
- Available expressions

**Loop optimisations**
- Several discussed
- Loop dependence analysis