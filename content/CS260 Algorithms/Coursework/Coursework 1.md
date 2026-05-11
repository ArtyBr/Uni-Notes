
a) ![[Pasted image 20231031202306.png]]
b) 
```
pothole list = [n1... nk]                       O(1)
distance covered = 0                            O(1)
i = 1                                           O(1)
While i < size of pothole list:                 O(n)
    current pothole = pothole list[i]           O(1)
    if current pothole > distance covered:
        Place a slab with the left end just covering the pothole                                         O(1)
        distance covered = current pothole + 1  O(1)
    i = i + 1                                   O(1)
```
**Proof**:
Given that every slab has a pothole at its left edge, a suboptimal covering of potholes happens in the following cases:
    1. A slab is placed that doesn't cover any potholes
    2. 2 slabs cover a range of potholes that are at most 1m apart (They could be covered by just 1 slab)
My algorithm doesn't place a slab in either of these cases:
    1. My algorithm places slabs only when a pothole is present, placing the left edge of the slab on the pothole
    2. Slab n only placed if slab n-1 is placed is at least 1m away, and since slab n-1 must have a pothole at the left edge of it, the potholes covered by slab n must be at least 1m away from this very leftmost pothole under slab n-1

Since this basically just iterates through the list of potholes which takes O(n) time, with all other operations being O(1), the algorithm runs in O(n) time

c)
![[Pasted image 20231031202808.png]]
6 Slabs

d)
1. It doesn't ensure the condition for slab[i] to be the same as slab[i+2]
2. Doing this greedily, like how the AI has done it, doesn't always return an optimal number of operations.
For example, if the 3...m slabs were already in an optimal order but the first 2 weren't, this algorithm would change the color of the first 2 and potentially cause a chain reaction causing it to change the color of all the rest of the slabs, unnecessarily

e)
Starting arrangement: [R, G, B, G, R, Y, P]
Final arrangement:    [R, G, R, G, R, G, R]

Count the number of each coloured tile in the even indexes and the odd ones. *O(n)*
Choose pair of tile colours, one from each list, whose tallies add up to the highest number, but which aren't the same colour (so choose second best pair if both same colour). *O(nlogn)*

Optimal:

Final arrangement of the algorithm colours the tiles is such that:
- There is only one colour in the even tiles
    - due to rule that slab[i] = slab[i+2] 
- There is a different colour in the odd tiles 
    - due to rule that slab[i] =/= slab[i-1]

An optimal algorithm colours the slabs in this way, and for the even colour repaints the least amount of slabs to this colour, and same for odd slabs/colour.
This means that the optimal colour we choose to repaint the even slabs should be the colour that appears most often in the even spaces in the original arrangement, and same for the odd slabs.

Since my algorithm counts the number of slabs painted in each colour in the even and odd spaces, and maximises the combination of the two, while keeping completeness by only choosing them only if they are different colours, it must be optimal.

Time complexity = O(n) + O(nlogn) = O(nlogn)

f)
The algorithm takes an arbitrary array as an input and outputs an ordered array with specific conditions to the ordering.
For any algorithm that transforms one such array of elements of size n into another such array, the fastest it can possibly be is O(n) as it at the least it needs to iterate through the array in order to check that it follows the desired conditions.