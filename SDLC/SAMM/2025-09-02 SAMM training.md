**TODO** 
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


### Verification
#### Architecture Assessment
Link to threat modeling
##### Architecture validation
- architecture reviewed for security mechanism (iam, least privilege, encryption)
level 1: review all major design changes on ad-hoc basis 
level 2: more stuctured reviews (vs external or internal requirements), systematic reviews, quality criteria for the strucured process
level 3: use of reference architectures

##### Architecture mitigation
- have all mitigations defined in threat model actually been implemented?
- 
level 1:  ad-hoc review of application architecture -> security savvy-staff is doing the review of the implemented system -> security champions
level 2: systematic review -> done by the application team. unhandled stuff logged as defects
level 3: review reference security architectures

#### Requirements driven testing
Building in testing at all level - preferrabily automated
##### Control verification
Do testers test the security requirements just like other requirements
tests exists, preferably automated
regression tests exist
level 1: verify implementation of authz, input, encoding etc on ad-hoc basis. when the application changes
level 2: automating the above,  use a standardized test framework (e.g. ASVS)
level 3: full automated regression tests
##### Misuse / abuse testing
**Here, the levels don't build on top of each other!!!**
- Specialized testing like fuzzing
- abuse test cases
- DoS testing

level 1: are you fuzzing?
level 2: perform abuse test cases?
level 3: perform load testing / DoS tests?

#### Security testing
Do you test the software according to security best practices?
Do you do something with the results?
##### Scalable baseline
automated security testing to detect low hanging fruit
- SAST and DAST in place
- customized to our env -> limit false positives
- integrated in CI/CD
level 1: are you using suitable tools? generate inputs = using those tools to do blackbox testing
level 2: tune tools to minimize false positives to avoid fatigue, also minimize false negatives
level 3: integrating this in CI. Need to track and merge this results and centralize them (defectdojo or dashboard)
##### Deep understanding
Do you do pentesting to go deeper than standard tools
- internal manual testing to ensure 
- regular pen testing
- feedback loop to improve complete application portfolio
level 1:  internal testing on high risk tests, track findings as bugs
level 2: external pen testing
level 3: use patterns in findings to make requirements and designs better. use them to create training and playbooks


### Operations
#### Incident management
- can you detect?
- can you respond?
- Do you have metrics and use those to improve?
##### Incident detection
- do you have good logs for all places and look at them?
- there is a process for incident handling - a responsible - trained - simulation
- review the detection process
##### Incident response
- is there a dedicated response team
- do you exercise your responses? do you use these to improve the process
- level 2: forensic tools = they have the tools to do proper incident response available
- level 3: dedicated resources for incident response (full-time) with all professional tools to do so


#### Environment management

##### Configuration hardening
level 1: manual hardening
level 2: more organization -> baselines have owners, they are documented, people are trained
level 3: automated checks (e.g. https certs), security dashboard is also documentation

-> level 3 is target here for DSH

##### Patching and updating
Higher maturity: teams watch for new updated and patch themselves (doesn't mean latest and greatest, but upstepping when needed)

level 1: know your components and dependencies, check ad-hoc for vulnerabilities
level 2: process to scan for patches and vulnerabilities + SLA for responding to vulnerabilities
level 3: pro-active monitoring and patching . Process to find components with missing updates. Regularly improve the process

#### Operational management
##### Data protection
- backups, integrity etc
- data catalog = what elements are stored and processed, you know where copies of the data are stored.
- knowing which data is what level of sensitivity, which legislation or rules are applicable etc
level 1:  protecting the data: know what data and which is the sensitivity. Ensure data does not leak from prod to dev or staging
level 2: do we have a centralized location where the data catalog is stored + you know which legislation applies. Controls for protecting data are in place (e.g. encryption)
level 3:

##### System decommissioning and legacy management

Contains 2 parallel paths: as consumer of SW and as producer of SW

Legacy management: 
**as the user of 3rd party software**
- identify and remove deprecated, unsupported software (libraries)
- you have a process to clean up (e.g. associated accounts, resources etc)
**as the producer of software**
- ensure customers migrate from unsupported versions

level 1: ad-hoc by occasional review
level 2: track lifetime of components through regular process
level 3: actively predict when components , active process to deal with this LCM, regular review