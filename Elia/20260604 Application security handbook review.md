
Created: May 18 2026
Current version: May 19 2026
![[Pasted image 20260604164503.png]]


### Remarks
- Starts at version 1.0.0?
- Do they eat their own dogfood? E.g. Service-to-service communication
	- Service-to-service communication must:
		- Use authenticated identities
		- Use encrypted transport
		- Authorize access based on service identity and purpose
		- Log security-relevant requests
		- Avoid broad network reachability
### Strong
- Security lens approach gives concrete guidance to developers
- 

### To improve
- All requirements in this document are mandatory unless a formal exception is approved by the accountable security and platform owners. -> Nice, but document how and where to store the exception

### Evidence required
#### Containerized applications running on Kubernetes must, at minimum:
- Run as non-root unless an approved exception exists
- Disallow privilege escalation
- Drop unnecessary Linux capabilities
- Avoid privileged mode
- Avoid host namespace sharing
- Avoid host path mounts unless explicitly justified
- Use read-only root filesystems where the application permits it
- Use minimal, approved base images

#### Service-to-service communication
	- Service-to-service communication must:
		- Use authenticated identities
		- Use encrypted transport
		- Authorize access based on service identity and purpose
		- Log security-relevant requests
		- Avoid broad network reachability

#### Each workload must use its own runtime identity and must not inherit broad cluster or namespace permissions by convenience.

This includes:
- Dedicated service accounts where required
- Least-privilege RBAC
- Explicit binding review
- No wildcard permissions without approved exception
- Separation between application identity and operator identity

#### Health, Metrics, Debug, and Administrative Endpoints

Operational endpoints must not become unintended attack paths.
This includes:

- Health endpoints that expose only what is necessary
- Metrics endpoints that do not leak sensitive identifiers or configuration
- Debug endpoints disabled in production unless explicitly approved
- Administrative functions strongly authenticated, authorized, and monitored
#### Runtime network exposure must be deliberate. Public exposure, internal service exposure, and outbound connectivity must be limited to what the workload actually needs.

This includes:
- Explicit ingress scope
- Explicit internal service exposure
- Network policies or equivalent controls for east-west traffic
- Controlled outbound connectivity
- Separation of public, internal, and administrative paths

#### Runtime Observability and Containment

Teams must be able to detect:
- Repeated authorization denials
- Unusual request rates
- Unexpected outbound connections
- Privilege or deployment drift
- Restart loops and abnormal workload behavior
- Secret access or configuration misuse where visible

#### When Threat Modeling is Required

- A new internet-facing or partner-facing service is introduced
- A service processes sensitive, regulated, or business-critical data
- A new authentication, authorization, or administrative flow is introduced
- A new external integration or third-party dependency is added
- A major architectural change affects trust boundaries
- A platform change affects deployment, secrets, identity, or network exposure