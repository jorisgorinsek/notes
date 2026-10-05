
# Overall risks

# Tenant SDLC

- Tenants don't use EDP / codebuild to develop their software so most of the proposed controls are not in use (yet). What incentive is there for teams to stop using their current (Azure devops) CI and move everything over to EDP Codebuild?
	- e.g. MCCS status: https://confluence.bare.pandrosion.org/spaces/EDPG/pages/105316802/MCCS+Open+Topics
	- Archi assessment MCCS ziet er goed uit (security is een klein onderdeel er van) https://confluence.bare.pandrosion.org/spaces/EDPG/pages/326565895/MCCS+RefArch+Synthesis
	- 

# Platform teams SDLC
- INFRA team seems to do SDLC activities related to security: 
	- https://confluence.bare.pandrosion.org/spaces/INFRA/pages/363136513/04_Security+Team?src=contextnavpagetreemode
	- Design decisions are documented: https://confluence.bare.pandrosion.org/spaces/INFRA/pages/37585923/DDI+Design+-+contains+HLD+and+LLD?src=contextnavpagetreemode
	- but even here: vulnerability mgmt docs empty: https://confluence.bare.pandrosion.org/spaces/INFRA/pages/644023587/Vulnerability+Management
	- security assessment empty: https://confluence.bare.pandrosion.org/spaces/INFRA/pages/620823217/Security+Assessment+Report?src=contextnavpagetreemode
- IAM team should be good? (used to be infra team right?)

Threat modeling: 
- INFRA security impact analysis gevonden, lijkt 1 keer ad-hoc gebeurd te zijn https://confluence.bare.pandrosion.org/spaces/INFRA/pages/443617705/Simple+Impact+Analysis+for+vCenter+Sub-Certificate+Leak+on+Gitlab

**Security champions**
= https://confluence.bare.pandrosion.org/spaces/EDPG/pages/620232709/Security+Architects+Champions+Roundtable


**SBOMS**

pipeline-includes repo
- seems to be for ICE
	- only used by watch-tower-api and watch-tower-ui
- SBOMS for container images using trivy


**SAST**
pipeline-includes repo
- seems to be for ICE or IaaS
- for python this calls sonarqube
- for helm this does linting

**container images repo**
- D4A


Short term
- Assign a single accountable security owner for EDP with an explicit mandate to review/block releases, preferably someone assigned by or, from within the 50Hz, ETB or Elia Group security teams. Replace the "Friends and Family"-style sessions with a structured, tracked Q&A process.
- Move credentials/secrets out of source repositories into the existing Vault/Keycloak infrastructure, with rotation enforced.
- Define and enforce a minimum supported-version/patch baseline, starting with Kubernetes and any internet-reachable components.
- Make IdP/SSO integration mandatory across all services; remove standing admin/overly privileged accounts not strictly required.
- Mandate Kubernetes-level network policies as an interim compensating control ahead of Workspaces 2.1, rather than leaving this optional.
- Centralize audit logging and define a documented incident response process for EDP, with clear ownership.

- Produce minimum-viable architecture/configuration documentation for the highest-risk custom components, starting with the network security layer.