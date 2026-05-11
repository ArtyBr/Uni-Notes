**Lexing** (scanning) *transforms* a **stream of characters** into a **stream of words** (*tokens*)

![[Pasted image 20251017124344.png]]

**Input**:
- **Sequence of characters**

![[Pasted image 20251017124439.png]]

**Output**:
- A **sequence of tokens**
	- ![[Pasted image 20251017124521.png]]
- Begins to maintain a **symbol table**
	- ![[Pasted image 20251017124532.png]]

--- 

Steps:
- **Scan** the input one char at a time
- **Remove** whitespace
- **Group** characters into meaningful sequences called **lexemes**
- For each **lexeme**, produce as output a **token** of the form ` <token-name, optional attribute-value>`
- Attempt to find the *longest* possible **legal token**
- **Error recovery/reporting**

The stream of tokens is sent to the **parser** for **syntax analysis**
- `getNextToken` - Get the lexer to read characters from its input until it can identify the next **lexeme** and produce for it the next **token**, returned to the parser

Additionally the lexical analyser interacts with the **symbol table**
- When a **lexeme** constituting an **identifier** is detected, that lexeme is **entered** into the **symbol table**
- Sometimes the symbol table assists in determining the **correct token** to be passed to the parser.

**Lexical errors** are *invalid tokens* or *malformed literals* (e.g. missing quotes around text intended as a string)

---
# Terminology

**Token** - a pair consisting of a **token name** and an optional **attribute value**
- `<name, opt-attrib-val>`
- Token name is an **abstract symbol** representing a *kind* of lexical unit (e.g. keyword, identifier)
- Attribute value - e.g. token **number** matches both 0 and 1, then the attributes of the token are 0 and 1

**Lexeme** - a sequence of characters in the source program that matches the pattern for a token and is identified by the lexical analyser as an **instance of that token**

**Pattern** - Description of the form that that lexemes of a token may take
- For a keyword (e.g. `if, else, while`) as a token, the pattern is just a sequence of characters that form the keyword
- For identifiers and some other tokens, th epattern is a more complex structure that is **bmatched** by many strings

![[Pasted image 20251017183803.png]]
Examples

For the following example:

![[Pasted image 20251017183819.png]]

`printf` and `score` are **lexemes** matching the pattern for token `id`.

`"Total=%d/n"` is a **lexeme** matching a `literal` 

The pattern for token **number** matches both `0` and `1`, in this case the lexer returns the token together with an attribute that describes the lexeme that was matched
- `<number, 0>, <number, 1>`

The token **name** influences **parsing decisions** (in syntax analysis)
The **attribute value** influences **translation** of tokens after the parse (semantic analysis)

**Operators**, **punctuation**, and **keywords** usually do *not need* an attribute value
Matching identifiers (**id**s), usually get an entry into the **symbol table** and a **pointer to that entry** as the **attribute** of the token

The following is an expression and its corresponding tokens:

![[Pasted image 20251017184544.png]]

![[Pasted image 20251017184526.png]]

---

# Overview of how lexing works

**Recognizers** - Programs that **identify words** in a stream of characters

**Regular expressions** (RE) - A formal notation for **specifying a recognizer**

---

Essentially, recognizers can be represented as **finite automata** 
- Link from CS259: [[1 Deterministic Finite Automaton (DFAs)]]

**Regular expressions** are used to create *concise* notations for recognizers.
- Link to REs: [[Regular Expressions]]

However, we need a way to turn a **regular expression** *into* a **machine-understandable recognizer**, (a DFA)

Given we can represent words with REs, we can write a **specification** for each of the **token classes** in the language:

![[Pasted image 20251017190647.png]]

Next, we construct an RE matching **all lexemes for all tokens** - simply take the **union** of all the REs for all token classes

![[Pasted image 20251017190842.png]]

Now that we have a RE $R$ for matching all lexemes for all tokens, the steps taken by the Lexer to **tokenize** an input sequence of characters is as follows:

1. Given an input sequence of characters $C_{1}, C_{2}, C_{3}, C_{4},... C_{n}$ to the lexer, we **check** whether some $i$ $(1\leq i\leq n)$ number of characters **belongs** to the **language** of $R$, $L(R)$
	- Check if $C_{1}, ..., C_{n} \in L(R)$
2. If true, we know that $C_{1}, ..., C_{n}\in L(R_{j})$ for some $j$ (i.e. $R_{j}$ is one of $R_{1}, R_{2}, R_{m}$ from the languages of tokens we're checking for)
3. **Remove** $C_{1}, ..., C_{i}$ from the input sequence and **go to step 1**.

What if different numbers of characters match $R$?

![[Pasted image 20251017191325.png]]

- We take the **longest** sequence - **maximal munch**

What about if more than one token matches?
- e.g. "for" can be both a **keyword** and an **identifier**

Solution - we use the token class specification **listed first**

If there is no match:
- Raise an **error**
- Errors can be defines using another regular expression that specifies strings that **don't belong** in the language

---
# Creation of a scanner

There exist lexical analyser **generators**, e.g. Lex or Flex, which **automate** the scanners.

However, implementation of this software requires the **simulation** of a **DFA**.

We need to **transform a RE into a DFA** for **direct implementation**

![[Pasted image 20251017204625.png]]

We need to:
- Convert the RE into an NFA
- Convert the NFA into a DFA

Converting to NFA:
- Use a **template** for building an NFA as such:

![[Pasted image 20251017204923.png]]

Now, to convert from NFA to DFA, we use **subset construction**
- Link to CS259: [[3 NFAs]]

Then, we use **Hopcroft's algorithm** to convert a DFA to a **minimal DFA**
- The DFA that emerges from subset construction can have a **large number of states** (up to $2^{N}-1$ for an $N$-state NFA)
- Some states from the DFA can be **merged**

![[Pasted image 20251017205858.png]]

![[Pasted image 20251017205929.png]]

---

All the formalisms and algorithms we have learnt allows us to **automate the construction** of the **scanner**
- The **compiler writer** creates an **RE** for each **syntactic category**
- Give the REs as **input** to a **scanner generator** (e.g. Lex or Flex)
- **Scanner Generator** builds NFA -> DFA -> minimal DFA

At this point, the scanner generator must **convert** the DFA into **executable code**
Strategies are:
- **Table-driven** scanner
- **Direct coded** scanner
- **Hand-coded** scanner

# Table driven scanners

![[Pasted image 20251017210208.png]]

![[Pasted image 20251017210217.png]]