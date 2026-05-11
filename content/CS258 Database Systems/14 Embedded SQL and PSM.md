Instead of using APIs like JDBC, some languages can **embed** SQL **directly**
#### Declarations

![[Pasted image 20231031091035.png]]
#### Getting Tuples

![[Pasted image 20231031091220.png]]

- INTO is fine for **one** tuple at a time
- When several tuples:
  - Need to store in an appropriate structure
  - Need to be able to access sand iterate through the tuples
  - So use special cursor variables

![[Pasted image 20231031091516.png]]


### Persistent Stored Modules (PSM)
#### The Two-Tier Architecture

![[Pasted image 20231031091737.png]]

- Client applications use **API** to access server databases via **standard interface** (*JDBS, Embeddd SQL for C*)
- Client program may be connect to **several** DBMSs
#### The Three-Tier Architecture

![[Pasted image 20231031092124.png]]

- Between the **client** and the **DB server**, there is some **DB application**
- Click browser, converts to **action**
- Sends **requests** to server, **converts** to **DB logic**
- **Updates DB**

**Separating** app **logic** from **DB**:
- **Security** purposes:
	- End user interacting with our application rather than directly with DB 
	- Server is **only** accessible from the **middle tier** 
		- **End users** **can't** directly **interact** with D
	- **Client** is just a web **browser** connected to a web **server**
#### PSMs
However, good reasons to **embed** app logic in **DB server**:
- Write the application **logic** in **functions/procedures** - **PSMs**

**Stored procedures/functions/triggers**
- **Logic** that is used by **multiple clients**
	- Keep **constraints** and **business rules** on the server
	- **Independent** of any client **application**
	- Don't need to keep programming rules in different clients
- Can be more **efficient**
	- Send parameters direct to DB server rather than calculating lots of different values and then sending
- Can use Procedures to express more **complex** data models than constraints

**Declaring** and **calling** stored procedures:

![[Pasted image 20231031093058.png]]

#### Extending SQL for PSMs
Since SQL is a declarative language, not a procedural one, need to have flavours of procedural code defined by the DBMS
- In Postgres PSM procedural code needs to be specified as '*PL/pgSQL*'
![[Pasted image 20231031093527.png]]
![[Pasted image 20231031093544.png]]

Full example:

![[Pasted image 20231031093556.png]]

#### Calling PSMs from JDBC
Use `CallableStatement`s instead of `PreparedStatement`s

![[Pasted image 20231031093813.png]]

#### Triggers
- Allows for more **complex checking/rejecting** of **new data**
- Can **update** DB when particular **actions** take place

![[Pasted image 20231031094002.png]]

- Logs the **time** at which an employee's salary is **updated**