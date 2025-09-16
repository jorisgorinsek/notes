Guideline about remote data processing is being created but is not going to be released any time soon (expect sometime in 2026)

"Making available on the market" -> for a product this is clear, how to interpret this for software? 

Supplier management
-------------------

- activate threat intel feeds about suppliers (e.g. news about ransomware attack)
- ask them to prove ISO27001 conformity or SOC2 type 2
-> these 2 don't cover SDLC or only slightly but are _third party attested_
- ask for security team contact details directly

Cetome overview of IoT security legislation worldwide

NIS2 compliance in Hungary requires ETSI EN 303 645 augmented with NIST SP 800-213A and SP 800-53 rev 5 (scope is smart devices).

SAMM assessments

- don't send out the questionnaire 
- for a medium risk app, involve sec champ from another team
- for a high risk app, involve external consultant
- teams should own the roadmap and take the initiative to improve
- full questionaire every 2 years


---
## Notes Arnaud
After a recap on the CRA requirements that might lead us to more open talks, the first topic would be on the reporting obligations. As second topic and if time allows it, I suggest the vulnerability management. A useful link on that matter is the [the beta version of the vulnerability database of ENISA](https://euvd.enisa.europa.eu/).

### Where to get started? Frameworks, guidelines, directions for an action plan:

1. High-level requirements and information provided (ENISA mapping of existing standards to the CRA)  
    à discussion started about how to define data that can be collected with regards to the data minimisation principle: limited to what is necessary in relation to the intended purpose of the product  
    à also discussion on the type of security testing, suggestions went from pentesting to source code SAST/DAST scans  
    à the harmonised standards will take too long to be finalised, so they are to be considered in a later phase (drafts will be consultable in the Belgian standardisation committee, please contact me if wishing to be added to this group).
2. As a starting point, a risk assessment needs to be performed, but no real methodology is mentioned. The harmonised standard for the first requirement (appropriate level of cybersecurity based on the risks) will not give methodology but rather where to set the bar (and it will not provide presumption of conformity). Within the CRA Expert group, there is an ad-hoc group for drafting risk assessment guidelines (first meeting on 14/04, not (yet) very productive).
3. It was deemed that, in case a manufacturer is not really advanced with product security, a good framework to start with is the ETSI 303 645 which is 30-40 pages and freely available ([v2.1.1](https://www.etsi.org/deliver/etsi_en/303600_303699/303645/02.01.01_60/en_303645v020101p.pdf) from 2020 mentioned in the EN 18031 or CRA mapping of ENISA and [v3.1.3](https://www.etsi.org/deliver/etsi_en/303600_303699/303645/03.01.03_60/en_303645v030103p.pdf) of late 2024). Also, ETSI 303 645 contains a risk assessment methodology, which is not the case for EN 18031.
4. Then, EN 18031 (-1 for internet connexion and -2 for data processing) would be an ideal candidate for an advanced cybersecurity level approaching the CRA requirements, except the vulnerability requirements (but those are more straightforward). The standard is heavier (184 and 224 pages) The gaps were identified in a presentation given by Applus Laboratories (see enclosed, especially slides 16 to 24).
5. IEC 62443-4-2 should be strongly influencing the CRA harmonised standards and is mapped in EN 18031, so they are also to be considered for complex products, however it is a heavier standard (196 pages) that needs time investment.

### Supply chain management:

1. High-level information provided (presentation given during an international conference and used as starting point for our discussion)
2. The complex situation of components placed on the market (= first making available) before the application date of December 2027 (thus not having to comply with the CRA requirements) and used in a product placed on the market after the application date (having to comply), was highlighted. It seems that the manufacturer will have to take full responsibility for those components.  
    - The only way forward seems to ensure, contractually if possible, that at least some important CRA requirements are integrated in the component, and that some kind of vulnerability handling is ensured. But it is not easy at all.  
    -  Another (partial) solution is to test those for security (again DAST/SAST scans or pentesting). Some security benches exist in Germany and Netherland, Sirris would like to build one in Belgium.
3. In general, contracts with supplier are key to ensure the right part of CRA responsibilities on each side (SBOM, vulnerability handling, security patches, etc.). Exit clauses should be ensured.
4. For the OSS steward definition, it was made clear it must be a legal entity. Also, you can possibly be a product manufacturer and an OSS steward. Commission guidelines are being developed for the OSS definition (monetised or not) and the steward definition. With OSS, the fear is that several projects would end up without steward, and then the responsibility would be completely at the manufacturer's expense. It is very difficult to handle vulnerabilities coming from OSS. At least a set of rules to handle OSS should be set up (an internal acceptable use policy), such as checking who is the developer,, identify its asset owner, checking if an OS project is not abandoned, and then check if the findings match the internal policy.
5. Other talks went on ensuring the use of OWASP SAMM containing the minimum needed for a secure development lifecycle, and on rapid threat model prototyping (RTMP).

