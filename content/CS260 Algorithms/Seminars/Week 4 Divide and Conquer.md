
![[Pasted image 20231021222315.png]]

![[Pasted image 20231021223510.png]]

A: $$T(n)=\begin{cases}

1 & \text{if } n = 1\\

5T(\frac{n}{2})+O(n) & \text{if } n > 1
\end{cases}$$
B: $$T(n)=\begin{cases}

n-1 & \text{if } n = 1\\

nT(\frac{n}{2})+O(n) & \text{if } n > 1
\end{cases}$$

C: $$T(n)=\begin{cases}

1 & \text{if } n = 1\\

9T(\frac{n}{3})+\Theta(n^{2}) & \text{if } n > 1
\end{cases}$$
D: $$T(n)=\begin{cases}

1 & \text{if } n = 1\\

2T(n-1)+O(1) & \text{if } n > 1
\end{cases}$$
![[Pasted image 20231022124917.png]]
- A)
- ![[Pasted image 20231022141306.png]]
- B)
- ![[Pasted image 20231022175059.png]]
- C)
- ![[Pasted image 20231022175113.png]]
- D)
- ![[Pasted image 20231022181237.png]]



