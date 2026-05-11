# 1
![[Pasted image 20231110105313.png]]

4 Schedules:
{4, 3, 2, 1} = 12
{4, 2, 1} = 7 - Optimal
{4, 1} = 8
{4, 3, 1} = 10

![[Pasted image 20231110105707.png]]

$f(n)=2^{n-2}$

- For every additional one, new dimension to representation with 2x2 (in, not in) grid

![[Pasted image 20231110110459.png]]

$OPT(i)=$ shortest path from $i$ to 1
$OPT(i)=min_{1\leq i<j} (OPT(j)+P(i,j))$ $\leftarrow O(i)$
In total,
$\Sigma_{i=1}^{n}O(i)=O(n^2)$
# 2
![[Pasted image 20231110171735.png]]

![[Pasted image 20231110171741.png]]

![[Pasted image 20231110172342.png]]

![[Pasted image 20231110172022.png]]

![[Pasted image 20231110172359.png]]

![[Pasted image 20231110172633.png]]

Top-down:
$OPT(u)=min(\Sigma_{i=1}^{l}OPT(u_i),w(u)+\Sigma)$

$OPT-IN(u)$
- Optimal value of the maximum weight independent set **including** u
$OPT-OUT(u)$
- Optimal value of the maximum weight independent set **not including** u

$\text{OPT-IN}(u)=w(u)+\Sigma_{i=1}^{l}\text{OPT-OUT}(u_{i})$
$\text{OPT-OUT}(u)=\Sigma_{i=1}^{l}\text{OPT-IN}(u_{i},\text{OPT-OUT}(u_{i}))$

# 3
![[Pasted image 20231110173243.png]]

![[Pasted image 20231110173555.png]]

{2, 4, 2, 2, 4} finishes at (7, 6)

![[Pasted image 20231110173604.png]]

Trivial, $O(n^2)$:
```js
for i = 1...n:
	for j = 1...m:
		Find length of longest joint segment ending at (i,j) 

Maintain maximum running through
```

$$Opt(j)=\begin{cases} 

OPT(i-1, j-1)+1 & \text{if} x_{i}=y_{i} \\

0 & \text{if otherwise} 


\end{cases}$$

