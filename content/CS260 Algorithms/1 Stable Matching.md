**Perfect matching**: Everyone is matched **bijectively**

**Stability**: no incentive for some pair of participants to undermine assignment by joint action

**Stable matching**:Perfect matching with no unstable pairs

**Problem**: Given the preferences list of n doctors and hospitals, find a stable matching if one exists

### Propose-and-reject algorithm
Intuitive method that **guarantees** to find a stable matching

```js
Initialize each entity to be free. 
while (some doctor is free and hasn’t applied to every hospital) {
	Choose such a doctor d 
	h = first hospital on d's list to which d has not yet applied
	if (h is free)
		assign d and h to be matched 
	else if (h prefers d to current match d') 
		assign d and h to be matched, and d' to be free 
	else 
		h rejects d 
}
```

#### Proof of Correctness: *Termination*
- **Observation 1**: Doctors apply to hospitals in decreasing order of preference
- **Observation 2**: Once a hospital is matched, it never becomes unmatched; they only get an improved match
- **Claim**: Algorithm terminated after at most $n^2$ iterations
- **Proof**: Each time through the while loop a doctor applied to a new hospital. There are only $n^2$ possible pairings
#### Proof of Correctness: *Perfection*
**Claim**: *All doctors and hospitals get matched*
**Proof**:
1. Suppose that doctor $D$ is not matched upon termination of algorithm
2. Then some hospital, say $H$, is not matched upon termination (since there are $n$ doctors and $n$ hospitals)
3. By **Observation 2**, $H$ was never applied to
4. But, $D$ should have applied everywhere, including $H$, and since $H$ hadn't been applied to before they should have been matched at this point
5. This is a contradiction, so this cannot happen. Therefore the claim is true
#### Proof of Correctness: Stability
**Claim**: *The algorithm does not produce any **unstable pairs***
**Proof**: 
- Suppose $A-Z$ is an unstable pair: each prefers the other to partner in Gale-Shapley matching $S*$ 
 ![[Pasted image 20231008125931.png]]
- Case 1: $A$ never applied to $Z$
	- $A$ prefers their GS pairing to $Z$
	- $A-Z$ is stable
- Case 2: $A$ applied to $Z$
	- $Z$ rejected $A$ (right away or later)
	- $Z$ prefers their GS partner to $A$
	- $A-Z$ is stable
- In either case $A-Z$ is stable - contradiction

### Efficient implementation
- We currently have an $O(n^2)$ time implementation using arrays and queues
- Doctors are named $1, ..., n$ and hospitals are labeled $1', ..., n'$
#### Data structures used to process the applications:
- **List** of free hospitals, e.g., in a queue
- Maintain two **arrays** `doctor_at[h]` and `hospital_of[d]`
	- Set entry to 0 if unmatched
	- If `d` matched to `h` then `doctor_at[h] = d` and `hospital_of[d] = h`
- To handle doctors applying:
	- For each doctor keep a list of hospitals, ordered by preference
	- Keep an array `count[d]` to count applications made by `d`
- To handle hospitals accepting/rejecting:
	- Does hospital `h` prefer doctor `d` to doctor `d'`?
	- For each hospital, create **inverse** of preference list of doctors
	``` js
	// Inverse algorithm:
	inverse = [length(pref)]
	for i = 1 to n
		inverse[pref[i]] = i
	```
	
![[Pasted image 20231008133809.png]]

- Constant time access for each query after $O(n)$ preprocessing



