| ID  | Type            | Name                                                         | Description                                                                                                                                                                                                              |
| --- | --------------- | ------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| E1  | External entity | Tenant Admin                                                 | Human actor; obtains a JWT via the OAuth client credentials flow (TR5) and sends API requests over the public network to the reverse proxy (TR1) [1]                                                                     |
| E2  | External entity | Tenant Apps                                                  | Tenant workloads on the shared Kafka data plane; explicitly marked out of scope in the source DFD, shown here only to depict the shared data plane [1]                                                                   |
| P1  | Process         | Route and Authorize Requests (Reverse Proxy: Traefik + Kong) | Merged into a single process — the reason trust boundary TR2 was removed [1]; validates the JWT against Keycloak and forwards the request with the JWT for authorization [1]                                             |
| P2  | Process         | Issue and Validate Tokens (Keycloak / IAM)                   | Shared infrastructure for 5 platforms [1]; issues JWTs and supports validation by the reverse proxy (TR4)                                                                                                                |
| P3  | Process         | Manage Tenant Resources (Tenant REST API)                    | Creates client secrets (plaintext submitted to the Secret Store with a Limits Verifier reference), creates limits against the Limits Verifier without a request/response pattern, and manages tenant topics on Kafka [1] |
| P4  | Process         | Verify Usage Limits (Limits Verifier)                        | Receives fire-and-forget limit creations from the Tenant REST API [1]                                                                                                                                                    |
| D1  | Data store      | Secret Store (K8s Secrets)                                   | Receives secrets in plaintext; marked "out of scope" in the source legend but retained per TR6 as "an important asset we want to consider"; encrypted at rest with AWS KMS (EKS) [1]                                     |
| D2  | Data store      | Kafka (Shared Data Plane)                                    | Tenant topics; data plane shared between platform and tenants [1]                                                                                                                                                        |



| Trust boundary | Crossing | Status in source |
|---|---|---|
| TR1 | E1 ↔ P1 | Active [1] |
| TR2 | — | Removed: Traefik and Kong merged into one process [1] |
| TR3 | P1 ↔ P3 | Active [1] |
| TR4 | P1 ↔ P2 | Active [1] |
| TR5 | E1 ↔ P2 | Active (OAuth client credentials flow) [1] |
| TR6 | P3 ↔ D1 | Kept although "not really crossing a trust boundary," as an important asset [1] |
| TR7 | P3 ↔ D2 | Active [1] |
## Fidelity notes (rigor: extracted facts vs. inferences)

1. **No P3 ↔ P2 flow is drawn deliberately.** JWT validation happens only at the reverse proxy; components past Kong do not re-verify JWT signatures (V9/V18) DSH-KPN Tenant ...144829.pdf.
2. **Inferred flows** (direction supported by the bidirectional "↔" notation and context, but not spelled out as flows in the extracted text): D1 → P3 (read access exists per V20 "read/write access" and V19 "secrets are used as config" DSH-KPN Tenant ...144829.pdf); D2 → P3 (topic metadata); E2 ↔ D2 (supported by V22 "we share our data plane with the tenants" DSH-KPN Tenant ...144829.pdf).
3. **Omitted intentionally:** MQTT (mentioned in V1 but not a node in this threat model's DFD DSH-KPN Tenant ...144829.pdf), and logging infrastructure (Loki, S3 audit logs, Grafana) — these appear only as mitigations in the STRIDE analysis, not as DFD elements DSH-KPN Tenant ...144829.pdf.
4. The API response flow (P3 → P1 → E1) can carry the created/rotated service account secret — supported by the V5 remark that "any tenant user can retrieve the service account secret" DSH-KPN 