- Lexing has identified the **words**, now we need to find the **structure** of the program
- The structure is specified by a **grammar**
	- Set of grammatical **rules** in a notation such as BNF

![[Pasted image 20251017120902.png]]

- **Extracts the structure** - determine *how* the **grammar** has been *applied* to yield the input program
- Sees a **stream of words** (we call **tokens**) where each word is **annotated** with a syntactic category
	- The parser determines that the input stream is a **valid program**
- The **output** of syntax analysis is typically a data structure representing the structure of the program - called a **parse tree** (aka **concrete syntax tree**)

![[Pasted image 20251017121014.png]]

If the input stream is **not a valid program**, the parser reports the problem and diagnostic information to the user. (**Syntax errors**)

---

We use **Context Free Grammars** (CFGs) for:
- Specifying the **syntax** of the source language
- Giving a **systematic method** of determining **membership** in this formally specified language
- We use CFGs to specify the **grammatical structure** of programming languages

**Algorithms** for determining whether a program is a member of a language:
- **Top-down** parsing
	- **LL(1)** and **recursive descent** parsers
	- Build parse trees **from top** (root) **to bottom** (leaves)
- **Bottom-up** parsing
	- **LR(1)**, **canonical LR(1)** and **LALR** parsers
	- Build parse trees **from the leaves** **up to the root**

Regular expressions can't be used for syntax analysis because we need a way to be able to recognise the **order** and **structure** of tokens appearing in the string, and there is also no way to ensure **precedence**
- Even more simple proof: unable to write a REGEX for 'balanced parentheses'
- $(^m)^n$

---

Intro and definition of Grammars from CS259 - [[6 Grammars]]

Example grammar for arithmetic expressions:

![[Pasted image 20251020094651.png]]

A grammar **derives a sentence** by beginning with the **start symbol** and repeatedly **replacing a nonterminal** by the body of a **production** of that nonterminal

**Parsing** is the process of taking a **string of terminals** and figuring out how to **derive it** from the **start symbol** of the grammar
- If can't - report a **syntax error**

---

Traditional notation - **Backus-Naur form (BNF)**

![[Pasted image 20251020100204.png]]

- **Nonterminals** wrapped in **angled brackets**
- **Terminal** symbols **underlined**
- `::=` means **derives**
- `|` means **also derives**

Given the following grammar:

![[Pasted image 20251020114443.png]]

Can we figure out if the sentence $'(a+b) \times c$' is a **valid sentence?**

By starting with the start symbol and applying rules **right-first**, we can derive this **parse tree** - representing a **derivation** as a tree graph
- aka a **rightmost derivation**

![[Pasted image 20251020114559.png]]

We can get the same tree from a **leftmost** derivation. However this is not always the case.

The parse tree has:
- **terminals** at the **leaves**
- **nonterminals** at the **interior nodes**

An **inorder** traversal of the **leaves** will give the **original input**
A **postorder** traversal of the tree will **evaluate** the **program**

Example:

![[Pasted image 20251020114951.png]]

A grammar for which **multiple derivations** exist (of ***either* the leftmost or rightmost derivation**) is called an **ambiguous grammar**

We don't want our program to have **multiple possible meanings**, therefore we would like to **re-write** the grammar such that there is only one possible meaning

![[Pasted image 20251020115236.png]]

A grammar is ambiguous if:
- it has **more than one leftmost derivation** for a sentential form
- it has **more than one rightmost derivation** for a sentential form

The **leftmost** and **rightmost** derivations for a sentential form **may differ** **even in an unambiguous grammar**

---

However, ambiguity may not be the **only issue**
- An unambiguous grammar may still not apply **precedence** correctly

![[Pasted image 20251020120337.png]]

The following re-write can add precedence to the grammar:

![[Pasted image 20251020120458.png]]

To add **precedence** to a grammar:
- Create a non-terminal for each **level of precedence**
- **Isolate** the corresponding part of the grammar
- Force the parser to **recognise higher precedence sub-expressions first**

e.g. for algebraic expressions
- Multiplication and division first (level one)
- Subtraction and addition next (level two)

