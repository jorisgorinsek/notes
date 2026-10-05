
The teams are expected to follow the full EDP Operating Model for Applications, from ideation to
production, using the platform’s capabilities.
-> in practice this is not the case (yet)

**Personas:**
 #edp_question- Application operator / incident manager for DE / BE but not for DEV/TEST?


### Validation
- early validation in dev/test
- rest of validation in PROD BE / DE -> separate integration env there?


### Provisioning of tenants
- no automation?
- access of tenants to EDP resources?
- tenants have k8s admin access?
- Tenant Personas do not get access to the EDP CLI (or
underlying services like EDP IaaS, EDP PRM) and EDP IAM.
All Tenant Personas get Administrative access to EDP
Developer Portal and already provisioned Tenant resources
at the moment.

![[Pasted image 20260611101011.png]]
 #edp_question -> example of this process being run?

### Code, scan, test and build
 #edp_question The EDP Artifact Registry -> 1 per az in dev/test? All tenants share artifact registry? Use of PAT to access artifactory -> access rights to other tenants 3rd party deps?

### Generate and sign artefacts
harbor push OCI container to tenant/dev/foo
- #edp_question> why dev, is this shared with other envs? -> enforced that i cannot pull from dev?

ORAS = OCI registry as storage

### Service deployment
 #edp_question Do you Platform may enforce runtime policy on the Tenant k8s clusters to deny privileged pods on EDP K8s within APP DEV/TEST

### Integration testing  and additional testing
The uber chart is managed in a dedicated repository with a custom build pipeline. The pipeline uploads the chart to a corresponding Harbor project. **All artefacts must be immutable.**

EDP Platform/Application Observability is not yet available as a Product for the Tenants to consume. We recommend using OTEL as a standard for collecting Traces, Metrics and Logs from your application.
You can of-course setup your own observability stack in the meantime.

### Handover to prod
**This is the only trust boundary mentioned sofar**
#edp_question what other trust boundaries are considered?

### Promote artefacts
![[Pasted image 20260611111944.png]]
#edp_question Does the toolchain to do this already exist?
#edp_question Does the promotion controller already exist? -> no, planned for Q3


