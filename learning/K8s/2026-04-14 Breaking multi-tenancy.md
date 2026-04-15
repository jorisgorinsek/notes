https://www.youtube.com/watch?v=PEU4uDzurvc
https://github.com/ernw/k8s-multi-tenancy

**Namespace based multi-tenancy is challenging**

### **New problems discovered**

#### Insecure Cross-namespace references in CRDs

- Kubeflow: machine learning and AI platform for k8s
	- Every user has their own namespace
	- Users can create virtual services in Istio
	- Istio gateway exists in the *gateway* namespace
	- The Istio virt service exists in *workload* namespace (user has access as it can create virt svcs)
	-  attacker can redirect certain URIs to their own pod, accessing the cookies in that request. This includes e.g. oauth2-proxy cookie which allows attacker to impersonate the user

Research paper: https://arxiv.org/pdf/2507.03387

#### Insecure Cross-Namespace References in Annotations
- Traefik was vulnerable
	-  Traefik uses ServersTransport CRD which defines e.g. which client cert to use for mTLS auth. This CRD is referenced using an Annotation
	- Attacker can also reference the same CRD with a cross-namespace annotation
	- The attacker can now use the client secret on requests without having actual access to it, Traefik will fill it in for them
	- Impact can go beyond the cluster: attacker can also reference resources that are hosted outside of the cluster, e.g. in EKS infra
	-
#### Cross-Namespace attacks on Data Plane
- Istio Gateway API does not have this issue, it can sercurely access cross-namespace

### **Lessons learned**
- Methodology to find this kind of issues

**How to identify potential weaknesses?**
- **Apply hardening**: PSP etc
- **Identify all component running on cluster ->** which resources are in control of a tenant?
- **Assess resources:** 
	- what control plane (direct or indirect access to k8s API) interaction?
	- What data plane (what can they reach as services) interaction?

**Addressing weaknesses:**
- use admission policy sets, e.g. Kyverno
- define custom policies where needed for cluster specific stuff
	- best = allowlist what they can do
-