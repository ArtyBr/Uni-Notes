A **Universal Turing Machine** $U$ takes as input the string "$ENC(M)\#w$" and **simulates** $M$ on $w$
- We just write $<M,w>$
We can define many interesting languages using Universal Turing Machines
# The **Halting problem**
- Given a **word** $x$ and **machine** $M$, will $M$ **halt** on the word $x$?
Is it **recognisable?**
- Given a Machine $M$ and input string $x$, can we make a machine that accepts if $M$ **halts** on $x$?
	- But if it loops on non-halting results, that's fine
- ![[Pasted image 20240602174217.png]]
- Simulate the machine $M$ on $x$, and if it **halts** then **accept**, otherwise don't care - can either reject or accept (will loop since it's checking if it halts)

Is it **decidable?**
- Given a Machine $M$ and input string $x$, can we make a machine that determines if $M$ **halts** on $x$?
	- Either **accepts** or **rejects** - **cannot loop**
- Formally:
	- ![[Pasted image 20240602173236.png]]
No, by **diagonalization** (Cantor's Theorem):
- Make a table describing the behaviour of **every** TM on **every** word:
	- ![[Pasted image 20240602173355.png]]
- Want to prove that there is no decider/total TM for HP
- So, assume it's **true**
- Now, make a new machine $k$ such that when looking at each machine $y$'s run on the corresponding word $y$ and construct it such that when run on every word, reverse the output that $M_y$ gets on $y$.
	- i.e. if $M_y$ **halts** on $y$, make this new machine **loop** on $y$ 
	- and if $M_y$ **loops** on $y$, make this new machine **halt** on $y$
	- ![[Pasted image 20240602173705.png]]
- Now we have a machine that **didn't exist in the table**, so the claim **must be false**
- $\implies$ HP is **not decidable**

# The **Membership problem**
- Given a string $x$ for a machine $M$, check if $M$ **accepts** on $x$ (not enough for it to **halt** on $x$)

Is it **recognizable**?
- Just simulate $M$ on input $x$
	- If $M$ ever accepts $x$, machine says **yes**
	- otherwise it doesn't have to say anything at all

Is it **decidable**?
- Reduce $HP$ to $MP$
	- Construct a new machine $M'$ obtained from $M$ as follows:
		- **Add** a new **accept state** to $M$
		- Make all incoming transitions to eh **old accept and reject states** go to the **new accept state**
	- Simulate the assumed decider $N$ on input $<M',x>$ and accept iff $N$ accepts
