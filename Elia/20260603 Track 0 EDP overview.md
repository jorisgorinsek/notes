Group project = complications
- see slide 1, no details yet
- Platform dev & test and platform operations for dev & test (phase 2) is done at Group level. Group is not a legal entity etc. If not resolved this becomes an unrecoverable issue

**EDP Operations Framework**
- Look into this

**EDP Governance framework CNAG**
- App sec and data governance are part of CNAG
- Sample code, reference architectures etc

**3 stages in development**
1. Platform dev & test
2. Platform operations for application dev & test
3. Platform operations for application operations

Pipelines, SCM for platform development -> phase 1
Pipelines, SCM etc for applicatons -> phase 2

Assumption is that they are in phase 2 now

**Deployment for operations (phase 3)**
- 1 deployment in BE for ETB
- 1 in BE  for 50Hz

**Integrating EDP in existing IT landscape**
1. Operations integration -> no operations teams in BE or DE
2. Network integration
3. IAM integration
4. EUD integration
5. Security integration

EDP claims they are not responsible for these integrations 
- e.g. IAM: EDP can come with IAM, but can also integrate into existing IAM. BE and DE different view on how the integration should be done.  

**Complexity: dev & test environment needs to be integrated with both ETB and 50Hz IT landscape (e.g. IAM)**
- example of "settlement" business case: one side chose Spark - other side chose Cassandra for processing data (rejected the Spark proposal)

Concept of "you build it, you run it" is very foreign for current IT and development.

They claim the data centers on ETB and 50Hz are not sufficient to run critical applications -> DE is building 2 new data centers in BE too. DE data centers will go live summer 2027

**PMF milestone**: functionally complete (should be) - some non-functionals too
**GA: milestone:** non-functionals should be ok 

**Lack of maturity of the rest of the organization is main challenge for EDP**
- e.g. explaining CN, shared respo etc
- **they claim Digital transformation strategy is not even clear yet**
- 

**Rationale for EDP**
- data volume and direction (no longer just Southbound) that comes with energy transition

