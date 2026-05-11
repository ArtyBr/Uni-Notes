predicate - likes(X, Y)
- Starts with lowercase letter
Variable - X
- Starts with uppercase letter

`predicate :- condition1, condition2, ...`
- Where ',' means 'AND;

`trace.`, `notrace.`
- toggles how it found answers to commands

- Comparison operators: =:=, =/=, >, <, >=, <=
- Is predicate: assign numerical value of right hand side to left hand side
	- Computes/evaluates the left hand side first then assigns to right hand side
	- e.g.
		- $X=3+5$ 
			- Sets $X$ to the string '$3+5$'
		- $X$ is $3+5$ 
			- Sets $X$ to $8$
			- $X=:=8$
- = unification operator: bind free variables to make them match the other members
- ! - shortcut operator - 

**Lists**
- Notation: [a, b, c]
- Lists in lists: [a, b, [c, d, e]]
- Access first element of list and the rest by [Head|Tail]
- Can be iterated: [H1, H2|Tail]
- Test membership: member(El, List)
- Get nth entry: nth1(Idx, List, El)
- Prolog cut operator: !

