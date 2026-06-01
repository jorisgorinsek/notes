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