# Parsing

A compiler must **infer a derivation** for a given string
- **or** determine that **no such derivation exists**

The process of **constructing a derivation** from a specific input sentence is called **parsing**

A **parse tree** is equivalent to a **derivation**
- Parse tree's **root is known** - **start symbol** of the grammar
- The **leaves** of the parse tree - they must match, **in order from left to right**, the **stream of words** returned by the scanner
- The actual parsing involved **discovering the grammatical connections** between the **leaves** and the **root**

There are **two distinct** and **opposite** approaches for constructing the tree:
- **Top-down**
	- Begin with the **root** and *grow* the tree toward the leaves
	- At each step, **select a node for some nonterminal** on the **lower fringe** of the tree
	- **Extend** it with a **sub-tree** that **represents the right-hand side of a production** that re-wites the nonterminal
- **Bottom-up**
	- Begin with the **leaves** and grow the tree toward the root
	- At each step, identify a **contiguous sub-string** of the parse tree's upper fringe that matches the right-hand side of the production
	- **Build a node** for the rule's left-hand side and connect it into the tree

## Top-down parsing

 Begin with the **root** and *grow* the tree toward the leaves
- At each step, **select a node for some nonterminal** on the **lower fringe** of the tree
- **Extend** it with a **sub-tree** that **represents the right-hand side of a production** that re-wites the nonterminal

![[Pasted image 20251020122359.png]]

It may become clear that choosing some next choice will lead to a **mismatch** between the **string** and the **fringe** of the parse tree
- Now the parser must **backtrack** and reconsider each of the previous choices

**Backtracking** - systematically reconsidering earlier decisions
- For a **valid** sentence - it will lead the parser to a **correct sequence** of choice and construct a correct parse tree
- For an **invalid** sentence - it will **systematically exhaust** all choices at each previous decision point and lead to reporting an **error** - **syntax error**
- Backtracking increases the **asymptotic cost** of parsing - it is an **expensive way** to discover syntax errors

![[Pasted image 20251020123056.png]]

There are a number of way to **transform** a grammar to make it **suitable** for top-down parsing
- Eliminate **left recursion**
- Backtrack-free grammars - **predictive grammars**
- Further eliminate backtracking with **left factoring**
### Left recursion

A grammar is said to be **left recursive** if it has a nonterminal $A$ such that there is a derivation $A\rightarrow Aa$ for some string $a$

![[Pasted image 20251020123814.png]]

Top-down parser **cannot handle** left-recursion
- With left-recursion, a top-down parser can **loop indefinitely** without generating a leading terminal symbol that the parser can match

We follow the following steps to transform a grammar and **eliminate direct left recursion**
1. Prepare the grammar by eliminating $\epsilon$ **productions**
2. Prepare the grammar by eliminating **cycles**
3. Now, given any number of $A$-productions, **group** these productions:
	- ![[Pasted image 20251020124052.png]]
	- Where no $\beta_{i}$ begins with an $A$
4. Then, **replace** $A$-productions with:
	- ![[Pasted image 20251020124134.png]]
	- We assume that not $\alpha_i$ is $\epsilon$

This shows how to eliminate **direct left recursion**
However, this does not eliminate **indirect left recursion**, such as:

![[Pasted image 20251020124328.png]]

To eliminate indirect left recursion, we follow the algorithm:

![[Pasted image 20251020124955.png]]

---

New terminology - **NULLABLE**
- We say that $A$ is *nullable* if all the symbols in $A$ can be expanded with $\epsilon$ productions
- In other words, $A$ **derives** $\epsilon$
- We write this is NULLABLE(A) being `true` or `false`

![[Pasted image 20251022131307.png]]

### Eliminating $\epsilon$ productions

- Look for *nullable* non-terminals
- If the non-terminal is *nullable*, **create a new production** by replacing it with $\epsilon$

![[Pasted image 20251022131411.png]]

e.g. for

![[Pasted image 20251022131425.png]]

After eliminating $\epsilon$ productions, the result is:

