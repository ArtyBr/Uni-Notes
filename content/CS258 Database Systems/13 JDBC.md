**Procedural programming** for databases
- Programs which **interface** with a database

Different languages
- **Host** language
	- *Java, C* etc.
- **Data** sublanguage
	- DDL/DML - *SQL*

#### Ways of accessing DBs
- **Interactive**: SQL command window
- **File/batch**: File with SQL commands
- **Application programs** (DB apps) - e.g. web interface to interact like *forms*
#### Interfaces to DBs
- **Embedding** SQL commands in an app
- Using a **library** of DB functions/classes
- Developing a **bespoke** DB **programming language**

#### Client-server architecture
- DB runs as **server**
- SQL programmer uses **client** software
- Often the **server** runs on **same host** as the **client**
- Application programs **communicate** with **server** over **network** in the same way as any other client

![[Pasted image 20231026161437.png]]

![[Pasted image 20231026161449.png]]
#### Independence Mismatch problem
- Two languages 'blended' together - *Java* and *SQL*
- Data types in SQL and data types in Java are different
- **Structures** need to be **converted** between SQL and Java and vice versa
## JDBC
JDBC is an **API** that **calls** SQL **commands** from Java
**One** Java program can have connections to **several** DBs
- The *driver manager class* is used to handle **multiple connections** to different DBs
### 3 main classes/tasks
#### **Connection class**
#### Statement class
- Use *PreparedStatement* to make statements
	```java
	String stmt = "SELECT Lname, Salary FROM EMPLOYEE WHERE Ssn = ?";
	PreparedStatement p = conn.preparedStatement(stmt1);
	p.setString(1, name);
```
PreparedStatement prevents *SQL Injections*
- Operates **without the data**
	- Parses the query
	- Compiles the query plan
- The actual value of the data doesn't affect the query plan
	- DBMS just needs to know that there will be some data when it's executed 
	- DBMS already knows how query should be executed

Parameters can't be executed as code 
- Query plan has already been compiled 
- No need to remove keywords or escape characters 
	- And less complex, error prone 
- Therefore immune to SQL Injection attacks 
Query is parsed/compiled once then executed multiple times 
- For larger numbers of execution and more complex queries, this can be considerable performance advantage
#### **ResultSet Class**
- This is the return value from **execute** statements
	- e.g. `ResultSet r = p.executeQuery();`
- Essentially works liked a **linked list**, where the initial head points to *just before the first row* of the table
- Use `r.next()` to return the next element of the list
```java
while (r.next()){
	lname = r.getString(1); // parses varchar SQL variable into a Java string
	salary = r.getDouble(2);
	system.out.printline(lname+salary);
}
```

##### ResultsSets can have a few **extra properties** that may be useful

![[Pasted image 20231030130658.png]]

#### Type Terms
- **Scrollable**
	- Can move forwards **and backwards** (rather than just forwards)
	- ![[Pasted image 20231030131154.png]]
- **Positionable**
	- Can move **anywhere** (e.g. jump to row $n$) and back
- **Sensitive**
	- ResultSet can be updated to **reflect changes** made **after** the ResultSet was **created**
	- ![[Pasted image 20231030131105.png]]

