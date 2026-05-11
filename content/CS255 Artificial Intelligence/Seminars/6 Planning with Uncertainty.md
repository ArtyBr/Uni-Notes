# 1
![[Pasted image 20231130150508.png]]

$P(A,B)=P(A\land B)$

Define the **events**
- $C$: Contracting disease
- $JP$: Joint pain
- $TP$: Testing positive

We have **given**:
- $P(C)=0.0001$

- $P(JP|C)=0.64$
- $P(JP|\lnot C)=0.6$

- $P(TP|C)=0.99$
- $P(TP|\lnot C)=0.04$

Want to find: $P(C|TP,JP)$

Using Bayes':
- $P(C |TP,JP)=\frac{P(JP,TP |C)P(C)}{P(JP,TP)}$
	- $P(JP,TP|C)=P(JP|TP,C)P(TP|C)$
	- $=P(JP|C)P(TP|C)$ 
		- Can assume conditional independence in terms of the **test** not **causing** the joint pain - reasonable assumption
- $P(C|TP,JP)=\frac{P(JP|C)P(TP|C)P(C)}{P(JP,TP)}$
	- $P(JP,TP)=P(JP,TP,(=T))+P(JP,TP,(=F))$
	- $=P(JP,TP|C=T)P(C=T)+P(JP,TP|C=F)P(C=F)$
	- $=P(JP|C=T)P(JP|C=T)P(C=T)+P(JP|C=F)P(JP|C=F)P(C=F)$
- $... = 0.0026$

![[Pasted image 20231130152816.png]]

Would definitely be a better guess

Want to find: $P(C|TP_{1},TP_{2},JP)$
- $=\frac{P(TP_{1},TP_{2},JP|C)P(C)}{P(TP_{1},TP_{2},JP)}$
	- $P(TP_{1},TP_{2}|C)=P(TP_{1}|C)\times P(TP_{2}|C)$

![[Pasted image 20231130153319.png]]

$n$ times:
- $P(C|TP^{n},JP)$
- $=\frac{P(JP,TP^{n}|C)P(C)}{P(JP,TP^{n})}$
- $=\frac{P(JP|C)P(TP|C)^{n}P(C)}{P(JP|\lnot C)P(TP|\lnot C)^{n}P(C)}$
- Needs to be larger than 0.5, so set = to 0.5 and find $n$ (take log)

# 3

![[Pasted image 20231130154616.png]]

Variable elimination:
- Find facts
- If variable is known, eliminate from factors
- If variable unknown:
	- Multiply factors
	- Marginalise out
- Normalise via $\frac{n}{P(e)}$

