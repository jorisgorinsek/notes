
**Question 27 and 28**
Benjamin shares screen of integration stage gates for platform releases
-> integration phase -> have been doing this for 8 weeks
-> acceptance phase to be implemented
 -> will share links

**Question 1:** training for tenants
- No LMS, no tracking of training

Question 2: security of technologies
- evaluation for Keycloak was done ad-hoc
- evaluation of Gitlab was done?  
- should be documented in ADR -> Sven-thomas will provide links

**Question 3:** should be replaced by factory 3.0

**Question 4:**  HW and SW acquisition process
- is this process actually being implemented?
- Andreas answers: question for Reinard from Finance. Was installed by finance
- They claim to have served as reviewers for the PASOLF process
	- last 2 or 3 month there have been reviews of 28 softwares

**Question 5:** ADR reviews start?
- since beginning of 2026
- concept of security champions 

**Question 6:** audit logs
- still building it, not much present
- poc phase for SIEM

**Question 7:** data classification
- internal data is handled implicitly
- for tenants -> this is included in data governance pillar, draft we have not seen yet. Mansab will provide a link to the draft

**Question 9:** no roadmap beyond 2026 for security
- the CRS team was renamed to the ISRC team

**Question 10:** metrics tracking
- part of factory 3.0 - 6 to 8 month timeframe

**Question 11:** Andreas has a list of security controls needed from EU laws and ISO 27001
-> please share -> Roland will share
-> have been used to create NFRs, these apply to PRSes
-> they received the list from Kris and German CISO, they are mapping these to their current controls

**Question 12:** no tracking back from NFR to security controls -> this is part of factory 3.0

**Question 13**: are implementing Wazuh in GCP
-> operational model 
    -> to be operated by EDP ops team
	-> will be monitoring app dev/test, prod BE and DE
-> for operations in BE, DE the plan is to integrate with Splunk
	-> Wazuh should be seen as first line and event filter before events get forwarded to Splunk in the prod environments

**Question 14:** encryption at rest and in transit
- netapp doesn't do encryption at rest
- CEPH will add encryption 

**Question 15:** SDLC training
- Andreas says there is governance around this
- Factory specification 
- Staale explains release manifests and what they contain -> will share the manifest

**Question 16:** security champions
- security champions model is being changed at the moment
- security champions did not have any power for enforcement -> did not work
- list will be shared, along with role description

**Question 17:** threat models
- There is a threat model for DevSecOps -> Roland can share
- There are no threat models created in the PL teams
- Threat models are not a mandatory artifact now, will be enforced later on





 