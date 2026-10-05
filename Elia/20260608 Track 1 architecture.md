
**EDP acts as distribution provider**
- they provide software, Elia BE and  50Hz need to setup their own instance
- dev/test is maintained by EDP

**Requirements**
- will evolve over time
- have been started with ISV model as a requirement

**Glossary**
- Quantum = group of services that are co-dependent
- ICE = API to create infrastructure (abstraction towards the control plane) - IaaS component interacts with this one
- ESL - used to be called Service - provides ao k8s as a service -> interageert met Platform Resource Management dat op de rand zit en multi-tenancy voorziet
- DDI = DNS, DHCP and IP

Each tenant should be able to manage their own realms -> IAMaaS


3 different artifact stores used, different CI pipelines (each time does it their way) -> how do you guarantee quality?
- Currently they are converging to a single set of tools
	- Visie is dat ze via CI regels voor publicatie kunnen afdwingen en zo SDLC kunnen enforcen (e.g. vuln scanning, secrets in code etc)
 - SBOMs zijn er nog niet
 - is there a policy that governs CI usage?
 - 
Andriy thinks there is no SDLC documented for all teams
	- each time has their own pages etc
	- ESL domain as example gebruiken!
	- Architecten strugglen om mensen in check te houden rond gebruik van tech, processen, ...

Zoek miro board "platform level architecture" https://miro.com/app/board/uXjVLA2Z6Qs=/

Local operations team
- zij zien in dat operations om zo'n platform in de lucht te houden complex is en veel mensen nodig heeft
- specifieke kennis per domein nodig -> zij verwachten dat er verschillende profielen nodig zijn met specialiteiten

Some teams use open source
**Some teams like ESL write software from scratch**
-> Andriy is interessant persoon om hier te interviewen


All tenants get a dedicated database instance

Dichte integratie tussen PRM en IDP / secrets management omdat multi-tenancy hard leunt op accounts / secrets / workload identities







