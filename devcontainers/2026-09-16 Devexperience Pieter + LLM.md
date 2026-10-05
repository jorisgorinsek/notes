Rutger uses devpods to develop in a container without an IDE

### Improvements for current devcontainers
- container images bouwen en gebruiken in CI
- 1 central repo met onze images
- 1 container per taal

### Struggles / issues
- netwerk switching van wifi naar fixed geeft issues
- docker-in-docker werkt ok (wel de devcontainer versie) -> bekijken.

### Input LLM
Introducing devcontainers across 100 cloud-native developers is a great architectural move for environment reproducibility, but isolating blast radius across projects requires careful design.

  

**Security & Blast Radius**

  

- **Docker Socket Mounts (`/var/run/docker.sock`):** Mounting the host Docker socket into the container gives the container root-equivalent access to the host machine. If compromised, an attacker can break out immediately, negating project isolation.
    
      
    - _Mitigation:_ Use **Docker-in-Docker (DinD)**, **Rootless Docker**, or daemonless container tools like **Podman** or **Buildah** inside the devcontainer.
        
          
        
- **Credential & Secret Leaks via Mounts:** Mounting `~/.aws`, `~/.kube`, or SSH keys directly from the host into the container exposes global credentials.
    
      
    - _Mitigation:_ Use short-lived credentials (e.g., AWS IAM Identity Center/OIDC with short sessions), pass scoped tokens via environment variables, or rely on SSH agent forwarding (`ssh-agent`) rather than mounting private key files directly.
        
          
        
- **GitHub Fine-Grained Personal Access Tokens (PATs):** Classic PATs grant broad access across repositories.
    
      
    - _Mitigation:_ Enforce fine-grained PATs restricted strictly to the single repository, or utilize repository-scoped GitHub App tokens with short lifetimes.
        
          
        
- **Caching & Artifact Pollution:** Shared local image caches or volume mounts across containers can create side-channels for cross-project data leakage.
    
      
    

**Developer Experience (DX) & Performance**

  

- **I/O Bottlenecks on Non-Linux Hosts:** macOS and Windows rely on virtual machines to run Docker. Binding large source trees (like massive `node_modules` or target build directories) via bind mounts results in significant disk I/O slowdowns.
    
      
    - _Mitigation:_ Use named Docker volumes for dependency directories or leverage feature flags like VirtioFS on macOS.
        
          
        
- **Extension Overhead & Container Bloat:** Pre-installing every possible tool and VS Code extension bloats image size, leading to slow startup times and high disk usage across 100 engineers.
    
      
    - _Mitigation:_ Keep base devcontainer images lean. Rely on `devcontainer.json` "features" for optional tooling rather than bundling everything into a single monolithic Dockerfile.
        
          
        
- **Resource Contention:** Running multiple devcontainers concurrently can quickly consume host RAM, CPU, and disk space.
    
      
    

**Governance & Maintenance**

  

- **Supply Chain Risks of Base Images:** If devcontainer definitions are fully decentralized, project teams might pull unverified third-party base images or pin outdated, vulnerable dependencies.
    
      
    - _Mitigation:_ Build and publish standard, hardened internal base images (hosted on your internal container registry) that project-level devcontainers must inherit from.
        
          
        
- **Drift Across Projects:** Tool versions (e.g., `kubectl`, `helm`, language runtimes) will drift if not centrally audited or routinely updated via automated PRs (e.g., Dependabot or Renovate for devcontainer features).
    
      
    

**Architectural Overview**

```
+-----------------------------------------------------------------------------------+
| Host Machine (macOS / Linux / Windows)                                           |
|                                                                                   |
|  +-------------------------------------+   +------------------------------------+ |
|  | Devcontainer: Project A             |   | Devcontainer: Project B            | |
|  | - Scoped AWS/K8s Credentials        |   | - Scoped AWS/K8s Credentials       | |
|  | - Isolated DinD / Podman            |   | - Isolated DinD / Podman           | |
|  | - Fine-grained Repo PAT             |   | - Fine-grained Repo PAT            | |
|  +-------------------------------------+   +------------------------------------+ |
|                    ^                                         ^                    |
+--------------------+-----------------------------------------+--------------------+
                     |                                         |
                     +-------------------+---------------------+
                                         |
                                         v
                         +-------------------------------+
                         | Internal Container Registry    |
                         | (Hardened Base Images)        |
                         +-------------------------------+
```


