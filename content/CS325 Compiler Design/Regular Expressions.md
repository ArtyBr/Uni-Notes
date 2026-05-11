A Regular Expression (RE) describes a set of strings over the characters contained in some alphabet, $Σ$, augmented with a character ε that represents the empty string. 
- For a given RE, $r$, we denote the language that it specifies as $L(r)$ 
- A Regular Expression is built up from three basic operations 

1. **Alternation**- The alternation, or union, of two sets of strings, $R$ and $S$, denoted by $R | S$ , is ${ x | x ∈ R \text{ or } x ∈ S }$ 
2. **Concatenation**- The concatenation of two sets $R$ and $S$, denoted $RS$ , contains all strings formed by prepending an element of $R$ onto one from $S$, or ${ xy | x ∈ R \text{ and } y ∈ S }$ 
3. **Closure** - The Kleene closure of a set $R$, denoted $R^*$ is $$\bigcup_{i=0}^{\infty}R^{i}$$ The union of the concatenations of $R$ with itself, zero or more times

There are other symbols we use to make it easier to write notate REs
- **One or more instances** - *unary, postfix* operator is $+$
	- e.g. $R+$ denotes one or more instances of $R$
- **Zero or one instance** - *unary, postfix* operator $?$ means 'zero or one occurrence'
	- e.g. $R?$ is equivalent to $R | \epsilon$
- **Character classes** - a RE $a_{1} | a_{2} | a_{3} | ... | a_{n}$ where $a_{i}$'s are each symbols of the alphabet can be replaced by the shorthand $[a_{1}a_{2}a_{3}...a_{n}]$. When it's a logical sequence, can be replaced with $[a_{1}-a_{n}]$ e.g. $[a-z]$
- **Complement operator** - The ^$C$ specifies the set $\Sigma-C$ - the complement of $C$ with respect to $\Sigma$ 
- **Parenthesis** can be used to group parts of a RE with **round brackets**. ()

Examples

![[Pasted image 20251017190354.png]]
