### Set Comparisons
- We can apply principles similar to `IN` for other comparisons across sets
- For example let `<OP>` be one of `=, <>, >, <, >=, <=`
Note: `<>` is the **'not equal to'** operator - `true` if left is not equal to right, otherwise returns `false`
```sql
SELECT <attList>
FROM <tableList>
WHERE <attName/attTuple> <OP> ALL/ANY/SOME <subQuery>
```
### Quantifiers
#### `ANY` or `SOME`
`ANY` behaves as an **existential** quantifier
Syntax: `x <OP> ANY(<relation>)`
- `True` if an only if `x <OP>` returns `true` for **at least one** tuple in the relation
###### Example: `x>= ANY (<relation>)`
Means `x` is **not the smallest tuple** in the relation
- Note in this example tuples must have attribute only
#### `ALL`
`ALL` behaves as a **universal** quantifier
Syntax: `x <OP> ALL (<relation>)` 
- `True` if and only if for **every** tuple `t` in the relation, `x <OP> t`returns `true`
Example: Choose beers which are sold for the **highest price**
```sql
SELECT beer
FROM Sells
WHERE price >= ALL(
					SELECT price
					FROM Sellls
					);
```

```sql
SELECT courseid
FROM (studentcourses NATURAL JOIN students) a
WHERE 
'Bob' <> ALL(
	SELECT studentname
	FROM (studentcourses NATURAL JOIN students) b
	WHERE a.courseid = b.courseid)
```

```sql
SELECT beer
FROM sells
WHERE price >= ALL(SELECT price
				FROM sells)
```
### Correlated Subqueries
- The subqueries we have seen up to now are rather independent of the **outer** query
	- **Subqueries** can be evaluated once and then results are used in the outer query 
- Oftentimes, a **nested** query need refer to attributes of tuples in the tables of the outer query
	- The semantics here are different: 
		- We want the **nested** query to be **evaluated** **per tuple** of the **outer** query
#### `EXISTS`
- Allows us to conduct *emptiness* testing
- The presence of any value in the result set of the nested query causes the `EXISTS` statement to resolve to `TRUE`
- Note - a `NULL` in the result set means the set is not empty - the `EXISTS` will resolve to `TRUE`

Syntax `EXISTS( <relation>)`
- Evaluates to true – if and only if the `<relation>` is **not** empty.
```sql
SELECT courseID 
FROM courses C 
WHERE EXISTS (
				SELECT * 
				FROM studentCourses D 
				WHERE C.courseID = D.courseID
			);
```

- The inner query referred to an attribute from the outer query 
- We say the queries are correlated 
- The nested inner query is evaluated once for each tuple in the outer query 
- Scope principles apply to **all** nested queries 
- `EXISTS` can be used to test for **emptiness** as well as **non-emptiness** (e.g. use `NOT`)

```sql
SELECT beer
FROM beers a
WHERE NOT EXISTS (SELECT beer
				 FROM beers b
				 WHERE a.name <> b.name
				 AND a.manf = b.manf)
```

![[Pasted image 20231019162150.png]]

### Set operators and Subqueries
- Viewing the results of subqueries as sets, allows us to think of queries which combine the results for subqueries using set operators. 
- Operators: `Union`, `Intersection`, and `Difference` 
These are expressed by the following **forms**, each involving subqueries:
- ( subquery ) `<operation>` ( subquery )

```sql
(SELECT *
FROM likes)
INTERSECTION
(SELECT drinker, beer
FROM frequents NATURAL JOIN sells)
```
#### Example
![[Pasted image 20231022213154.png]]

![[Pasted image 20231022213204.png]]


### Bag vs Set semantics
The SQL Select-From-Where statement uses in general **bag** semantics. 
- However, union / intersect, and difference are **set** operations.
	- The **default** for union, intersection, and difference is hence **set** semantics


```sql
SELECT drinker
FROM (SELECT drinker, COUNT(beer) cl
	 FROM likes
	 GROUP BY drinker) l NATURAL JOIN
	 (SELECT drinker, COUNT(bar) cf
	 FROM frequents
	 GROUP BY drinker)
WHERE f.cf > l.cl
```

Using difference:

```sql
(SELECT drinker
 FROM frequents)
 DIFFERENCE
 (SELECT drinker
 FROM likes)
```

