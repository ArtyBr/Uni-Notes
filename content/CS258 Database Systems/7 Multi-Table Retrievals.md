#### Without the `WHERE` clause
Selecting from **multiple** tables **without** `WHERE` returns a **direct concatenation** of the rows of all of the tables, so the size is exactly |`Table_1`| $\times$ |`Table_2`| $\times$ ... $\times$ |`Table_n`| (*cartesian product*)
###### Example
![[Pasted image 20231015223333.png]]

![[Pasted image 20231015223402.png]]
#### With the `WHERE` clause
We use `WHERE` to **limit** the **size** of these concatenations so we get the results that we actually **want**

![[Pasted image 20231015223534.png]]

We **distinguish** attributes of the **same name** by using the semantics `<relation>.<attribute>` 
###### Example
- Using `Likes(drinker, beer)` and `Frequents(drinker, bar)`:
	- Find **beers** liked by people who frequent **Joe's bar**
```sql
SELECT beer
FROM Likes, Frequents
WHERE bar = "Joe's Bar" AND 
	  Frequents.drinker = Likes.drinker
```


Such multi-relation queries are used to **join** tables on specific condition (**joint attributes**) specified in the `WHERE` clause
#### Operational Semantics
- Imagine **one** tuple-variable for **each** relation in the `FROM` clause
- These tuple-variables visit each **combination** of tuples, one from each relation
- If the tuple-variables are pointing to tuples that **satisfy** the `WHERE` clause, send these tuples to the `SELECT` clause
###### Example

![[Pasted image 20231015224430.png]]
#### Explicit tuple-variables
At times we need to refer to **two or more** copies of the same relation
- **Distinguish** copies by following the relation name by the name of a **tuple-variable** in the `FROM CLAUSE`
###### Example
- Given `Beers(name, manf)`: Find all pairs of beers produced by the same manufacturer
	- **Avoid** pairs with the **same** elements like `(bud, bud)`
	- Produce pairs in **alphabetical** **order**
```sql
SELECT b1.name, b2.name
FROM Beers b1, Beers b2
WHERE b1.manf = b2.manf AND b1.name < b2.name
```

SQL permits `AS` between relation and its tuple variable
- e.g. put in `FROM` clause
```sql
Beers AS b1, Beers AS b2
```
