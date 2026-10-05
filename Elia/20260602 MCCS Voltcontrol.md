TSO - Transmission System Operator

control center - controls the grid that Elia is operating

Historic need for upgrading the system -> don't use COTS but build internally
https://www.mccs.com/en/platform-and-products

Runtime env where they can put critital part of MCCS
 -> comply to German law for critical systems
 -> **initially the modules they bring to EDP are not critical**

Discussed about bringing HA, critical application to EDP
-> earliest 2027-2028


Around 80 devs, 150 total

USing all the Azure native tools -> repos, CI pipelines (Azure devops)
Azure mode 2, mode 3 not allowed for production grade runtimes
Azure mode 1 -> is ok?

Test setup on Berlin datacenter (test env) with Volt control as one of the first tenant applications
Waiting for dev/test environment, available since end of April
 - trying to get artefacts from Azure running on  dev/test env in Frankfurth
 - 

### Onboarding experience

1. contact customer success team
2. explain what you want to do
3. onboarding workshops so EDP team can understand what you need to build (C4 diagrams)
	1. document tech stack etc
4. EDP team has questionnaire for tenants: what persistence do you need, are your services stateless, ...
5. Determine product fit with EDP team
6. e.g. Azure keyvault migration to hashicorp vault for EDP

Good fit - MCCS team was already working cloud native way

harness -> harbor
argo on cluster pulls OCI images from harbor

2 separate instances of harbor: internet facing and internal facing ->

artifactory -> cluster is not possible
artifactory -> harbor -> cluster is possible

access to EDP is always via VPN

corporate laptops cannot use VPN
so they develop on other laptop -> 


