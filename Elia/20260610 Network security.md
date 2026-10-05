
**Workspaces**
 -> similar to VPC in GCP
 -> no segmentation between VPC
 
Working on Workspaces 2.1 -> to be released in 2 months
-> isolation between VPCs
-> peering concept
  - initial peering for GA, needs to be expanded into more peering products

Infra team works on Networking / Compute / storage / DNS / PKI
PKI will move to IAM team

Infra exposes their products to ESL
 -> ESL handles tenant identities and authN / authZ

Opinionated platform
- only features the teams need
- REST API, fully API driven

3 layers -> see overview drawing on confluence
- product layer
- provisioning layer
- platform layer

Consists of bunch of microservices all written in python

 **Workspaces 2.0**
VRF is assumed as a safe environment, within a workspace there are intentionally no security controls on the network layer. These controls live on the boundaries of the workspace, e.g. through peering etc (2.1)
-> not implementing network policies (conscious decision)
-> only k8s clusters are deployed right now (may change in the future, e.g. individual VMs) -> network policies can be deployed at k8s level


**Workspaces 2.1**
-> no more vyOS!

Connector concept
- both conceptual and real thing
- from customer perspective, these can be deployed based on needs (default connectors are always there - e.g. for DNS resolution and other access to internal services)
- connectors are exposed as peering services
- connector is Linux virtual machine
	- immutable ubuntu distro, hardened
	- sudo users cannot change anything
- working on immutable distro for customer VMs
- ESS is the hypervisor platform -> going to move to linux + kvm
	- remove VTAPs from switches etc -> will allow to scale better (not limited by switches)
- Rules on peering (through policies) on what workspace allow which peering services
	- e.g. external workspaces (P-DMZ or A-DMZ) do not allow peering that would expose internal network or peering that would not allow external access
- Connectors are deployed in pairs for HA (rolling connectors when upgrades are needed)
	- connector pairs per availability zone
- **Default services**
	- Internet egress is now based on an allowlist
	- storage on netapp platform 
		- -> limited to 1024 SVMs (because of this they provide shared block / object / file)\
		- -> limited in terms of security controls
		- will move to Ceph -> performance, scalability (starting with object store)
			- will provide proper security controls
	- Client access 1.5
		- remote access is to be provided by the operators
		- for EDP developers -> VPN alike solution for dev access
		- programmable service for setting up VPN connections
			- User identity can be bound to a VPN group and each VPN group is bound to a workspace
			- identities: client certificate signed by internal PKI, password for local IdP
			- OSCP is implemented for certificate revocation
			- client cert is needed to lock out external attackers from reaching local IdP and internal services
			- Ops teams handles onboarding persons in the IdP (and JML) -> process documentation


**Access to the infra**
- VPN access is only way to access, except for VDI for some specific use cases
- VDI access for 
	- devs for password changes (locked down VM)
	- infra management for the infra admins
		- from VDI to jump hosts, login in with their shadow accounts
		- internal IdP = redhat IPA (hosts do LDAP)


**Internet ingress**
- only HTTPS traffic is allowed
- P-DMZ will have an ESL kubernetes cluster deployed
	- application security team is responsible to deploy a reverse proxy (e.g. WAF) to make sure routing to a-DMZ to work -> security team does not exist yet. All processes and procedures for daily operations 
- A-DMZ will have a tenant kubernetes cluster deployed
Example for IAM
- read-only 


**Ask about connectors, built by infra team**
- Secure signed images
- immutable images

**Ask about open source alternatives**
- why not use Openstack?


--
# Meeting Notes: EDP Due Diligence & Architecture Review

## Meeting Overview

- **Project/Context:** EDP Platform Due Diligence Project
- **Focus:** Security governance, tenant segmentation, and infrastructure workload isolation
- **Duration:** 2 Hours (scheduled)
    

## Key Discussion Topics

### 1. Workspaces & Tenant Segmentation Evolution

The team discussed the structural shift from the legacy workspace implementation to the upcoming highly isolated model. Workspaces act similarly to a VPC on GCP or a VNet on Azure, stretching as a single VRF across chosen Availability Zones (AZs).

- **Workspaces 2.0 (Current State):**
    
    - Implemented roughly three years ago to spin up the platform quickly.
    - Segmentation between different workspaces is currently open with no security controls between them.
    - Relies on a single connector via a shared network to exchange routing tables.
        
- **Workspaces 2.1 (Upcoming Release - ~2 Months):**
    
    - Introduces **Zero Trust by default** with full tenant workload segmentation.
    - Workspaces will be completely isolated upon deployment.
    - Requires explicit, **non-transitive peering services** to establish communication between specific workspaces (e.g., if A peers with B, and B peers with C, A and C cannot communicate transitively).
    - Moves from a single connector to redundant, high-availability controller pairs to allow seamless updates without traffic disruption.
        

### 2. Connectors and Operating System Security

- **Technical Makeup:** Connectors are network-optimized Linux virtual machines (currently Ubuntu) acting as virtual routers running FRRouting (FRR), OpenTelemetry, and troubleshooting tools.
- **Immutability:** Version 2.1 introduces an **immutable operating system**. Even engineers with pseudo-access cannot modify critical core directories (like `/etc`), mitigating the risk of privilege escalation or compromised states.
- **Granular Security:** Stateful firewalls will be deployed on these connector boxes to manage customer firewall policies cleanly without needing to redeploy the underlying immutable OS image.

### 3. Infrastructure Abstract Layer Policy

- **REST API First:** Infrastructure features are exposed to the East-West/Northbound layers purely via Python-based microservices and REST APIs.
- **Strict Abstraction:** The platform purposefully conceals underlying infrastructure hardware, vendors, rack locations, and tenant identities from customers and the ESL team. This strict boundary prevents security exposure risks and structural over-dependencies.

### 4. Storage & Technology Roadmap

- **Ceph Transition:** The current NetApp storage platform is heavily limited to 1,024 Storage Virtual Machines (SVMs), forcing the use of shared SVMs which limits cloud-native security controls.
- **Timeline:** The team is transitioning to **Ceph** to serve as the exclusive storage platform (starting with object storage, followed by file and block). Ceph will enter non-production environments this year for integration development and roll out fully next year to enable secure, identity-based credential locking.
- **Workspaces 3.0 Horizon:** Long-term architectural goals involve moving away from ESX hypervisors to **Linux and KVM**. This will shift VTEP functionality directly into the hypervisors, creating a dumb, high-speed IPv4 switching fabric that vastly scales VRF capacity.

### 5. Remote Access (Client Access 1.5)

- **Client Access 1.5:** An organically grown, programmable VPN capability that replaces older manual configurations.
- **Mechanics:** It binds user identities to specific VPN groups, which are then tied to individual workspaces. Users are authenticated via an internal IDP using a combination of internal PKI-signed client certificates and shadow username/password credentials.

## Action Items & Next Steps

- **Confluence Access:** External partners currently only have access to Track 1 documentation. Attendees need to follow up with **Jennifer** to gain access to the Track 2 documentation containing this workspace presentation and infrastructure security links.
- **Q&A Session:** A follow-up Q&A round is scheduled for next week to address deeper technical inquiries stemming from the Track 2 documentation review.

