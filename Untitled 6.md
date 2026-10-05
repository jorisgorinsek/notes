# Complete Weakness Inventory — KPN DSH Tenant REST API (as Threat Statements)

Below is the full set of weaknesses from the threat model (V1–V23), each rewritten as a structured threat statement (actor → capability → action → weakness → technical result → business impact). Statements are grouped by trust boundary, with the risk rating from the document [1].

## TR1 — Tenant admin ↔ Reverse proxy (public edge)

- **V1 (Low):** A threat actor who can flood the reverse proxy (Traefik) with requests can cause a denial of service, making the Tenant REST API unreachable and blocking all *new* MQTT connections (only existing MQTT connections survive), resulting in reduced availability of all platform business logic — e.g., KPN customers can no longer change channels on their set-top box [1].
- **V2 (Low):** A threat actor who can enumerate subdomains routed by Traefik can map the platform's API attack surface (api.\<id\>.kpn-dsh.com, auth.\<id\>.cp.kpn-dsh.com), leading to information disclosure that enables targeted attacks against platform endpoints [1].
- **V3 (Low):** A threat actor who can probe the reverse proxy can extract information useful for further attacks (implementation details, error output, routing metadata), leading to information disclosure that supports reconnaissance for more advanced attacks [1].
- **V6 (Medium):** A threat actor who compromises one of the CAA-allowed certificate authorities can issue a valid TLS certificate for the platform domains and impersonate the genuine reverse proxy to tenant admins, leading to interception of authentication data and reduced confidentiality/integrity of tenant admin traffic [1].
- **V7 (Medium):** A threat actor who can spoof DNS responses (DNSSEC cannot be enforced on Azure) can redirect tenant admins to a fake reverse proxy or fake IAM, leading to man-in-the-middle interception of credentials and reduced integrity/confidentiality of the authentication chain [1].

## TR3 — Reverse proxy ↔ Tenant REST API (in-cluster)

- **V4 (Medium):** A threat actor with access to network traffic between the reverse proxy and the Tenant REST API can read, manipulate, or block requests because the connection is unencrypted (no TLS after the proxy), leading to exposure or tampering of tenant API requests and the JWTs passed for authorization, resulting in reduced confidentiality and integrity of tenant operations [1].
- **V8 (Medium):** A threat actor (tenant) who can impersonate the ctest user can abuse its elevated rights to reach system services, leading to unauthorized access to platform-internal components. Mitigating fact stated in the document: the ctest user is not enabled on AWS production [1].
- **V9 (High):** A threat actor controlling a compromised tenant namespace who exploits a Calico misconfiguration — or who forges an identity once past Kong, since no user identity re-verification exists in any component beyond the reverse proxy — can access or modify other tenants' data and system services, leading to cross-tenant data leakage, i.e., the doomsday scenario of confidential data leaks and reputational damage for KPN and Klarrio [1].

## TR4 — Reverse proxy ↔ Keycloak

- **V10 (Low):** A threat actor who can tamper with the Kong configuration can point token validation at an attacker-controlled Keycloak that does not perform JWT signature checking, leading to authentication bypass and reduced integrity of the entire authorization chain [1].
- **V11 (Low):** A threat actor who uses stolen or forged credentials can act without a durable forensic trail, because token issuance is logged only in regular Loki logs that rotate after 30 days and never reach the audit logs, leading to repudiation and reduced accountability [1].
- **V12 (Medium):** A threat actor who can attempt password/client-secret guessing against Keycloak can compromise tenant admin accounts, because no brute-force detection exists (and no detection of anti-brute-force mechanisms kicking in), leading to account takeover and reduced confidentiality/integrity of tenant and platform data [1].
- **V13 (Low):** A threat actor who can generate sustained load on Keycloak can exhaust the IAM service, because no load tests have validated its capacity, leading to reduced availability of authentication for all five platforms that share the Keycloak infrastructure [1].
- **V14 (Low):** A threat actor who spams Kong with requests can trigger the Keycloak-side load balancer to block the whole platform, leading to unavailability of the Tenant REST API as a denial-of-service side effect [1].

## TR5 — Tenant admin ↔ IAM (OAuth client credentials flow)

