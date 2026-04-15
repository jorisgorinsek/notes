
Presentation: https://www.youtube.com/watch?v=MTW4QMKSyGI
Slides: https://www.canva.com/design/DAGZrMSnRv4/ZGU-7Y15NvSHO0ivXNfU9w/view?utm_content=DAGZrMSnRv4

# What to focus on?
- Container 0-days are not so common, what are the real threats?
- Prevent container breakout
- What do actual attackers do? -> cryptomining!

## Preventing Container breakout

**for regular k8s**
Risks Priorities:
1) An engineer sets up RBAC   incorrectly
  
Mitigations:
- Seccomp DefaultRuntime

**for RCEaaS: the above +**
Risk prio:
1) An customer breaks out of the  container and compromises the  organization

Mitigations:
- gVisor, Kata, Firecracker
- Runtime enforcement tools
- Admission controllers

## Cluster level security
Issues with compliance driven security: false sense of security
- patching container CVEs within days
- CIS benchmark must all pass

### CIS benchmarks
- good hardening guide
- PSP and RBAC are great advice but are manual checks

**Auto tools to check for issues:**
- kubehound
- rbac-police

### Seccomp
- seccomp profiles will not make your containers into a sandbox
- **seccomp is like  SELinux: super powerful, easy to shoot yourself in the foot.**

**Seccomp profiles opstellen**
- inspektor-gadget om profiel te maken
- securitu-profiles-operator om profiel af te dwingen
- seccomp profiles nemen ook container runc etc mee want worden mee afgecheckt door seccomp profile -> container runtime heeft clone(2), socket(2) etc nodig!!

 Newer gVisor version is a lot more performant on IO operations

