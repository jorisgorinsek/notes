Tobias 50Hz ISRC team lead
Roland ISRC team

-> See org chart in presentation

### org chart
Risk & compliance specialists -> work with product lines -> security champions?

Security operations -> being built up
- Roland and Theodore
- hiring engineers to setup process 

**Interaction with CISO team**
- CISO at group level Kris
- acting CISO (ISO for 50Hz) Martin
- starting a new process of weekly lineups

CISO team has soc and SIEM capabilities

**Policies**
- translated into non-functional requirements to the platform
- these are only there for the last month
- they have maturity level basic and something else

**SIEM**
- Wazuh only for dev/test
- there is something about Splunk being used

**Security champions**
- each product line has a security champion and product architect
- Frank - operation -> not clear what the list of security champions is
- there is a security board that gathers with Roland - bi-weekly or at hoc (not very convincing)

**Threat modeling**
- NFR that it needs to happen
- no guidance on how it needs to happen
- no follow-up on what happens with the findings
- training? -> no training but lightweight process 


**Factory model feedback loop** -> see confluence


**Vulnerability management**
- vuln detection via trivy
- vuln mgmt is there not yet
- standardized scoring mechanism
	- dependencies between components taken into account
	- they don't know what tenants will run on the platform - so no app risk levels

**Different risk registers**
- security risk register
	- confluence page
	- originally there was a Jira based risk register
- program risk register
	- goal is to have it in Jira, not there yet
	- Andreas owns all risks and the program board decides about all risks


**Data protection**
- data service product line -> provides features on things like data labeling, lineage, etc
- 

**Compliance**
- NIS2 compliance is needed 
- ENTSO-E is needed
- Elia / 50Hz should see EDP as a third party supplier
	- should provide security requirements and demand compliance proof of them

**Security culture**
- did there own mapping from NFR of security teams to BSI technical controls etc
- strong culture on their 

**Security requirements**
- are included as NFR in the product release specification
- are taken along in the ADR, although it is not entirely clear how
- are reviewed in step 3 (but they lack the manpower so it is not done in practice -> will hire 2 new persons to help)
- evolve as the product evolves
- they will provide link to the security requirements in Jira


-- 
AI summary
