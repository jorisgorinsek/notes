**Take 2 product lines and investigate how security NFRs were taken along**
- 1 from ESL
- 1 from IAM or infra / ICE
Input: 
- cross-cutting requirments https://confluence.bare.pandrosion.org/pages/viewpage.action?pageId=235637054
- EDP PRS https://confluence.bare.pandrosion.org/spaces/EDPG/pages/271484333/EDP+Product+Release+Specifications
- ADRs https://confluence.bare.pandrosion.org/spaces/EDPG/pages/192905716/10-Architecture+Decision+Records
- 


**Interesting links**
- Setup a test env: https://confluence.bare.pandrosion.org/spaces/EDPG/pages/229346516/How+to+setup+e2e+qa+env
- lll

**D4P is the product line tools for platform development -> CI/CD and so on**

**Review CI/CD**
- for platform
- for tenants

**Review factory model**


### security in platform CI pipelines

CI runs https://gitlab.bare.pandrosion.org/edp/infrastructure/platform-development/workspaces/edp-connector-gatekeeper/-/jobs?kind=BUILD
#### edp-connector-gatekeeper
- **Renovate** keeps Go modules, npm packages, Docker base images and scanner image tags up to date. It opens MRs nightly via a scheduled CI pipeline. See [RENOVATE.md](./RENOVATE.md).

- **`gosec`** finds common Go security bugs: hardcoded credentials, unsafe `exec`, weak crypto, etc. Part of `task audit`.

- **`govulncheck`** flags CVEs in the Go stdlib and our modules. It is call-graph-aware, so it only fails when our code actually calls the vulnerable function. Part of `task audit`.

- **`staticcheck`, `errcheck`, `vet`** catch bugs and unchecked errors that often turn into security issues. Part of `task audit`.

- **`syft`** generates CycloneDX SBOMs for both container images. `task scan:sbom`.

- **`grype`** scans the SBOMs for CVEs. The gate is `--fail-on high --only-fixed`, so we fail the build only on findings that have a patch available.

- **`trivy config`** scans Helm charts, Kubernetes manifests and Dockerfiles for misconfiguration. Ignores live in [`.trivyignore`](./.trivyignore). `task scan:config`.

- **`go-licenses`** blocks Go modules with `forbidden`, `restricted` or `unknown` licenses. `task scan:licenses`.

renovate:
- enable cooldowns and define policy
	./vault-stack/renovate.json:2462:      "minimumReleaseAge": "1 day"
	./vault-eso-addon-base/renovate.json:88:      "minimumReleaseAge": "7 days"
	./vault-eso-addon-base/renovate.json:104:      "minimumReleaseAge": "3 days",
use taskfiles
trivy container is pulled on tag, not sha version and 2 months old (newer one is available) -> renovate not active on taskfiles

https://gitlab.bare.pandrosion.org/edp/infrastructure/platform-development/core/ice-client-access
SAST
secrets scanning



- [ ] Integrate findings Lander into document
- [ ] what needs to be in the document?
