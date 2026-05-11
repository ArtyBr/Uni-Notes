`CREATE SCHEMA studentInfo AUTHORIZATION 'Jsmith'`
- This is used for **SysAdmin**/**DBA**
- **Tables** are stored **within schemas** - even though we have not specified one so far, they have existed inside a 'working' schema

Many SQL DBS store schemas within a **catalogue/dictionary**
- These return essentially all the info during the `CREATE TABLE` command
Alternatively, some DBMS have a special schema `INFORMATION_SCHEMA` with the metadata

![[Pasted image 20231024091347.png]]

#### Assertions 
Type of **constraint** to be enforced by the DBMS
- They apply to the whole database state (not just an attribute or tuple)
- Satisfied as long as **no combination** of tuples in database **violates** it
- Tend to be more **heavyweight** than other mechanisms (e.g. `NOT NULL` or `CHECK` in `CREATE SCHEMA`), so primarily to be used only if other options do not suffice

![[Pasted image 20231024091541.png]]

#### Views/virtual tables
An SQL view is a table **derived** from **other tables**
- or other defined views
Can be **virtual**
- If it exists physically, the view is said to be **materialised**

- Useful for **frequently specified queries**
- Useful for **authorisation** - use view to **present subset** of data, and **restrict** **access** to view
- Can be **queried** like tables
- Automatically updated by DBMS when **base** relations are updated

`CREATE VIEW <name> AS <query>;`

![[Pasted image 20231024094112.png]]
##### Examples

![[Pasted image 20231024092755.png]]



### Overview
![[Pasted image 20231026160811.png]]