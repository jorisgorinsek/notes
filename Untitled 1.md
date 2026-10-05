Performance and load testing
- **What the suite contains** - six performance tests, defined in TestRail (suite [E2E_Perf](https://testrail.bare.pandrosion.org/index.php?/suites/view/6833&group_by=cases:section_id&group_order=asc&display=tree)) and documented under the [Load and Performance Testing](https://confluence.bare.pandrosion.org/pages/viewpage.action?pageId=477824031) hub; the most recent execution was the [Perf[2026-04]](https://testrail.bare.pandrosion.org/index.php?/plans/view/31695) run. They all run from a QA-Perf runner cluster in VE-VIRT against targets in VE-IDT (AZ1), are control-plane / provisioning-focused, with bounded concurrency, strict cleanup and a one-hour hard cap:
    
    - **PostgreSQL Baseline** - a moderate k6 baseline (10 virtual users, ~5 min) against four Postgres shapes (nano/small/medium/large); read/write/mixed × 4 shapes = 12 cases (not a stress, soak or spike test). [[details](https://confluence.bare.pandrosion.org/pages/viewpage.action?pageId=635797969) · [TestRail](https://testrail.bare.pandrosion.org/index.php?/suites/view/6833&group_by=cases:section_id&group_order=asc&display=tree&group_id=37566)]
        
    - **Ingress Load** - exercises the Cilium ingress controller and Kubernetes API control plane by creating waves of Ingress/Service/Deployment objects (ClusterLoader2) and measuring pod-ready latency (threshold p99 < 5 s). [[details](https://confluence.bare.pandrosion.org/pages/viewpage.action?pageId=637730866) · [TestRail](https://testrail.bare.pandrosion.org/index.php?/suites/view/6833&group_by=cases:section_id&group_order=asc&display=tree&group_id=41694)]
        
    - **PVC Load** - dynamic PersistentVolumeClaim provisioning across three StorageClasses (ext4 / nfs / xfs), measuring volume bind + attach + pod-ready latency (threshold p99 < 5 s). [[details](https://confluence.bare.pandrosion.org/pages/viewpage.action?pageId=637730867) · [TestRail](https://testrail.bare.pandrosion.org/index.php?/suites/view/6833&group_by=cases:section_id&group_order=asc&display=tree&group_id=41695)]
        
    - **Cluster Creation** - the full Kubernetes cluster lifecycle via the nzero CLI (create → Ready → delete) at 5/10/15 concurrent users (Locust), measuring cluster-provisioning throughput. [[details](https://confluence.bare.pandrosion.org/pages/viewpage.action?pageId=633504452) · [TestRail](https://testrail.bare.pandrosion.org/index.php?/suites/view/6833&group_by=cases:section_id&group_order=asc&display=tree&group_id=42968)]
        
    - **K8s Networking Bandwidth** - pod-to-pod TCP/UDP throughput (iperf3) across same-node, cross-node and cross-cluster paths with a TCP MSS sweep (e.g. TCP same-node > 40 Gbps, cross-node > 8 Gbps, cross-cluster > 5 Gbps). [[details](https://confluence.bare.pandrosion.org/pages/viewpage.action?pageId=637730868) · [TestRail](https://testrail.bare.pandrosion.org/index.php?/suites/view/6833&group_by=cases:section_id&group_order=asc&display=tree&group_id=46179)]
        
    - **Object Storage** - the full S3 bucket+key lifecycle via the nzero CLI (create → Ready → delete) at 5/10/20 concurrent users (Locust), measuring object-storage provisioning. [[details](https://confluence.bare.pandrosion.org/pages/viewpage.action?pageId=637730869) · [TestRail](https://testrail.bare.pandrosion.org/index.php?/suites/view/6833&group_by=cases:section_id&group_order=asc&display=tree&group_id=46555)]
        
    
    Infra-level performance tests are also defined - network (pod-to-pod throughput), storage (block + object) and provisioning (simultaneous cluster creation) - alongside Ceph benchmarking and network performance testing.
    
    **Why they are not run regularly (current state)**
    
    - They require a **dedicated environment**. Attempts to run them in the shared VE environment made the tests act as **noisy neighbours** to co-located workloads; they are not run there.
        
    - At the time of those attempts, **observability was not yet in place** to capture and visualise the results properly.
        
    - Standing up a dedicated environment (with observability) to run the suite regularly is **on the roadmap**.
        
    
    **Access to setup & reports**
    
    - Runnable tests / setup → TestRail [test plan 31695](https://testrail.bare.pandrosion.org/index.php?/plans/view/31695).
        
    - Concepts, catalogue & past analyses (e.g. the PostgreSQL write-bottleneck study) → the [Load and Performance Testing](https://confluence.bare.pandrosion.org/pages/viewpage.action?pageId=477824031) hub.
        
    - Test code → GitLab ().
        
    
    **References:**
    
    - [Load and Performance Testing](https://confluence.bare.pandrosion.org/pages/viewpage.action?pageId=477824031)
        
    - [Nightly Perf Tests (catalogue)](https://confluence.bare.pandrosion.org/pages/viewpage.action?pageId=633504408)
        
    - [Infra performance testing](https://confluence.bare.pandrosion.org/pages/viewpage.action?pageId=313692635)
        
    - [Network Performance Testing](https://confluence.bare.pandrosion.org/pages/viewpage.action?pageId=313692094)
        
    - [Ceph Benchmarking (ARCH)](https://confluence.bare.pandrosion.org/pages/viewpage.action?pageId=82870361)
        
    - TestRail test plan - [plans/view/31695](https://testrail.bare.pandrosion.org/index.php?/plans/view/31695)




# I-SD-B
[I-SD-B-1]

Not all credentials are stored in secret store. e.g. from ESL risk register [https://confluence.bare.pandrosion.org/spaces/SRV/pages/566558793/ESL+Risk+Register](https://confluence.bare.pandrosion.org/spaces/SRV/pages/566558793/ESL+Risk+Register)  
11 Legacy kubeconfig has static admin token | Insecure for obvious reasons (excessive permissions, long or even infinite lifetime, ...) | Compliance / Security | NKS4 |- Likely 4 - | Major | [Oleksandr Ryzhyi](https://confluence.bare.pandrosion.org/display/~x1oryzhyi@corp.transmission-it.de) | Accepted until WLID & usage actively discouraged

- Hashicorp Vault is being used to store most platform secrets
    
- Different vault deployments for production DE / BE and app dev/test.
    
- Some PL teams perform secrets scanning in CI, e.g. Infra team in [https://gitlab.bare.pandrosion.org/edp/infrastructure/platform-development/core/ice-client-access](https://gitlab.bare.pandrosion.org/edp/infrastructure/platform-development/core/ice-client-access)
    
- No proper least privilege implementation for access to secrets: platform developers have access to all secrets on app dev/test.
    

  
[I-SD-B-2]

- Have not seen any evidence that abnormal secret access is being logged and / or alerted on
    
- Secrets in code scanner is present in Harness pipelines and fails build when secrets are detected
    

  
[I-SD-B-3]

- Secrets are generated using Hashicorp Vault (or openBao soon). Due to the nature of SAMM scoring, I cannot award points on level 3 so increased the level 2 score instead.

# V-rt-A
[V-RT-A-1]

- The IAM team has a set of automated tests that test security test cases: [https://testrail.bare.pandrosion.org/index.php?/suites/overview/7](https://testrail.bare.pandrosion.org/index.php?/suites/overview/7)
    
- The infra team also has some security related automated tests, e.g. in the ICE API acceptance test suite [https://testrail.bare.pandrosion.org/index.php?/suites/view/4165&group_by=cases:section_id&group_order=asc&display=tree&display_deleted_cases=0](https://testrail.bare.pandrosion.org/index.php?/suites/view/4165&group_by=cases:section_id&group_order=asc&display=tree&display_deleted_cases=0)
    
- Platform acceptance test suite also tests for negative test cases (e.g. invalid password etc)
    

[V-RT-A-2]

- All test cases mentioned above are automated as test scripts
    

[V-RT-A-3]