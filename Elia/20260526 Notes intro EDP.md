Setting up IAM

CORP or BELGRID IDs

Onboarding

- create project
- add developers
- add operator -> can create k8s resources

vcluster alike model -> tenants get k8s access
creating repos via developer portal
artifactory registry for fetching vendored deps (original from internet)

Image push to container registry needs to pass checks / rules on number of CVEs and severity etc

There should be SAST in CI

Integration between k8s for tenants and EDP secret manager -> no secrets in code

Codebuild runtime application security -> not explained further

DNS and TLS integrated in k8s cluster
-> dynamically added for tenant services
-> SME = service management engine
  -> for self-service provisioning (e.g. postgres or mongo DB), tenant does not need to case about provisioning etc -> just gets service string and creds

EDP roadmap exists and is updated quarterly, we don't have access to it yet

Question: git, analysis of code, analysis of PR etc is out of scope for EDP? Does EDP also provide a git repo platform, i.e. do we need to develop in EDP git?
A: full SW dev cycle of an application should happen on EDP

Upstepping platform services on EDP
 -> manual (because tenant services depend on it)


Cloud native application goverance for EDP
-  **reference architecture** and app -> zoeken
- operating model
- **application security handbook** -> 10 april nog niet beschikbaar, waar is het nu?
- Data governance paper -> idem
- 


Secure coding guidelines for Java, .NET and Python


EGAF = Elia Group Application Framework
OMA = Operating Model for Applications = Governance framework -> **zie link naar OMA overview op slide 40 (no access yet)**
-> governance for SDLC, how your app moves from idea to production on EDP

Different environments
- App dev / test
- app prod BE
- app prod DE

**Operating model for applications** describes the roles in the road to production in detail
-> OMA compliance portal zou moeten komen om tenants zelf compliance te laten checken

**15 stages to production**
01 - Ideation, Planning & Design  
02 - Code, Scan, Test & Build  
03 - Generate & Sign Artifacts  
04 - Service Deployment  
05 - Integration Testing  
06 - Performance Testing  
07 - Prepare Release Candidate  
08 - Handover Release Candidate  
09 - Exercise Change Management  
10 - Promote Application Artifacts  
11 - Deploy Release Candidate  
12 - Ensure Production Readiness  
13 - Prepare Release  
14 - Deploy Release  
15 - Application Operations


**DXP = development portal**






