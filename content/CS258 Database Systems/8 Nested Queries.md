Akin to **subroutines** in procedural programming
A **paranthesised** `SELECT-FROM-WHERE` statement (*subquery*) can be used as a **value**

Reducing the size of the table that you're joining like this can **reduce computation time** massively due to the nature of the **cartesian product operation** when **joining** tables
#### Subquery in `FROM` clause
- **Instead** of **naming** a table in the `FROM` clause, we can name instead a **query**
	- This **nested** query will return a new table - lets call it `NEWT`
	- So the above is equivalent to **creating** a table `NEWT` and then using its name, `NEWT` in the `FROM` clause
	- Good to use **tuple-variable** to name tuples of the result of the nested query
###### Example - Subquery in `FROM`
- Find the beers liked by at least one person who frequents Joe's bar
```sql
SELECT  beer
FROM    Likes,
		(SELECT drinker
			FROM Frequents
			WHERE bar="Joe's bar")JD
WHERE Likes.drinker = JD.drinker
```
#### Subqueries that return one tuple
If a subquery is **guaranteed** to produce **one** tuple
- The subquery can be used as a **value**
Usually, the tuple has **one component**
- e.g. a single tuple is guaranteed by **keyness** of attributes
- A **run-time** error occurs if there is **no** tuple or **more than one** tuple
#### Scalar Subqueries
It is possible to use **arithmetic comparisons** if the subquery returns a single **scalar value**.
- If the query returns just one value, it is like comparing two number
	- Although formally this is still a relation with one row and one column
###### Example
From `Sells(bar, beer, price)`: find
- The bars that serve Miller for the same price Joe's charges for Bud
We can break down the above query into two steps:
- Find the price Joe's charges for Bud - and treat this as a known value thereafter
- Find the bars that serve Miller at that price
```sql
SELECT bar
FROM Sells
WHERE   beer = "Miller" AND
		price = 
			(
			SELECT price
			FROM Sells
			WHERE bar = "Joe's Bar"
			AND beer = "Bud"
			)
```


#### Nesting with the `IN` operator
```sql
<tuple> IN <relation>
```
- Evaluates to true if and only if the tuple is a **member** of the relation
###### Common use-case:
```sql
SELECT <attNameList>
FROM <tableList>
WHERE (
	<attName> IN (SELECT <attName>
				  FROM <tableList>
				  WHERE <expr>)
);
```
#### Subqueries vs Unnested Multi-table Queries
Compare:
```sql
SELECT R.a
FROM R.s
WHERE R.b = S.b;
```
vs
```sql
SELECT R.a
FROM R
WHERE R.b IN (SELECT b FROM S);
```


```sql
SELECT DISTINCT  a.studentid, b.studentid
FROM studentCourses a, studentCourses b
WHERE a.studentid <> b.studentid
AND a.courseid = b.courseid
```