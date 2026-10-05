**Versies**
Calico v3.31.5  
Kubernetes: Server Version: v1.35.6-eks-bca9cf6

**Calico docs**
- pods volgt het de defaults van k8s (default allow) 
- VM's en host interfaces is het default deny



### Spec

| Field | Description                                  | Accepted Values                                        | Schema | Default |
| ----- | -------------------------------------------- | ------------------------------------------------------ | ------ | ------- |
| nets  | The IP networks/CIDRs to include in the set. | Valid IPv4 or IPv6 CIDRs, for example "192.0.2.128/25" | list   |         |

Die eeste is order 9 de 2de order 10 dus die eerste wordt eerst gechecked en heeft geen match dus de 2de komt in actie en die denied


From https://docs.tigera.io/calico/latest/reference/resources/globalnetworkpolicy#entityrule

When using selectors in network policy, remember that selectors only match (known) resources, but rules match packets. A rule with a selector all() won't match "all packets", it will match "packets from all in-scope endpoints and network sets". *To match all packets, do not include a selector in the rule at all.*

| Field | Description                                       | Accepted Values                                                                                           | Schema        | Default |
| ----- | ------------------------------------------------- | --------------------------------------------------------------------------------------------------------- | ------------- | ------- |
| nets  | Match packets with IP in any of the listed CIDRs. | List of valid IPv4 CIDRs or list of valid IPv6 CIDRs (IPv4 and IPv6 CIDRs shouldn't be mixed in one rule) | list of cidrs |         |

source: {} means "all" (wat we ook gebruiken in de correcte versie)  
destination: {} also means "all" (consistent en ok te verwachten)  
  
De vraag is of:  
Destination:  
   nets: []  
  
Ook "all" zou moeten zijn?

We have 2 GlobalNetworkPolicies
-  private-lb-tenant-policy (order 9) to allow Egress TCP traffic to one specific host for all namespaces that match the namespaceSelector
- tenant-policy (order 10) to deny egress TCP traffic for all namespaces that match the namespaceSelector

We noticed that in private-lb-tenant-policy if the destination nets rule is empty like below, it matches all packets and as a result all TCP egress is allowed.

This behavior surprised us. We knew source: {} means "all" 
destination: {} also means "all" 
  
De vraag is of:  
Destination:  
   nets: []  
   
Is this intended behavior or did we discover an issue?
