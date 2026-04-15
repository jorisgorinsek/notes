**Downsides of open source:**
- maintenance
- support
- Limited features (post detection) 

Open Source  is good for detection


## What do we need to protect

### Priorities
**Critical**
- CSPM (cloudsec posture mgmt)
- Secrets detection
- In-app Firewall
- SCA

**Priority**
- IaC scanning
- Malware detection
- SAST
- DAST

**TODO**
- License protection
- end-of-life components


## Interesting Tools

### Supply chain security

#### SCA
- Trivy

#### Malware detection
- Phylum (opensource-ish) https://docs.phylum.io/ Free tier abandoned
	- -> absorbed by Veracode 
	- https://github.com/phylum-dev/cli
	- API that you can send lockfiles and manifests to for analysis


**EoL components**
- endoflife.date

**Compliance / License**
- Syft

### Protecting Source Code
#### Secrets detection
- trufflehog
- ggshield (opensource-ish) https://www.gitguardian.com

integrate it as a git hook
- each secrets found must be revoked
- before it gets committed to the git repo (available in history)
### Securing cloud infra
- CSPM -> cloudsploit
	- aqua security project
	-  https://github.com/aquasecurity/cloudsploit?tab=readme-ov-file#background
	- Read-only rechten geven op uw cloud account en scan laten lopen -> wat komt daar uit?
	- TODO: uitproberen
- IaC scanning -> checkov

### Securing the application

#### In-app firewall 

-> Zen (aikido) -> similar to WAF, can detect injection

#### SAST
- Bandit (python)
- GoSec
- Semgrep

#### DAST
- Nuclei
- ZAP

![[Pasted image 20260105135534.png]]

