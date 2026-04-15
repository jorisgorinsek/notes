**Agenda**:
- Risk assessment
- reference frameworks
- use case (virtual)

## CRA recap
### Scope
- remote data processing: guidance is that 2 way traffic is required to comprise remote data processing. i.e. logging for e.g. remote maintenance would not count as RDP

#### Requirements
1. (Annex I part I 1.) secure by design (risk based) -> harmonized standard in draft: prEN 40000-1-2
2. (Annex I part I 2.) requirements (will replace the EN 18031) ->  40000-1-4 (work in progress)
3. (Annex I part II). Vulnerability mgmt: HS in draft: 40000-1-3

### Interplay with Product Liability Directive (PLD)
- manufacturer might be liable for damages through software update
- BUT can also be liable for damages through lack of updates (i.e. don't patch in time, damages occur as a consequence)

## Use case

**Definition of Making available vs placing on the market:**
- making available: supply of a product in the course of a commercial activity
- placing on the market: first making it available -> **for each unit** the first moment it is made available on the market (i.e. leaves your warehouse in a commercial transaction to distributor) 
-> in practice there is not much of a difference unless you are a distributor etc. manuf. makes available first (=place on market), then distributor makes available for purchase by retail, then retail makes available for purchase by end customers

## Risk management

Next time: check on compliance & strictness of checking compliance with CCB representative. Concept of "80% compliance" - in line with reality of support in supply chain

1. threat model
2. score risks (include intended purpose, operating env, length of time in use)
3. determine which requirements (annex I part I  art 2.) are applicable based on risk
4. Document how manuf. applied art 1 (secure by design) and art 2 (vuln mgmt)

- Reasonably foreseeable use:
	- from pov of average user: might be helpful to create personas to describe different user profiles
	- consider usage beyond typical use cases (i.e. product aimed at professionals but is easily accessible for end customer -> need to consider this as well)

Q: do you need an SBOM before starting the risk assessment? Consensus is NO, but you will not cover all risks related to the supply chain! Full list of vulnerabilities does give an important overview of how risky a certain component in the supply chain is.

**Slides cover whole of section 6 of prEN 40000-1-2**

Daikin uses intuitive threat modeling using a catalog of threats which devs have to check up front and then us
RAPID threat modeling at Daikin: (typically 3 to 4 hours)
 - DFD prepped up front -> checklist to check for completeness. Use the C4 model for zooming in.
 - threat catalog up front -> e.g. 3demb (MITRE for embedded)
 - facilitated by someone of the security team
 - timebox it and come back to it regularly
 
Vertical standard for hardware devices with security functions has nice list of threats and counter measures -> welke vertical standard?
## Guidance & reference frameworks
