# Software Process models
Definition - a sequence of activities that leads to the production of software product
Many different processes, but **all involve**:
- **Software Specification**
	- What the software should **do**
- **Software design and implementation**
	- **How** it should be **organised** and **implemented**
- **Software validation**
	- **Checking** it does what the customer **asked**
- **Software evolution**
	- **Changing** the software **over time**
Two main process model categories:
## Plan-driven
- All of the process activities **planned** in advance
- Progress **measured** against the initial plan 
- (Nearly) fixed specification **before** development commences
### The Waterfall model
- Plan-driven 
- Strict linear ordering of processes 
- Each stage must be completed before moving onto the next 
- Allows distributed development - system broken down to components

![[Pasted image 20240110122209.png]]

![[Pasted image 20240110122221.png]]

#### When does it work?
- Requirements are understood and will not change during development 
- Few team constraints (size, location) as development can be distributed in discrete chunks 
- Each component can be independently tested against its own specification before integration (outsourcing) 
- Easy to add members to the team because the whole system is well documented (churn)
#### When does it not work?
- Customer can wait a long time before they see results 
- Using this model, it is very difficult to accommodate change once the process is underway 
- Difficult to respond to changing customer requirements 
- Could be a major problem if the project is long running 
	- Business priorities change 
	- Customer expectations change 
	- Underlying technologies change
### Incremental Software development
Software can be developed in **stages** with customer feedback incorporated between iterations
- New functionality can be added in each iteration
- Each stage is planned in full and validated against the plan before the iteration is considered complete

![[Pasted image 20240110123453.png]]

#### Good:
- Cost of accommodating changing customer requirements is reduced 
- Software available to user quicker, and therefore feedback can be easily solicited 
- Greater perceived value for money - customers can see development progress 
- Include the user in acceptance testing at each phase.
#### Bad:
- Difficult to estimate the cost of development 
- Difficult to maintain consistency with new features being added - poor design choices at the beginning may hinder later development 
- As progress continues, it becomes harder to include new features or make changes to fundamental components 
- Not cost-effective to produce documentation for every version of the software Increased cost of repeated deployment
### Reuse-oriented Software Engineering
- Rewriting software from scratch is unnecessary and expensive 
- Many developments rely on a large base of reusable components 
	- Common off the shelf (COTS) systems 
- Typical applications include web apps, frameworks (.NET) 
- Compromises may have to be made, but development can be rapid and less costly 
	- Companies often develop their own libraries (e.g. for I/O)

![[Pasted image 20240110124235.png]]

- **Component Analysis** 
	- Searching for available components to meet the specification. 
- **Requirements modification** 
	- Analyse the found components to determine if they meet the requirements, modifying requirements if acceptable to do so 
- **System design with reuse** 
	- Design the system using the components. 
- **Development and integration** 
	- Integrate the selected components into a complete system

## Agile
- **Incremental** planning 
- More **adaptable** to change during development

Agile is a principle that defines a **set** of methodologies
- **Rapid** software development
	- Specification, design and implementation are **interleaved**
	- System is developed as a **series** of **versions** with stakeholders providing feedback at each stage

Principles:
- **Customer Involvement**
	- The role of the customer is to provide and prioritise new system requirements and evaluate iterations of the system
- **Incremental Delivery**
	- The software is developed in increments with the customer specifying the requirements for the next iteration
- **People, not process**
	- The skills of the development team should be recognised
	- Team members should be lest to develop their own ways of working
- **Embrace change**
	- Design a system that accommodates requirement change
- **Maintain Simplicity**
	- Focus on simplicity in the software being developed and the development process

Two Agile methods:
### Extreme Programming (XP)
Incremental delivery with **fast iterations**
- Versions built several times a day 
- Delivered to customers every 2 weeks 
- Automated tests to verify builds 
	- Build only accepted if all tests pass 
- Code continually refactored to maintain simplicity 
- Strong customer involvement

![[Pasted image 20240111143116.png]]

- **Incremental Planning** 
	- Requirements recorded on story cards, stories to be included in the next release are selected based on their priority 
- **Small Releases** 
	- Initially, minimum set of functionality developed 
	- Following releases add new functionality 
- **Simple Design** 
	- Only enough design performed to meet current requirements 
- **Test-driven Development** 
	- Test are written before the software, software then evaluated using them
- **Refactoring** 
	- Developers are expected to continually refactor and improve code 
- **Pair Programming** 
	- Developers work in pairs, checking each others work and providing support - one codes, the other watches 
- **Collective Ownership** 
	- At least 2 developers have responsibility for any part of the code, any developer can work on any part of the system if they want to make a change. 
- **Continuous Integration** 
	- Components integrated as soon as they are ready
- **Sustainable Pace** 
	- Large amounts of overtime is avoided, programming occurs at a sensible pace with support to complete the job well 
- **On-site Customer** 
	- Customer is always available to work closely with the team 
	- No waiting for response to emails holding up development
### Scrum
General agile method that focuses on managing iterative development rather than specific agile practices

3 primary stages:
- **Outline planning phase** 
	- Establish general goals 
- **Sprint cycles**
	- Each cycle develops an increment of the system 
- **Project closure** 
	- Wrap up the project, documents and delivers
#### Sprints
- Quick development cycles 
	- Typically 2-4 weeks 
	- Daily team meetings to discuss current work 
	- Each sprint completes item on the backlog (remaining task on project) 
	- Features selected with the customer, than isolated from the customer for the remainder of the cycle 
	- Scrum master interfaces between team and customer 
	- At the end of each sprint, work is reviewed and presented

![[Pasted image 20240111144939.png]]

### Prototypes vs Minimum Viable Product
- An agile approach can produce MVPs or Prototypes
- For the project, you should focus on prototypes

![[Pasted image 20240111145031.png]]