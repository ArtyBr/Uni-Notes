We want to make sure we are **checking** important things during compilation, *depending on the language*
- Types
- Scope - match identifier **declarations** with their **uses**
- Inheritance relationships in OO languages

A **type** is a **set of values** together with a **set of operations** on those values
- A programming language comes with a **type system**
	- Specifies which operations are *valid* for which types
	- In other words, a set of logical rules that a legal program of that language must respect

Type *checking* verifies that the type system's rules are respected
- The goal is to ensure that operations are used **only** with the correct types
- Enforces the **intended interpretation** of values, since there won't be anything else that does this after this step

**Type errors** arise when operations performed on values **do not support that operation**
- Additionally type checking enables some **compiler optimisations**

Type systems can be:
- **Weak** (e.g. C, C++)
	- Operations can be performed on values of **wrong types**
	- Can allow type errors to happen at runtime
	- Also if the program behaviour becomes unspecified after an incorrect typing
- **Strong** (e.g. Python, Java)
	- All type errors are caught - guaranteed that there are **no type errors**
	- Every expression is assigned an **unambiguous type**, until it's reassigned

In general, **Strongly-typed** languages are **more robust**
**Weakly-typed** languages are often **faster**

- **Dynamic** (e.g. Python, JS)
	- All (or almost all) checking of types is done as part of **program execution** (run time)
	- In other words, type checking is performed **while running** the program
	- For compiled languages this means that the compiler generates code to do the checks at runtime
	- **Pro**: Allows for *rapid prototyping*
	- **Pro**: *Less restrictions*
	- **Con**: *Type errors* at runtime
	- **Con**: Type checking overhead *slows execution*
- **Static** (C, C++)
	- All (or almost all) checking of types is done as part of **compilation** (compile time)
	- In other words, type checking is performed **before running** the program
	- **Pro**: Catches many *errors at compile time* - bad things less likely to happen while program is running
	- **Pro**: No overheads due to runtime type checks - *faster execution*
	- **Con**: Compiler must prove correctness - restricts the types of programs that can be written
	- **Con**: Difficult to do rapid prototyping, e.g. may not know the types when initially creating some prototype
- **Untyped**
	- No type checking done at all
	- Essentially just machine code

![[Pasted image 20260512122538.png]]

---

Rules for type checking:

**Type synthesis**
- Builds the type of an expression from the **types of its sub-expressions**
- Requires names to be **declared** before they are used
- Type of $E_{1}+E_{2}$ is **defined** in *terms* of the types of $E_{1}$ and $E_{2}$

**Type inference**
- Determines the type of a language construct from the **way it is used**
- If $null$ is a function that tests whether a list is empty, we can imply that from $null(x)$, $x$ must be a list

**Type conversion** - reassigning a type of a variable or expression
- **Explicit** - done by a programmer, called a *cast*
- **Implicit** - done automatically by the compiler, called *coercions*
	- `a=2; b=2+'ac';`
	- Compiler may convert `a` to a string in order to perform string concatenation

Type conversion rules vary from language to language. There are a couple general **types** of **conversions**
- **Widening** conversions are intended to *preserve information* - any type lower in the hierarchy can be widened to a higher type without losing bits
- **Narrowing** conversions lose information
Implicit type conversions (coercions) are limited in many languages to **only widening** conversions

![[Pasted image 20260512123535.png]]

---

The **formalism** for type checking are the **logical rules of inference**
- We write $$\vdash e:T$$
- If the **expression** $e$ has **type** $T$
- $e : T$ reads as "$e$ has type $T$"
- The symbol $\vdash$ means 'we can infer' or 'it is provable that'
- Use the logical conjunction symbol $\land$ for 'and'

Write the inference rules using the following syntax:

![[Pasted image 20260512124017.png]]

This is read as 'if the **hypothesis** is true, then we can infer that the **conclusions** are true'
- An inference rule where the **hypothesis** is *empty* is called an **axium**

![[Pasted image 20260512124058.png]]

Adding **scope**:

If we see:

![[Pasted image 20260512124123.png]]

How do we know the **type** of $X$ if we don't know what it refers to?
- Solution: add **scope**

![[Pasted image 20260512124155.png]]

The scope of an identifier is the **portion** of the program in which that identifier is **accessible**

If we write

![[Pasted image 20260512124223.png]]

We read it as: "Under the *assumption* that variables have the types **given by scope $S$**, it is *provable* that the expression $X$ has the type $T$"
- Types are now proven **relative to the scope** they are in

![[Pasted image 20260512124329.png]]

Type checking is concerned with **proofs**
- The proof is one on the structure of the **abstract syntax tree** (AST)

![[Pasted image 20260512124423.png]]

Proof has the **same shape** as that of the AST - one type rule used at each AST node
- In the type rule used for a node $E$:
	- **Hypotheses** are the **proofs** of types of $E$'s subexpressions
	- **Conclusion** is the **type** of $E$
- Types are computed in a **bottom-up** pass over the AST

There are some more complicated rules:
- Arrays and Function calls

![[Pasted image 20260512124557.png]]

---

**Summary**
Why do we specify types in this way?
- Gives a **rigorous definition** of types, *independent* of any **particular implementation**
- Gives **maximum flexibility** in implementation - can implement type-checking however you want, as long as you *obey the rules*
- Allows **formal verification** of program properties - can do **inductive proofs** (recursive) on the structure of the program
