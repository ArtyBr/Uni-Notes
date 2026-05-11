Functional dependencies are a formal tool that allows derivation of 'good' DB designs
#### Notation
- $X, Y, Z$ represent sets of attribute
- $A, B, C$ represent single attributes
- Notation $ABC$, implies ${A, B, C}$

$X \rightarrow Y$ = "$X$ Functionally determines $Y$" or "$Y$ is functionally determined by $X$"
- If $X \rightarrow Y$ the values of the $Y$ component of a tuple **depend** on values of the $X$ component
- So, $X \rightarrow Y$ specifies that whenever two tuples of $R$ agree on values of all the attributes of $X$, then they must also agree on values of attribute of $Y$ 
- Formally, $t1[X] = t2[X] \rightarrow t1[Y] = t2[Y]$

Database relations can either be defined using functional dependencies to define them or manually defining them separately

Popular exam question:
 - Given functional dependencies, tell me the keys (and reverse)

