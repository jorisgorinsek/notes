**TODO 
- Define the scope clearly!
- bring in some treats!
- look at the "what is good" slides for a given activity (always there before the levels)

**Recommendations**
- Some level of trust  is essential for a good assessment -> everyone in the same room. 
- Start small, then extend scope

**Presentation**
- Stress that it is not a threat, but an objective measurement
- Share vision with the full team during a presentation.

**Questionnaire**
- don't say "are you using this" -> suggest they should be doing this. Instead ask curious questions "how are you guys doing X". 
- is there a need for a certain activity, how do you guys see that?


**Defining scope**
- Applications (DSH)
- Teams (Green, Indigo)
- Assets ?
- Software components?
- Technology domains

What data is in scope? -> unclear
- Record types
- Databases
- etc

**Methodology**
- Make sure the result of the assessment is comparable to other assessments in the same org.
- Stakeholder identification => make sure the right people for the topic at hand are in the session!
- Not everyone needs to be in the session all the time!
- Not all questions will get full answers in a session -> follow-up or homework needed
- write down why we score how we score

## Stream 1: Governance
### Strategy and metrics
- create and promote
- measure and improve
	- what metrics do you have (from different perspectives)
	![[Pasted image 20250903144157.png]]

 Also **think about which level you are scoring** before assigning a score: **higher = more strict!**

## Stream 2: Design
### Security requirement
- level 1: we do use requirements (e.g. ASVS)
- level 2: structured approach, legislation and standards as input. Included reviewing the requirements by sec champions
### Supplier security
- This is not about OSS and supply chain security -> there is another SAMM activity about that -> secure build (SW dependencies)
- this is about managing the risks when buying software from this vendor
- **We should look at the guidelines here and see how we as a vendor should behave towards our customers**
### Security Architecture
#### Architecture Design
- resources and training for architects on secure by design
- common security libraries and common security functions 
#### Technology management
- What are you depending on, do you know everything that is needed to get something in production
- are all of these dependencies up to date and well maintained?

## Stream 3: Implementation
### Secure build
- Full automation
- Security checks
- Make sure there are no vulnerabilities in dependencies
#### Build process

#### Software dependencies
- level 3 expects a "nice list" of allowed dependencies that is checked


### Secure Deployment
#### Deployment process
- Deployment should be done by someone separate from the developer
#### Secrets management
- managing secrets requires separation of duties

### Defect Management
#### Defect tracking
#### Metrics
