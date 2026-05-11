We have seen that SQL supports 3 basic constraint types : 
- **Key** constraint: A primary key value is unique 
- **Entity Integrity** Constraint: A primary key value cannot be null 
- **Referential integrity** constraints : a “foreign key” must have a value that is already present as a primary key or may be null
Plus, **attribute** constraints
#### Specifying Attribute Constraints
Additional restrictions on attribute domains can be specified:
- `NULL` not permitted: `NOT NULL`
- **Default** value of an attribute
	- `DEFAULT <value>`
	- e.g. `AccountBalance REAL NOT NULL DEFAULT 0.0;`
- `CHECK` clause
	- `Age INT NOT NULL CHECK (Age > 0 AND Age < 125);`
	- Allows attribute constraints based on application semantics
#### Key Constraints
- `PRIMARY KEY` clause
	- Specifies one or more attributes that make up the primary key of a relation
	- `Dnumber INT PRIMARY KEY;`
- `UNIQUE` clause
	- Specifies alternate (secondary) keys
		- Candidate keys
	- `Dname VARCHAR(15) UNIQUE`
#### Referential Integrity Constraints
- `FOREIGN KEY` clause induces constraints which must be checked and handled during updates
	- Default operation: reject update on violation
	- Attach **referential triggered action** clause
	- Options include:
		- `SET NULL`
		- `CASCADE`
		- `SET DEFAULT`
	- Attach action to be taken upon violation varies on types of updates
		- e.g. `ON DELETE` or `ON UPDATE`
###### Example
``` SQL
CONSTRAINT SuperSSN-SSN 
FOREIGN KEY (Super_ssn) REFERENCES EMPLOYEE(SSN) 
ON DELETE SET NULL ON UPDATE CASCADE
```
- If a supervisor’s tuple is deleted from EMPLOYEE 
	- The tuples referencing (thru the FK Super_ssn) the deleted tuple will have its Super_ssn set to NULL
- If a supervisor’s tuple’s SSN in EMPLOYEE is updated 
	- The new SSN value will be cascaded to the value of Super_snn of all tuples referencing the updated EMPLOYEE tuple.
#### Specifying attribute constraints - `CHECK`
`CHECK` applies restrictions on the accepted values of an attribute - not only do they have to match the domain, they also have to have the expression evaluate to true
```sql
StudentID INT PRIMARY KEY,
StudentAge INT,
	CHECK (StudentID > 0 AND StudentAge > 0),
```
Constraints can also be named which allows for easier reference so that they can be altered/removed later
```sql
StudentID CONSTRAINT nonzero
	CHECK(StudentID > 0)
```
