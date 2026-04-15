# Doomsday scenario's

## One or more MQTT API keys are leaked and must be rotated
- No rotation mechanism in place

## A tenant reachable system component (e.g. msg-pki) is compromised
- msg-pki is reachable for tenant pods
- msg-pki can create signing requests and grant system level access to kafka
- msg-pki can reach secret-store (no zero trust): all secrets compromised
- msg-pki can reach platform-rest-api (no zero trust): complete tenant access
- tenant access: on lz platforms you can then run workloads inside kpn lz

## An external facing system component is compromised (tenant-rest-api / msg stack)
- can reach secret store (no zero trust)
- can reach platform rest api (no zero trust)
- can create kafka cert signing requests?
- has kafka certificate granting system access: either the cert already requested or possibility to request new one via msg-pki

## RBAC misconfiguration
- Tenant allowed to create crossplane resources
- Tenant allowed to create signing requests
- Tenant allowed access to k8s secrets/certs
- Tenant allowed to create k8s resources outside of ns
- Should be prevented by calico blocking access to kube api.

## ZooKeeper is compromised
- ZooKeeper is compromised and the attacker can alter kafka ACL's
- attacker can create whatever tenant resource

## S3 buckets are compromised
- Backups are poisoned, crossplane resources are poisoned
- Velero backups contain cleartext secrets

## GitHub compromise
- Attacker can run whatever in system/any namespace

## GitHub dev account / Dev laptop compromise
- Backdoor in system component on Harbor

## Malicious Tenant / Tenant Compromise on LZ
- Floods kafka disk (DoS on control plane)
- Floods monitoring stack (DoS on LGTM stack)
- Can reach LZ
- Reads all tenant secrets in clear text
- Exhausts conntrack limit, isolates tenant node from network