![[Pasted image 20251022131449.png]]

However, this **increases** the **number of productions** from $n+1$ to $2^{n}+1$

## Recursive descent parsing

There is **one parse method** *per* **non-terminal symbol**
- A **nonterminal symbol** on the right-hand side of a rewrite rule leads to a **call** to the parse method for that non-terminal
- A **terminal symbol** on the right-hand side of a rewrite rule leads to **"consuming"** that token from the input string
- **Multiple productions** from a nonterminal (i.e. with | in the CFG) leads to the parser needing to select one production at a time (with **backtracking** for incorrect selections) to complete the parse

The recursive descent parser uses the **operating system stack** (an *implicit* stack) when evaluating a sentence

General algorithm:
![[Pasted image 20251024103206.png]]
#### Backtracking
If we want to use backtracking, we need to make the following additions to the algorithm:
- Choosing the wrong *A-Production* in line 1 will be done in some order, one production at a time
- It will lead to **failure** in line 7, but can get back to line 1 and try another *A-Production*
- But to try another *A-Production*, need to **reset the input pointer** to where it was when line 1 was first reached
- Use a **local variable** to store this input pointer for future use
### Backtrack-free parsing
With a **look-ahead** in the input stream, and using **context**, a parser may pick the **correct production**
- A grammar that can be parsed **top-down** *without backtracking* is also called a **predictive grammar**

**Predictive parsing**:
- Given $A\rightarrow \alpha | \beta$ the parser must be able to choose **either** $\alpha$ or $\beta$ as the **correct choice**
- Solution - consider **both** $A$ (the focus symbol) *and* the **next input symbol**, called the **look-ahead symbol**

![[Pasted image 20251024103626.png]]

- Of course, in this case the 'first' symbol of the RHS of all choices were either terminals or an $\epsilon$. What do you do if the first symbol is a **nonterminal?**

### First set
If $\alpha$ is **any string of grammar symbols**, we define FIRST($\alpha$) to be the **set of terminals** that begin strings derived from $\alpha$. If $\alpha\rightarrow^{+}\epsilon$, then $\epsilon$ is also in FIRST($\alpha$)

- If $\alpha$ is either a **terminal** or `eof`, then FIRST($\alpha$)=${\alpha}$
- For a **nonterminal** $A$, first($A$) contains the **complete set of terminal symbols** that can appear as the **leading symbol** in a sentential form derived from $A$
- The **domain** of FIRST is the set of grammar symbols in $Terminals \bigcup Nonterminals \bigcup\epsilon\bigcup\text{eof}$ 
- The **range** of FIRST is $Terminals\bigcup\epsilon\bigcup\text{eof}$
- `eof` is a special 'end marker' symbol that is assumed not to be a symbol of any grammar

Considering two $A$-productions $A\rightarrow\alpha|\beta$, where FIRST($\alpha$) and FIRST($\beta$) are **disjoint sets**, we can choose between these $A$-productions by **looking at the next input symbol** $a$ since $a$ can be in **at most one of** FIRST($\alpha$) and FIRST$(\beta)$
- This would allow the parser to make a **correct choice** with a look-ahead of **exactly one** symbol
- (Almost, as we still need to handle $\epsilon$ productions)

We need to know the words that can appear as the **leading symbol *after*** a valid application of the $\epsilon$ production - The FOLLOW set.

---
### Follow sets
Define FOLLOW($A$) for nonterminal $A$ to be the **set of terminals** that can **appear immediately to the right** of $A$ in some sentential form. That is the set of terminals $a$ such that there **exists a derivation** of the form $S\rightarrow^{+}\alpha Aa \beta$ for some $\alpha$ and $\beta$

![[Pasted image 20251024104633.png]]

- Terminal $c$ is in FIRST($A$) and $a$ is in FOLLOW($A$)
- There **may have been symbols between** $A$ and $a$ at some time during the derivation, but if so, they **derived $\epsilon$ and disappeared**

Computing the **FOLLOW set**

