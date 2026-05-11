#### Basic syntax
```sql
SELECT <attribute1, ..., attributei>
FROM <tables>
WHERE <condition>
```
- `<attribute1, ..., attributei>` is a list of attributes to be retrieved
- `<tables>` is a list of the relation names/tables needed to process the query
- `<condition>` is a boolean expression - only return tuples whose attribute values satisfy the condition
```sql
SELECT studentName 
FROM students 
WHERE studentID > 1;
```
- `*` - wildcard, fetches all attributes
##### `DISTINCT` keyword used to remove duplicates from the result
```sql
SELECT DISTINCT StudentName
FROM Students;
```
- Returns a **set** rather than a **bag**
#### Ordering query results
Query results can be ordered using the `ORDER BY` keyword
```sql
SELECT * 
FROM students 
ORDER BY studentID ASC;
```
#### Renaming and using expressions for values in columns
You can rename attributes in the resulting column using `AS` keyword
You can use operations to affect values of attributes in the resulting table
```sql
SELECT bar, beer, price*147 AS priceInYen 
FROM Sells;
```
If you want an answer with a particular string in each row, use that constant as an expression
```sql
SELECT drinker, 'likes Bud' AS whoLikesBud 
FROM Likes
WHERE beer = 'Bud';
```
![[Pasted image 20231010171858.png]]
#### Strings in `WHERE` clauses
- `WHERE` clauses can also have conditions for strings, in which a string is compared with a **pattern**, to see if it matches
	- `<StringAttribute> LIKE <pattern>`
	- `<StringAttribute> LIKE <pattern>`
- Pattern is a quoted string with special characters
	- `$` - any string - stands for **zero or more characters**
	- `_` - any character - stands for **exactly one character**
e.g. From Drinkers(name, addr, phone), find drinkers whose phone has area code 167
```sql
SELECT name 
FROM Drinkers 
WHERE phone LIKE '%167-_ _ _ _ _ _ _’ ;
```
##### `NULL` values
We need to be able to test whether a value is `NULL`. 
- Syntax: `A IS NULL`
	- Use instead of “A = NULL”
	- “Attribute=NULL” is **never true** 
- `IS NOT NULL`
#### Operational semantics - how the computer goes through a query
- Start with the **relation** from the `FROM` clause
- Apply **row-selection** indicated by the `WHERE` clause
- Apply the **column-projection** indicated by the `SELECT` clause
