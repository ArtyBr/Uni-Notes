A **compiler** is a program that can read a program in one language - the *source language* - and translate it into an equivalent program in another language - the *target language*

![[Pasted image 20251017115141.png]]

If the target program is an executable machine-language program, it can then be called by the user to process inputs and produce outputs

---

An **interpreter** appears to directly execute the operations specified in the source program on inputs supplied by the user. It *does not* produce a target program.

![[Pasted image 20251017115304.png]]

- Usually, the machine-language target program produced by a **compiler** is *much faster* than an **interpreter**
- *But* an interpreter can usually give *better error diagnostics* than a compiler

---

Some languages, like Java (and Scala), *combine* compilation and interpretation

![[Pasted image 20251017115426.png]]

- Bytecode **compiled** on one machine can be **interpreted** on another machine
- Some Java compilers translate the bytecode into machine language *immediately* before they run the intermediate program to process the input
	- **Just-in-time** compilation, JIT

---
### Creating an executable from a compiler

Example for languages such as C, C++ and Fortran:

![[Pasted image 20251017115930.png]]

- The **Preprocessor** *collects* the source program
	- Source program may be spread across separate files and modules
	- It *expands* short-hands (macros) into source language statements
	- *Adds/removes* portions of the **source statements** based on programmer defined and environment variables
- A **Target assembly program** is easier to give as *output* for errors, and easier to *debug*
- The **linker** resolves external memory addresses, where code in one file may refer to a location in another file.
- The **loader** brings together all of the executable object files into memory for *execution*

--- 
## Phases of a compiler

![[Pasted image 20251017120312.png]]

---

### 1 - Lexing

The first stage is called **lexing**, or **lexical analysis**, or **scanning**.
- It sees a **sequence of characters** as input

- **Scan** the input one char at a time
- **Remove** whitespace
- **Group** characters into meaningful sequences called **lexemes**
- For each **lexeme**, produce as output a **token** of the form:

![[Pasted image 20251017120529.png]]

- Attempt to find the *longest* possible **legal token**
- **Error recovery**

**Output** is a sequence of **tokens** and a **symbol table**

![[Pasted image 20251017120701.png]]

---
### 2 - Syntax analysis (aka Parsing)

- Lexing has identified the **words**, now we need to find the **structure** of the program
- The structure is specified by a **grammar**
	- Set of grammatical **rules** in a notation such as BNF

![[Pasted image 20251017120902.png]]

- **Extracts the structure** - determine *how* the **grammar** has been *applied* to yield the input program
- The **output** of syntax analysis is typically a data structure representing the structure of the program - called a **parse tree** (aka **concrete syntax tree**)

![[Pasted image 20251017121014.png]]

---
### 3 - Semantic analysis (Context sensitive analysis)

Verify that the program is **meaningful**

Uses the **syntax tree** and information from the **symbol table** to:
- Check the source program for **semantic consistency** with the language definition
- Gather **type information** and saves it to the syntax tree or the symbol table

Important aspect - **type checking**
e.g.
- An array index must be an integer - report an error if a float is used to index an array
- A binary operation between a float and bool is an error

Language definition can also allow for **type conversions** (called **coercion**)
- Adding two numbers - an int and a float - may result in the integer being **converted** or **coerced** into a float

Will also check if variables are **in scope** and have been **declared** in that scope

![[Pasted image 20251017121711.png]]

---

### 4 - Intermediate code generation

In the process of translating a source program into target code, a compiler may construct *one or more* **intermediate representations** (IR)

After semantic analysis, many compilers generate an explicit low-level or machine-like IR which we can think of as a program for some *abstract machine*
- IR should be **easy to produce**
- Should be **easy to translate** into **target machine code**

![[Pasted image 20251017121933.png]]

A popular form of IR is **three-address code** - a sequence of assembly-like instructions with three operands per instruction

Why do we need it?
- Compiler needs a representation for **all the facts it derives** about the program.
- This is encoded in the IR and is used to *convey* this information from **on phase** of the compiler **to another**

IR code generation is the **last phase** that is **independent** of the **target platform** (OS and architecture)

---

### 5 - Symbol table management

We consider the symbol table to also be an **intermediate representation**

A symbol table **records** the **variable names** (including function/procedure names) used in the source program and **collects information** about various **attributes of each name**

**Attributes** contain information such as:
- **Storage** allocated for a name
- **Type** of a name
- **Scope** of the name (where it can be used)
- For procedure names:
	- **Number** and **types** of its **arguments**
	- **Method of passing** each **argument** (by value or by reference)
	- **Type returned**

Symbol table is a *data structure* containing a **record** for each variable name, with fields for the attributes of the name
- Allows to (*very quickly*) **find** the record for each name
- **Store** and **retrieve** data from that record quickly

![[Pasted image 20251017122627.png]]

Symbol table for **each scope**

---

### 6 - Code optimisation

This can be performed across **all steps** of a compiler

When performing across **machine-independent stages**, code optimisation attempts to **improve** the **intermediate code** so that better **target code** will result
- Better usually means **faster**, but **shorter code** or **code that consumes less power** can also be objectives
- A good way to generate **good target code** is to follow IR generation with code optimisation

![[Pasted image 20251017123213.png]]

There are **simple optimisations** that **significantly improve** the **running time** of the target code without slowing down compilation too much

---

### 7 - Target code generation

The code generator takes as **input** an IR of the **source program** and **maps** it into the **target language**
- If the target language is **machine code**, *registers* or *memory locations* are selected for *each of the variables* used by the program (**register allocation**/**memory management**)
- Then the intermediate instructions are translated into sequences of **machine instructions** that perform the **same task** (**instruction selection**)

A key aspect of code generation is the **judicious** (optimal) assignment of **registers** to hold variable (register allocation)

![[Pasted image 20251017123629.png]]

