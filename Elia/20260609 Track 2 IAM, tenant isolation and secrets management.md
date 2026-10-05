
### Identity management

Authentication -> IAM
Authorization governance -> PRM

Workforce identity -> Enterprise IDP = master (federation)
AuthZ is decentralized in platform, managed via PRM


**Static credentials still used -> want to move away from it**

Clear split between infra IAM (manage compute/storage/network) and Workforce IAM

**multi-tenant IdP (managing platform access for tenants)**
-> delegated tenant authorization = tenant self-service IAM

IAMaaS -> intended for application level AuthZ

**Infrastructure IAM (Redhat freeIPA)**
- not federated to Elia IDP
- also manages machine IDs

JIT provisioning of users -> subsets of group in Enterprise IT

Managing EDP identity enforces MFA via Yubikey Bio series
Infra IAM -> need to create a ticket. Handled by the BE and German teams + EDP team for dev/test environment
-> **all VPN access and infra IAM user management is done manually**
-> they want to integrate this into Keycloak -> postponed beyond GA

There are **audit logs** on platform side -> keycloak logs these in database, pushed to OTEL 

To login into keycloak:
- guest accounts disabled
- token from upstream idp needs to contain certain EDP role or group otherwise login not possible

#### token "sub" claims
- contains random keycloak id in sub claim
- to ensure compatibility with legacy tools, they include the username  in the sub claim next to the typical user claim
- 

### Tenant isolation
Tenant isolation
- RBAC based
- what else?

In keycloak all tenants are in 1 single realm, they use organizations to do multi-tenant identity 

**PRM addons**
- discover all the services and the associated roles
- 

### Secrets management
- Workload identities through service accounts
- EDP secrets manager -> centrally managed vault instance

Looking at workload identities -> SPIFFE/SPIRE on the roadmap, but after GA

Split between tenant secrets and platform secrets
- SECR (tenants)
- SECR internal -> platform secrets

Access based on groups -> group membership defines access to vault
IaC reference asked as proof