Use the following rules:
1. If $A$ is the rightmost symbol in some sentential form then `eof` is in FOLLOW($A$)
2. For a production $A\rightarrow \alpha B \beta$, or a production, then everything in FIRST($\beta$) except $\epsilon$ is in FOLLOW($B$). Which means FIRST($\beta$) - $\epsilon \subseteq$ FOLLOW($B$). 
	- Since $\beta$ can follow $B$, $\subseteq$ reads as 'includes' or 'subset of or equal to'
3. For a production $A\rightarrow \alpha B$ or a production $A\rightarrow \alpha B \beta$ where $\beta$ is **nullable**, everything in FOLLOW($A$) is in FOLLOW($B$)
	- i.e. if $\beta$ is nullable then FOLLOW($A$) $\subseteq$ FOLLOW($B$)
##### Example run-through

![[Pasted image 20251020102028.png]]

![[Pasted image 20251020102040.png]]

![[Pasted image 20251020102100.png]]

![[Pasted image 20251020102109.png]]

![[Pasted image 20251020102120.png]]

---

### LL(1) property

Using FIRST and FOLLOW sets, we can specify precisely the condition that makes a grammar **backtrack-free** for a top-down parser - we call this the **LL(1) property**

We choose a production $N\rightarrow \alpha$ on a input symbol $c$ if:
- $c \in$ FIRST($\alpha$)
- Nullable($\alpha$) and $c\in$ FOLLOW($N$)

If we can **always choose a production uniquely** by using these rules, we say that the grammar is LL(1)
- The 1 means we can 'peek' **one symbol ahead** to make a decision
- The first $L$ stands for **scanning the input** from **left to right**
- The second $L$ stands for producing a **leftmost derivation**
- Parsers that use the LL(1) property are called **predictive parsers**
### Left-factoring to further eliminate backtracking

Not all grammars have the LL(1) property

![[Pasted image 20251020105220.png]]

- Productions 11, 12 and 13 all begin with `name` so cannot be uniquely identified
- In this case (in coursework) look further ahead by one token - you can take two look-aheads and make a decision

However, we are going to apply **left-factoring** to **transform** the grammar to LL(1)

Given a nonterminal and its productions:

![[Pasted image 20251020105433.png]]

Where $\alpha$ is a **common prefix** and $\gamma_{1}...\gamma_{j}$ represent right-hand sides that do not begin with $\alpha$

The left-factoring transformation introduces a **new nonterminal** $B$ to represent the alternate suffixes for and rewrites the original productions as:

![[Pasted image 20251020105548.png]]


**Summary** for the construction of LL(1) parsers
1. **Eliminate ambiguity**
2. **Eliminate left-recursion**
3. Perform **left factorization** if required
4. Add an extra start production $S'\rightarrow S\$$
5. Calulcate FIRST for every production and FOLLOW for every nonterminal
6. For nonterminal $N$ and input symbol $c$ choose production $N\rightarrow \alpha$ where:
	- $c\in$ FIRST($\alpha$) or
	- Nullable($\alpha$) and $c\in$ FOLLOW($N$)

This choice is encoded either in a **table** or **recursive-descent algorithm**

The primary **drawback** of top-down, predictive parsers lies in their **inability to handle left recursion** - this requiring grammar transformations

## Bottom-up parsing

Begin with the **leaves** and grow the tree toward the root
- At each step, identify a **contiguous sub-string** of the parse tree's upper fringe that matches the right-hand side of the production
- **Build a node** for the rule's left-hand side and connect it into the tree

![[Pasted image 20251020134251.png]]

We can think of bottom-up parsing as the process of **reducing** a string $w$ to the start symbol of the grammar

At each reduction step, a specific sub-string matching the **body of a production** is **replaced** by the **nonterminal** at the head of that production

![[Pasted image 20251020134402.png]]

---
### Derivations in Reverse and Handles

By definition, a **reduction** is the **reverse of a step in a derivation**
- The goal of bottom-up parsing is therefore to **construct a derivation in reverse**

Bottom-up parsing during a **left-to-right** scan of the input constructs a **rightmost derivation in reverse**

