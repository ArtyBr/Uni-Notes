A **modification** command is *not* a query
- It does not return a result (as a query does) but it changes the database state in some way
These kind of modifications:
- **Insert** tuple(s)
- **Delete** tuple(s)
- **Update** the value(s) of existing tuple(s)
#### Insertion
```sql
INSERT INTO <tablename>
 VALUES (<val1>, ..., <valn>)
```
- Each list refers to a record (tuple) being inserted
- Each value belongs to the attribute of the **same position** in the attribute list of the schema
#### Inserting Partial Records
```sql
INSERT INTO <tablename> <A1, ..., Aj>
 VALUES (<val1>, ..., <valn>)
```
- <A1, ..., Aj> - **Partial Attribute List** (Subset of Schema Attribute List
Two reasons to do this:
- We **forget** the standard order of attributes for the relation
- We **don't have** values for all attributes, and we want the system to **fill in** missing components
Values for **unspecified** attributes are set to either:
- `NULL`
- Default value
#### Deletion
```sql
DELETE FROM <tablename>
 WHERE <expression>;
```
Deletes **entire tuples** from the table for which the expression is **True**

If you do
```sql
DELETE FROM <tablename>;
```
**All tuples** will be deleted from the table - doesn't delete the table, just all of the **values for the attributes**

There is also:
```sql
DELETE FROM <tablename>
WHERE EXISTS
	(SELECT name
	 FROM tablename
	 WHERE <expression>
	 )
```
`EXISTS T` returns `TRUE` only if table `T` is not empty
#### Updating DB tuple(s)
```sql
UPDATE <tablename>
SET <attName1> = <val>, <attName2> = <val>...
WHERE <expression>;
```
Can set to `NULL`, `DEFAULT` and use arithmetic operations
```sql
SET attName = { expression | default | null }
```
