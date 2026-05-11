# 1
![[Pasted image 20231110233251.png]]

Knowledge base $KB$ **entails** sentence $a$ iff $a$ is **true** in **all** worlds where $KB$ is **true**

| $C$ | $B$ | $A$ | $A\lor B$ | $B\lor \lnot C$ | $\lnot B \lor C$ | $KB$ | $\alpha$ |
| --- | --- | --- | --------- | --------------- | ---------------- | ---- | -------- |
| T   | T   | T   | T         | T               | T                | T    | T        |
| T   | T   | F   | T         | T               | T                | F    | T        |
| T   | F   | T   | T         | T               | T                | T    | T        |
| T   | F   | F   | F         | F               | T                | F    | T        |
| F   | T   | T   | T         | T               | F                | F    | T        |
| F   | T   | F   | T         | T               | F                | F    | F        |
| F   | F   | T   | T         | T               | T                | T    | T        |
| F   | F   | F   | F         | T               | T                | F    | T         |

Since wherever $KB$ is true, $\alpha$ is also true, $KB$ |= $\alpha$
# 2
![[Pasted image 20231111114426.png]]

Planning is a higher-level process of generally setting a goal to achieve for the agent, taking in information from the environment around the agent and working out what the best steps would be to execute that plan optimally
Problem solving is a process of solving more specific problems, potentially within the process of planning, in order to find the solution to something that is necessary to complete the problem.

**Planning**
- Flexible
- More complex
- Can look at sub-problems
- Look at actions more closely
**Search**
- Sequence of actions from start to finish
- State is represented completely
- Black-box of action, states, heuristic functions
# 3
![[Pasted image 20231111120832.png]]

- Selects an **open** precondition (of the step needed to get to the finish)
- Chooses an **action** which has the **effect** of fulfilling that **precondition**
- Record a **causal link** to the achieved precondition
- Add the operator to the set of **ordering constraints**
- Resolve any **threats** to causal links, and if we fail to, backtrack
# 4
![[Pasted image 20231111124048.png]]

A clobbering effect would happen if the causal link of one effect inhibits the precondition for another state or action. Resolve it by backtracking and completing the action that may be inhibited first, and then the second action 
(promote/demote).
# 5
![[Pasted image 20231111124914.png]]

![[Pasted image 20231108103810.png]]
# 6
![[Pasted image 20231116150353.png]]

Tackles the issue of having **incomplete** information
- Conditional actions based on what information you get
Use a **sensing** action which can gauge the environment and gain information

# 7
![[Pasted image 20231116155420.png]]

Action monitoring - Check if all preconditions of next action have been met
- Monitor so you don't have to backtrack after
Plan monitoring - Only check preconditions for actions that have already been executed