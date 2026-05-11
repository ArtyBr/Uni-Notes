Joins allow us to query data that is combined from **multiple** tables **simultaneously**
### `USING` keyword
For some joins may see use of keyword `USING` rather than `ON` 
- Specifies attributes to match on **equality** between **both** tables
- Attribute/Column Name must **exist** in **both** tables
- Can list **more than one** tuple to match upon

![[Pasted image 20231022213043.png]]
## Inner Joins
### Inner in general
The (default) join operator we have seen thus far is called an **inner join**, for which there also exists an explicit operator
```sql
SELECT <attList>
FROM <table1> INNER JOIN <table2>
	ON <expr>
```
- An inner join will **combine** the rows from `<table1>` with the rows from `<table2>` based on satisfying the `ON` boolean expression
- Expression usually takes the form of a **comparison** between a column from table1 and a column from table2
![[Pasted image 20231022194622.png]]

Functionally same as:
```sql
SELECT *
FROM students, studentCourses
WHERE students.studentID = studentCourses.studentID
```

- Join formatting more **readable**
- Using `ON` separates clauses that deal with *sources* of data (i.e. belonging to `FROM`) away from clauses that deal with *selection* criteria (in `WHERE`)
### Product/Cross Join
`R CROSS JOIN S`
- Every tuple of `R` is **concatenated** with **every** tuple of `S` and then output
Syntactically the same as `R, S`
### Theta Joins
Extension of natural joins
- Allow us to combine selections and a product in 1 operation
`R JOIN S ON <condition>`
- `<condition` is used for selection
	- e.g. condition: `R.B = S.B`
	- If operator is "=", it is called **equi-join**
	- Any other operator can also be used
	- Theta joins typically refer to **equi-** and **non-equi** joins
- Example:
	- `Drinkers(name, addr), Frequents(drinker, bar)`
	- `Drinkers JOIN Frequents ON name = drinker;`
	- returns all `(d, a, d, b)` quadruples such that drinker `d` lives at address `a` and frequents bar `b`
## Outer Joins
### Reason for Outer Joins
Inner Joins can '**lose**' information, because a tuple that doesn't join with any from the other relation **disappears** from the join result
- Suppose you join `EMPLOYEE `and `DEPT` on attribute `dept_id `and for some employees the `dept_id `value is `NULL`.
- If you wanted info from such employees as well you need use **outer** joins
- The null value `NULL` can be used to “*pad*” such tuples (called **dangling** tuples) so they appear in the join result.
- So, in outer joins, some tuples that do not “join” are **kept**.
- Variations: `left`- and `right-outerjoin` (keep only dangling tuples from the **left** (respectively, **right**). 
	- Every tuple of the **left** (resp. **right**) table **must** appear in result 
	- If it has no matching tuple in the right (left) table, values for all attributes of right (left) table are set to `NULL`.
### Outer Joins
Syntax:
```sql
SELECT <attList>
FROM <table1> [LEFT|RIGHT] OUTER JOIN <table2>
	ON <table1.attName1> = <table2.attName2>;
```
- Outer joins function in a similar manner to inner joins by **matching** on rows **satisfying** the `ON` clause, but handle **non matching** clauses differently: 
	- **LEFT** OUTER JOINS preserve data from the **first** (**left**) table and join them with a row of NULLs from the **second** table
	- **RIGHT** OUTER JOINS preserve data from the **second** (**right**) table and join them with a row of NULLs from the **first** table
`R OUTER JOIN S` is the **core** of an outerjoin expression. 
- It is modified by (**only one** of):
	- Optional `NATURAL` in front of `OUTER`
	- Optional `ON <condition>` after `JOIN`
	- Optional `LEFT`, `RIGHT`, `FULL` before `OUTER`
		- `LEFT` = pad dangling couples of `R` only
		- `RIGHT` = pad dangling couples of `S` only
		- `FULL` = pad both; this choice is the **default**
#### Examples
![[Pasted image 20231022211942.png]]

Brian's studentId does not match to be < anything in the other table so his info is still included but with `NULL` values

![[Pasted image 20231022212029.png]]

![[Pasted image 20231022212042.png]]
## Natural Join
`R NATURAL JOIN S` - Semantically equivalent to:
- Forming the product of `R` and `S`, looking at the attributes of `R` having the same name/type as attributes in `S`
- Keep only tuples from product whose same-name-type attributes have **same** **value**
- Only one of each set of duplicate columns is kept

![[Pasted image 20231019165149.png]]

Natural Joins can be:
- **Inner** Joins
- **Outer** Joins (Left or Right)
They differ in that they do **not specify** an `ON` clause
- Instead they match on the basis of columns/attributes that **share** a name in **both** tables
**Default** is `INNER JOIN`
## Extra Joins
### Semi-Join
### Anti-Join
Join only tuples that **aren't** the same 