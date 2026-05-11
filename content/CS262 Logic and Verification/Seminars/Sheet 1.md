## 1.
Base case - $c(X)$ = 0, $X$ is just a single proposition $a$
so parse tree is just $a$ - therefore $l(x)=1$.
1-1 = 0 so $c(X)=l(X)-1$

Inductive step:
First case:
For $\lnot X$ 
$c(\lnot X)=l(\lnot X)-1$

Second case:
$X \circ Y$
$l(X\circ Y) = l(X) + l(Y)$
$c(X\circ Y)=c(X)+c(Y)+1$
$c(X\circ Y) = l(X)-1+l(Y)-1+1$
= $l(X)+l(Y)-1$
so
$c(X\circ Y)=l(X\circ Y)-1$

## 3.
a - T, not T, 1, 2, not 1, not 2

b - binary operators

c - 
$X \land Y = X \land Y$
$X \land \lnot  Y = X \not\rightarrow Y$ 
$\lnot X \land Y=X\not\leftarrow Y$
$\lnot X \land \lnot Y = X \downarrow Y$
$\lnot(X\land Y)=X\uparrow Y$
$\lnot(\lnot X \land \lnot Y) = X\lor Y$
$\lnot(X \land \lnot Y)=X\rightarrow Y$
$\lnot(\lnot X \land Y)=X\leftarrow Y$

e - 
$X \equiv Y = \lnot(\lnot(X\land Y)\land \lnot(\lnot X \land \lnot Y)) = \lnot (\lnot X \lor \lnot Y) \lor \lnot (X \lor Y)$
$X \lnot \equiv Y = \lnot(X\land Y)\land \lnot(\lnot X \land \lnot Y) = \lnot(X \lor Y) \lor \lnot (\lnot X \lor Y)$

f - 
from c and d proved that can use any number of $\lnot, \land, \lor$ to make them

g -
$X \uparrow Y = \lnot X \lor \lnot Y = \lnot(X \land Y)$
$\lnot X = X\uparrow X$
So can construct all equations as in c and d since have a way to represent $\lnot, \land, \lor$

h - 
If variables are set to true for all inputs, then output will always end up true. So False cannot be achieved using these connectives. Same for other set except for false inputs instead of true.

## 4.
