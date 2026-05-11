When **querying/updating**, SQL is a **declarative language**
- Says 'what to do' (declarative) rather than 'how to do it' (procedural)
SQL is both:
- A **Data Definition Language (DDL)**: Schemas, relations, constraints..
- A **Data Manipulation Language (DML)**: Updates, queries
#### Schemas and Catalog
`CREATE SCHEMA` statement
- `CREATE SCHEMA COMPANY AYTHORIZATION 'Jsmith';`
Catalog
- Named collection of schemas in an SQL environment
#### CREATE TABLE
**Provide**
- Name of table
- Attributes; their types
- Any constraints
**Base** **tables** (relations)
- Relation and its tuples are actually created and stored as a file by the DBMS
**Virtual relations** (views)
- Created through the `CREATE VIEW` statement
- Do not (necessarily correspond to any physical/permanent file)

![[Pasted image 20231009131800.png]]
###### Example
``` SQL
CREATE TABLE students (
	studentID INTEGER PRIMARY KEY,
	studentName CHAR VARYING(30),
	courseID INTEGER
);
```
#### DROP
`DROP TABLE <tableName>;`
- This will delete the table - i.e. the entire relation, **including the schema and the data**

![[Pasted image 20231024091624.png]]
### ALTER Table 
If DB doesn't allow alter, can create copy and make modifications
#### ADD column
```sql
ALTER TABLE <tableName>
ADD <attName> <dataType> <constraints>;
```
- New column will be **filled** **initially** with `NULLS` for **each existing row**

![[Pasted image 20231024092014.png]]
#### DROP column
```sql
ALTER TABLE <tableName>
FROP COLUMN <attName>
```
- This also **destroys** the **data** for the column/attribute

![[Pasted image 20231024092102.png]]
#### Change Datatype

![[Pasted image 20231024092134.png]]
#### Renaming

![[Pasted image 20231024092209.png]]
#### Constraints

![[Pasted image 20231024092228.png]]

- **Cannot modify** an **existing** constraint
- To **change** constraints:
	- **Drop** existing constraint
	- **Recreate** constraint, with any desired modifications