**Do not abandon devcontainers for Flatpak/Bubblewrap.**

  
Abandoning devcontainers for native Bubblewrap (`bwrap`) replaces a widely adopted, ecosystem-backed standard (`devcontainer.json` supported by VS Code, JetBrains, and GitHub Codespaces) with a massive custom engineering burden. More importantly, **Bubblewrap nested namespaces break rootless container engines like Podman** due to missing subUID allocations and cgroup v2 mapping issues. You would solve one sandboxing problem only to break your cloud-native toolchains (e.g., `docker build`, `helm`, local K8s clusters).

  

Instead, address the root causes of DinD (Docker-in-Docker) failures in your devcontainers.

  

### Root Causes of DinD Friction in Devcontainers

1. **Storage Driver Incompatibility:** DinD tries to use `overlay2` inside an environment that already uses overlayFS, causing mount panics or severe performance degradation.
    
      
    
2. **Cgroup v2 Mapping Failures:** Modern Linux hosts use Cgroups v2. Nested containers fail to establish cgroup delegation without elevated permissions.
    
      
    
3. **Privileged Mode Security Holes:** Standard DinD requires `--privileged`, which bypasses container isolation—violating your goal to limit the blast radius.
    
      
    
4. **Volume Mount Path Confusion:** Mounting paths from inside a DinD container uses the _host's_ file pathing, causing silent volume mount failures.
    
      
    

### Strategies to Fix or Eliminate DinD

**1. Replace DinD with Sysbox (Best for Seamless Isolation)**

Use [Sysbox](https://github.com/nestybox/sysbox) as the container runtime for your devcontainers.

  

- **Why it works:** Sysbox creates nested virtual-container environments with real rootless user namespaces.
    
      
    
- **Benefits:** Developers can run systemd, Docker, and Podman inside the devcontainer **without using `--privileged`** and without storage driver bugs. It seamlessly isolates the child container from the host system.
    
      
    

**2. Swap Docker with Rootless Podman inside the Devcontainer**

If developers primarily need to _build_ images or run simple containerized dependencies:

  

- Install `podman` inside the devcontainer base image instead of `dockerd`.
    
      
    
- Set up alias rules (`alias docker=podman`).
    
      
    
- Use `vfs` or `fuse-overlayfs` inside the devcontainer storage configuration (`/etc/containers/storage.conf`) to eliminate nested overlayFS storage crashes.
    
      
    

**3. Use Daemonless Build Tools for CI/CD & Image Creation**

Often developers use DinD just to run `docker build`. Replace `docker build` with daemonless CLI tools that run entirely in user space inside the devcontainer without needing a Docker socket:

  

- **Kaniko** or **Buildah** for building OCI images.
    
      
    
- **Ko** (for Go) or **Jib** (for Java) to compile images directly from source without a Docker daemon.
    
      
    

**4. Transition Local K8s to Dev-Cloud Clusters**

If developers use DinD to run local Kubernetes clusters (e.g., `kind` or `minikube`), offload the compute:

  

- Use tools like **vcluster**, **Telepresence**, or **Mirrord**.
    
      
    
- The devcontainer holds the local source code and tools (`kubectl`, `helm`), but routes traffic directly to an isolated namespace in a remote dev Kubernetes cluster. This completely removes the need to run container engines inside the devcontainer.
    
      
    

### What to check with your team

To give you the exact `devcontainer.json` or runtime configuration to fix this, ask your developers:

  

1. **What specifically breaks when using DinD?** (e.g., storage driver errors, cgroup permissions, slow file I/O, or broken volume mounts?)
    
      
    
2. **What are they using DinD for?** (e.g., running `docker build`, spinning up integration test services via `docker-compose`, or running local K8s via `kind`?)
    
      
    
3. **What host OS are they running?** (macOS, Windows/WSL2, or native Linux?)