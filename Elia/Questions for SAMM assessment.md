
- Do we set baselines where we think they need to be (CRA ++)
- Do we evaluate SDLC for tenant or for EDP developers themselves
	- assumption is the latter
	- tenant SDLC will be pretty important for the audit too
- How will we do recordings of the meetings?
- Will we use SAMMY? Can we use SAMMY?


### **From reference architecture**: 

is this the case for EDP as a platform itself? (no)

| [10 Dev/Prod parity](https://12factor.net/dev-prod-parity) | Keep development, staging, and production as similar as possible | - How do your app environments (eg: dev, test, staging, production) differ?  <br>- Do you encounter issues in one environment and not the other? |
| ---------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
#### Respond to Vulnerabilities (RV)

| Practice                                                            | EDP                                                                                                                                                                                               | Reference Application                                                                                                                                                                                                                  |
| ------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Identify and Confirm Vulnerabilities on an Ongoing Basis (RV.1)** | Trivy provides scan results during the regular build process, checking for known vulnerabilities against up-to-date databases. Adding these security scans is mandatory to avoid potential risks. | The application is based on Java as the programming language, and Trivy is used to scan for known vulnerabilities based on the **[GitHub Advisory Database](https://aquasecurity.github.io/trivy/v0.52/docs/scanner/vulnerability/)**. |
Evidence please?

#### Principle of Least Privilege
Are developers supposed to be able to access everything on all clusters? What restrictions are in place? Are they all cluster admin? How is authZ determined?
Evidence please -> cfr kubernetes cluster access

#### Access control
Reference arch mentions RBAC, but also ABAC and Policy based access control. What service / functionality / tech does EDP offer for that?

#### Signed commits
[SSH keys](https://docs.gitlab.com/ee/user/ssh.html) should be used to communicate with the code repository, and [Signed Commits](https://docs.gitlab.com/ee/user/project/repository/signed_commits/) should be set up to cryptographically verify your identity.

-> gebruiken ze dat ook echt?

#### Develop locally
**Security** - Working locally reduces the risk of exposing sensitive data or code to external threats compared to working on remote servers or cloud-based development platforms. Developers have more control over access permissions and can implement additional security measures as required.
**-> really?**

## from Application security handbook

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