- **V5 (Medium):** A threat actor who steals the tenant admin client secret — which any tenant user can retrieve, since any tenant user can obtain the service account secret — can impersonate the tenant admin in the OAuth client credentials flow and receive valid JWTs, leading to unauthorized tenant administration; the shared retrievability additionally creates a repudiation problem, because actions cannot be attributed to an individual [1].
- **V15 (Low):** A threat actor who tricks a tenant admin into using a URL for a malicious IAM (the genuine IAM URL is published in DSH documentation, enabling look-alikes) can harvest client credentials, leading to compromise of the tenant admin account and reduced confidentiality of tenant data [1].

## TR6 — Tenant REST API ↔ Secrets

- **V16 (Medium):** A threat actor who social-engineers an SRE — directly or via Freshdesk tickets — can have access rights changed in their favor, leading to privilege escalation and reduced confidentiality/integrity/availability of platform and tenant data [1].
- **V17 (Medium):** A threat actor (tenant or outsider) who opens Freshdesk tickets requesting access-right changes can exploit the support process to obtain elevated privileges, leading to unauthorized access to platform resources [1].
- **V18 (Low):** A threat actor who can man-in-the-middle the secrets flow — the secrets component is reached without TLS and performs no JWT signature verification — or who alters a tenant name can cause another tenant's secrets to become their own, leading to theft of other tenants' credentials and reduced confidentiality [1].
- **V19 (Low):** A threat actor who can write to secrets that are consumed as configuration can inject code that is executed on behalf of another tenant, leading to reduced integrity and confidentiality of other tenants' workloads [1].
- **V20 (Medium):** A threat actor with access to the secrets component can read or write secrets without being detected, because the secrets component has no audit logging, leading to undetected credential theft and no accountability [1].
- **V21 (Low):** A threat actor who can generate load against the secrets component (which is the Kubernetes API) can degrade platform secret operations, because no load tests have been performed, leading to reduced availability of the platform [1].

## TR7 — Tenant REST API ↔ Kafka

- **V22 (High):** A tenant acting as an insider threat who can pull down the shared Kafka data plane — rate limiting is per-partition and can be bypassed by sending to multiple partitions — can make the Tenant REST API unresponsive, leading to reduced availability of all platform business logic, including KPN customer services (set-top box channel changes); the same shared data plane also allows a tenant to deny service specifically to other tenants [1].
- **V23 (Medium):** A threat actor who repeatedly performs wrong Keycloak login attempts against a victim can lock out legitimate users, including admin users, leading to reduced availability of tenant administration and authentication, since the lockout behavior is not enabled on all realms [1].

## Additional weaknesses without a V-number (from mitigations/remarks)

- **Audit log exposure:** A threat actor who is a DSH developer (or gains Grafana read access) can read the audit logs stored on S3, because all DSH developers have access, leading to reduced confidentiality of audit data; retention is also undefined ("TBC, 3 years?") [1].
- **Plaintext secret submission:** A threat actor observing the secret-creation flow can read new client secrets in plaintext, because plaintext is submitted to the Secret Store, leading to credential exposure [1].
- **Algorithm confusion (pending verification):** A threat actor may be able to forge JWTs if the Keycloak configuration is vulnerable to algorithm confusion attacks — a check still on the backlog (R17/DOK-3271) — leading to authentication bypass [1].
- **Keycloak blast radius:** A threat actor who compromises the Keycloak instance gains authentication control for five platforms, because Keycloak is shared infrastructure, leading to reduced confidentiality/integrity/availability across all five platforms [1].

## Doomsday scenario correlation (helps the end-user see impact)

| Doomsday scenario | Threats that can realize it |
|---|---|
| Cross-tenant confidential data leak → reputation damage | V9, V18, V19, V4, V5 [1] |
| Third party extracts sensitive data (e.g., call data) → Dutch authority fines | V9, V5, V12, V4, V6, V7, V15 [1] |
| Platform down → KPN customers can't change set-top box channels | V1, V13, V14, V21, V22, V23 [1] |
| Attacker controls smart-city traffic lights via tenant compromise | V9, V8, V16, V17 (tenant foothold → lateral movement to platform) [1] |

**Rigor notes:** (1) The document assigns V23 two different descriptions — the vulnerability table defines it as account lockout (used above), while the STRIDE section shows a KMS-encryption investigation note in the V23 slot [1]. (2) Risk distribution per the document's scoring export: 2 High (V9, V22), 10 Medium, and the remainder Low [1]. (3) The stated summary "Low 3" conflicts with the itemized table, which lists 11 Low items [1].