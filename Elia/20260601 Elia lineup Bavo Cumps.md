## Feedback Bavo Cumps

-  IAM, CI/CD was all designed without input from other Elia departments
-  archi is working on central IAM
-  they have central DevSecOp etc which is not used by EDP 
-  A lot of information (at architecture level) is missing
  -  non-functional is missing -> SLOs SLAs
  
-  EDP promised a lot they did not deliver
  - service mesh, API management, edge deployment etc
  - a lot of development was stopped because of EDP (e.g. API mgmt)
  
- original mission in 2022 did not include sovereignty

Andreas became CIO later on (initially brought in as consultant)

2 orgs
 - 50Hertz -> bought IT solutions 
 - Elia -> .NET + SAP custom solution
 
 Huge gap between what they have now and full cloud native. 
 - Current deployments are typically done manually
 - In critical applications -> deploy manually in secured zones of the network, not possible on EDP

Upgrade of SCADA-IMS (that steers the grid) takes 2,5 years with extensive testing

Blackout proof -> no electricity, no internet, no network
To recover:
-> power the grid step by step
-> communicate with backbone of the grip (via pylons)
-> power plants to start them without power
-> in BE, the data centers are on-site with the control centers


**Green - orange - Red** zoning in the network is defined but not ready yet.

**Belgium and 50Htz grids are not connected.** So 2 separate instances are needed -> 1 BE and 1 in Neurerlande (East-G + Hamburg + Berlin)
 
**SDLC is a gap, CISO dept is working on improving that**

**EDP is setting up their own services, not aligned with IT**
- what happens when the EDP people leave? They are almost all external -> big problem

Is Pieter-Jan Gums capable of operating such a platform? With the proper SLA and SLOs

SXP = smart energy platform, deepest integration into EDP for now 
MCCS = SCADA, Voltcontrol is small part of it

For each tenant you need separate VPN tunnels
No data sharing facilities provided between tenants