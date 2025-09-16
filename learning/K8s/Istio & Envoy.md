- fundamentals leren van Istio
- Istio opzetten op K3s 
- httpbin installeren
- keycloak opzetten op k3s
- istio-ingress gebruiken om services te exposen
- oauth2-proxy opzetten op k3s
- custom authorization policy introduceren om oauth2-proxy
- add observability
- opt: add spiffe

## Istio Control plane

- Istiod: Istiod is the core component of the control plane, which configures, secures, and man-  
    ages the Envoy proxies in the data plane with the combined functionality of these three components:
    
    - Pilot (Traffic management): Configures Envoy proxies (including gateways) with routing  
        rules, service discovery, and other policies.
        
    - Citadel (Security): Provides secure service communication through authentication, authorization, and certificate management.
        
    - Galley (Configuration management): Collects, validates, and distributes configuration data  
        to Envoy proxies (including gateways).
        

## Istio custom resources

- Authorization policies to create HTTPRoute to pod
    - An authorization policy is a way to specify the rules and policies for the communication between the pods or services in the service mesh
        
- VirtualService
    - A virtual service is a way to specify the routing rules and policies for the traffic to a service or a subset of a service.
        
- Destination rule
    - A destination rule is a way to specify the policies and parameters for the traffic to a service or a subset of a service.
        
- Service entry
    - A service entry6 is a way to add an external service to the service mesh, and to enable the communication between the internal services and the external service.
        

- Certificate management: Istiod acts as the certificate authority within CDP, issuing and man-  
    aging the rotation of certificates for workloads. These certificates are crucial for establishing  
    the identity and facilitating secure communication between services.
    
- Certificate agent functions: Running on each node, the Istio certificate agent interacts with  
    Istiod to request, renew, distribute, and rotate certificates for pods and services.