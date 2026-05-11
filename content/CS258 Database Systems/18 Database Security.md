#### Threats to DBs
- **Confidentiality**
	- Protect against unauthorised **disclosure**
- **Integrity**
	- Protect against unauthorised **changes** or **data corruption**
- **Availability**
	- Protect right to data **access** (stop 'denial of service')
#### Protection Mechanisms
- **Access Control**
	- **Who** and **How** 
		- What can individuals **do** with their access
- **Inference Control**
	- Provide access to **statistics** about the data but **not actual** data
- **Flow Control**
	- Prevent data being **read** from a **protected** area
	- And then **written** into an **unprotected** area
- **Encryption**
	- **Encrypt** in DB and only allow certain entities the key
## Database Security Mechanisms
DBMS has Database **Security** and **Authorisation** Subsystem
**Two** types of mechanisms:
- **Discretionary**
	- Grant/Revoke Access Privileges
- **Mandatory**
	- Enforce classification levels for users and data items
### DB Administrator
In charge of DB management including security
- **Account creation**
- **Privilege granting**
	- *Discretionary*
- **Privilege Revocation**
	- *Discretionary*
- **Security Level Assignment**
	- *Mandatory*
#### Security Reviews
**System Log**
- Logs entry for each operation applied to the DB
- Used to **recover** from transaction failure or system crash
- Used for security purposes as an **audit trail**
**Database Audit**
- Performed after **suspicious activities** detected
- **Review** the log
	- Consider all accesses and operations applied during a certain period
### Discretionary Access Control
Can use the query language to **grant** and **revoke** privileges
Types of privileges:
- **Account level**
	- Privileges the account has (commands they can run)
- **Relation level**
	- Access to each individual relation or view
	- e.g. some accounts might have access to `SELECT`
#### Relation Level (and views level)
Follows the **access matrix model**
- Rows (**subjects**)
- Columns (**objects**)
- $M(i,j)$ represents the types of privileges that subject $i$ holds  on object $j$

![[Pasted image 20231128094328.png]]

#### Privilege Control
Each **relation** is assigned an **owner** account
- Usually the account that **created** the relation
- Owner of a relation is given all privileges on that relation

![[Pasted image 20231128094426.png]]
#### Granting SQL privileges

![[Pasted image 20231128094504.png]]
#### Views for DAC

![[Pasted image 20231128094531.png]]
#### Revocation and Propagation of privileges

![[Pasted image 20231128095259.png]]

![[Pasted image 20231128095309.png]]![[Pasted image 20231128095316.png]]
#### Propagation Limits

![[Pasted image 20231128095332.png]]

![[Pasted image 20231128095336.png]]
### Mandatory Access Control
DAS is all-or-nothing
- More fine-grained classifications are often required

![[Pasted image 20231130161155.png]]

MAC can be combined with DAC

Follows the **Bell-LaPadula** Model
- Apply the **security level** (TS, S, C, U) to each
	- **Subject** (user, account, program) 
		- Class(S) - (also called '*clearance*' of S)
	- **Object** (relation, tuple, column, view, operation) 
		- Class(O)
- These restrictions also enforced:
	- **Simple Security** - $S$ is not allowed to **read** $O$ unless:
		- class(S) $\geq$ class(O)
		- "*No read up*"
	- **Confinement** (sometimes called *star* property) - $S$ is not allowed to **write** $O$ unless:
		- class(O) $\geq$ class(S)
		- "*No write down*"

![[Pasted image 20231130161741.png]]