- A **handle** is a sub-string that **matches the body of a production**
- A handle's reduction represents **ones step** along the **reverse** of a rightmost derivation

The most critical part is **finding a handle to reduce** (and also deciding on **which production** to reduce with)

**Shift-reduce** parsing is a **form of parsing** where a **stack holds grammar symbols** and an **input buffer** holds the **rest of the string** to be parsed.

![[Pasted image 20251020135057.png]]

Steps in Shift-reduce parsing:
1. Parser **shifts zero or more input symbols** onto the stack, until it is ready to reduce a string $\beta$ of grammar symbols on top of the stack
2. **Reduce** $\beta$ to the head of the appropriate production
3. Repeat (1 and 2) until **parser detects an error** *or* until the **stack contains the start symbol** *and* the **input has been exhausted** (i.e. input buffer is empty)

### LR(K) Parsers

The **most prevalent bottom-up parsers**
- Based on the use of **shift-reduce** actions
- $L$ - scan the input **left** to right
- $R$ - Construct a **rightmost** derivation
- $k$ - number of look-ahead symbols that are used

**Advantages**
- Can be constructed to **recognise virtually *all* programming language constructs** for which CFGs can be written
- Can detect **syntactic error** as soon as it is possible to do so on left-to-right scan
- Is more **general** than other types of parsers such as LL(k) - i.e. it can parse **everything** they can parse and more
- Can be implemented as efficiently as any other parser

**Disadvantages**
- Difficult to implement by hand - must use a **parser-generator** (e.g. Yacc)

>[!note]
>For a grammar to be LR(k), we must be able to recognise the occurrence of the right side of a production in a right-sentential form, with $k$ input symbols of look-ahead

- This requirement is **far less stringent** than that for $LL(k)$ grammars.
- Thus LR grammars are a proper **subset** of the class of grammars that can be parsed with predictive LL methods

![[Pasted image 20251022121841.png]]

An LR parser makes decisions by **maintaining states** to keep track of **where we are** in the parse

---

Each **state** maintained by an LR parser represents a set of 'items' where an 'item' indicates **how much of a production we have seen**
- An item of a grammar $G$ is a production of $G$ with a dot ($\cdot$) at some position of the body - we call this an $LR(0)$ **item**
- e.g. for a production $A\rightarrow XYZ$:
- ![[Pasted image 20251022122046.png]]
- ![[Pasted image 20251022122122.png]]
- The production $A\rightarrow \epsilon$ only has $A\rightarrow \cdot$ as an item

### LR(0) Automaton

![[Pasted image 20251022122302.png]]

![[Pasted image 20251022122312.png]]

![[Pasted image 20251022122319.png]]

![[Pasted image 20251022122854.png]]

For $I_{4}$ you need to take the **closure** of $F\rightarrow (\cdot E)$
- That's why it has so many in that state

In the end..

![[Pasted image 20251022123113.png]]

The LR(0) automaton can now **be used** with shift-reduce **decisions**
- if a string of $\gamma$ symbols takes the automaton from the start state $0$ to some other state $j$, then **shift** on next input symbol $a$if $j$ has a transition on $a$
- Otherwise choose to **reduce** - the items in state $j$ will tell us which production to use

We can codify these decisions in a table - called the **LR(0) parse table**

![[Pasted image 20251022123606.png]]

In this example we can see that there are two states where there is a shift-reduce **conflict**
- Doesn't give enough 'context' to decide whether to **shift or reduce**

We can resolve this with the more powerful **SLR(1) parse table** - using the **next symbol** and **FOLLOW set**

![[Pasted image 20251022123920.png]]

### LR Parsing Algorithm
An LR parser consists of an **input**, an **output**, a **driver program** and a **parsing table** that has two parts (ACTION and GOTO)

![[Pasted image 20251022124657.png]]

The **parsing program** is the same for all $LR$ parsers, **only the parsing table changes** from one parser to another
- The parsing program **reads characters from an input buffer** one at a time
- The **stack hold states** from the automaton

-- Example in slides --

TODO: Rest of slides (160+)