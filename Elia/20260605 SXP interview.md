Smart energy platform 
- process metering data from smart meter gateways
- goal is to prove compliance to legislation in DE
- 600k smart meters for now, grows 5% per month
- aggregate smart meter data 

Initially COTS application
End 2022 started on inhouse alternative
- transform original monolith into modules
- modules -> get taken over by 50Hz

What is the future these modules are going to run on?
Internally in 50Hz, not allowed to run containerized applications (in 2022)

EDP arrived as the solution

as stopgap, they spun up Openshift to run on until EDP was available

in 2024 

existing system will not scale beyond 1M gateways (based on Cassandra)
EDP is needed for them -> will move to Spark-ML

For spark, objectstore, tenant private network etc, they assume EDP will be operating this service for them

Requirements in OMA -> require clarification
 - reference architecture is good for insight in this

Production instance of EDP in 50Hz should be available on August 1st

Connectivity between 50Hz Mode 1 infra and EDP is needed (tested in dev/test env)
- security alignment between 50Hz and EDP is started now


#### SDLC
- use Azure devops
- EDP artifact registry and CI infra can connect to Azure container registry and they pull the artefacts to EDP, sign them and put them in EDP container registry
- in future, components that need to run on EDP will be moved to EDP CI etc

All existing applications now use Azure devops
- will all need the migration as proposed by SXP

#### Testing strategy
- perform unit, integration and system tests on azure devops
- then when helm charts and containers are available on EDP (pulled from Azure container reg) they get pulled and tested on EDP.

### Observability

- services running both on EDP and business infra
	- how the apps will stream their observation data to the platforms and the processes around it is TBD
- provided tools for observability are good, but require rework of their active dashboard
- they use grafana -> will work
- they also use other stuff -> wont work
- worries about the amount of data and when they can switch over to EDP

#### Load tests
- they did load tests on the dev/test environment

**Customer success team**
- helped them set up an intermediate S3 object store until it was available on EDP

**Missing services**
- IAMaaS -> Keycloak: moved to 2027. This is annoying as they need to setup IAM themselves
- Security signoff was already flagged early 2024

SLA: maintenance means some downtime 