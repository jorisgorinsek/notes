
**Hadoop - Map Reduce**: map = verspreid stukken data over servers, reduce = collecteer de verwerkte subresulaten terug tot 1 resultaat
(-) data komt nog steeds van disk = slow

**Spark**
+ snelheidsprobleem van Hadoop oplossen
+ houdt data in memory
+ sneller

(-) Enkel voor data engineers (scala & Java)

-> data scientists use Python! -> PySpark

(-) Operational overhead of clusters etc was huge (maintenance, security etc)

**Databricks**
- neemt operational stuff over (infra complexity)
- data teams moeten enkel met data bezig zijn
=> managed spark, notebooks etc

**Massive data lakes**
- without governance, quality assurance etc -> data lakes becomes data swamp
- mixed data types (also unstructured data)

**In parallel: Data warehouses (Snowflake)**
- SQL oriented vs code (python on data lakes)
- structured data only

**Data lakehouse**
- e.g. delta lake
- Merging data lakes and data warehouses
-  Adds transactions, schemas and versioning to data lakes
- single data layer (SQL based or script bases querying) on top of delta lake

-> audience problem solved
-> new problem: access management at org level: auditing etc


**Unity catalog**
Adds Schema, permissions, Lineage etc

**Add AI**
- avoid waiting for data scientists etc but query using prompts
- Add Mosaic AI
- 

![[Pasted image 20260504143232.png]]







