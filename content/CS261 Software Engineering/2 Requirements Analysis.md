Requirements are descriptions of **what the system should do**, **the service it provides** and the **constraints on its operation**
- They provide a basis for **tests**, **validation** and **verification**

![[Pasted image 20240116111834.png]]

![[Pasted image 20240116112113.png]]

**User/Customer Requirements (C-Requirements)** 
- Define how the system should work from a user’s view 
- Described in natural language with diagrams 
- Lists constraints of operation from the user’s point of view 
**System/Developer Requirements (D-Requirements)** 
- Detailed descriptions of the systems functions, services and operation constraints
- Defines exactly what must be designed and implemented
	- Acts as a basis for the contract with the developer

### Writing Quality Requirements

![[Pasted image 20240116113021.png]]

#### Whole set of requirements need to be:
**Prioritized**
- Assign an implementation priority to each requirement, feature, or use case to indicate **how essential it is** to include it, **relative** to others, in a particular product release
**Consistent**
- Consistent requirements do **not conflict** with other software requirements or with higher level (system or business) requirements
**Modifiable**
- You must be able to **revise** the set of requirements when necessary and maintain a **history** of changes made to each individual requirement **separate** from others
**Traceable**
- You should be able to **link** each software requirement to its **source**, which could be a higher-level system requirement, a use case, or a voice-of-the-customer statement
- Also, **link** each software requirement to the **design elements**, **source code** and **test cases** that are constructed to implement and **verify the requirements**
#### Individual requirements need to be:
**Correct**
- Each requirement **accurately** describes the **functionality** to be delivered
- The reference for correctness is the source of the requirement, such as an actual customer or higher-level system requirements.
**Feasible**
- It must be **possible** to implement each requirement within the known **capabilities** and **limitations** of the system and its environment
**Necessary**
- Each requirement should document something the customers **really need** or something that is **required** for conformance to an external requirement, an external interface or standard
**Unambiguous**
- The reader of a requirement statement should be able to draw **only one interpretation** of it
- Also, multiple readers of a requirement should arrive at the **same interpretation**
**Verifiable**
- See whether you can devise **tests** or use other **verification approaches**, such as inspection or demonstration, to determine whether each requirement is **properly implemented** in the product
### MoSCoW Requirements
Helps **prioritise** requirements

![[Pasted image 20240116113614.png]]

**Traceable** requirements (making sure they're traced throughout the project):

![[Pasted image 20240116113727.png]]

## Requirements Analysis Document
Has a variety of **possible users** who must be able to **understand** it
- **Customers**
	- To ensure the requirements meet their needs
- **Managers**
	- To bid for and plan the system
- **Engineers**
	- To guide the implementation of the system
- **Testers**
	- To design test cases based on the requirements
- **Maintainers**
	- To understand the relationship between components once the system has been completed

**Sections**
- **Preface**
	- Details history of the document and who is expected to read it
- **Introduction**
	- Justifies the need for the system and outlines what it will do
- **Glossary**
	- Explains any technical terms used throughout the document
- **User Requirements Design**
	- Describe the services provided for the users
	- Written in natural language with diagrams
- **System Architecture**
	- Presents a high-level overview of the system, showing the distribution of functions across system modules
- **System Requirements Specification**
	- Describes functional and non-functional requirements
- **System Models** (Can be Planning & Design doc)
	- Shows relationships between the system components, usually through diagrams (object models, data-flow diagrams etc.)
- **System Evolution**
	- Describes assumptions on which the system is based and anticipated changes due to changing user needs, hardware evolution
- **Appendices**
	- Provide detailed, specific information that is related to the application being developed (database schemas, hardware requirements etc.)

The **process** of writing down the user and system requirements in a formal document:
- **Unambiguous**
- **Easy to understand**
- **Complete**
- **Consistent**

**Functional Requirements**
- Describe what the system should **do**
- Statements of **services** the system should **provide** 
- Detail how the system should **read** in certain scenarios
**Non-functional Requirements**
- Often termed ‘qualities’ - **availability**, **performance**, **deployment** 
- **Constraints** on the services or functions offered by the system 
- Legislative constraints on the system (any applicable laws) 
- Often apply to the system as a **whole** rather than individual components

### Requirements Elicitation and Analysis
**Requirements Discovery**
- This stage gathers information about the project and converts into requirements 
	- Interviews with stakeholders, domain research, watching people use existing systems, use cases 
- Need to deal with: conflicting requirements, imprecise information from stakeholders, legal/organisational factors
**Requirements Classification and Organisation**
- Categorises the discovered requirements to group similar ones, or ones on similar themes
**Requirements Prioritisation and Negotiation**
- Prioritise the requirements and negotiate between conflicting requirements from different stakeholders
**Requirements Specification**
- Place the requirements in a document for stakeholders to read, then begin the process again as new requirements are discovered and old ones changed
### Requirements Validation
Once requirements have been document, they must be approved and shown to be acceptable
- **Validity**
	- Will the system support the customer’s needs?
- **Consistency**
	- Are there any conflicts?
- **Realism**
	- Can the system be produced with the available budget and technologies?
- **Verifiability**
	- Once complete, can the system be shown to satisfy the requirements?
### Requirements Management
As new requirements come in, they must be analyzed to see if they should be accepted
- **Problem analysis**
	- Checking the new requirement is valid and unambiguous
- **Change analysis**
	- Determine the effect on the rest of the system and estimate a cost of performing this change
- **Change implementation**
	- If valid and acceptable, change the requirements document and implement the requirement into the system

![[Pasted image 20240116115409.png]]

