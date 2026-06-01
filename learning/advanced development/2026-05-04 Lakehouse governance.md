
**Structured data formats**
- row-based format such as Apache Avro or Google’s Protobuf, 
- column-based format like Apache Parquet


2 layers 
1. IAM layer
2. Catalog services

![[Pasted image 20260504161642.png]]

**Data Asset Model**
The governance of a resource with respect to the lakehouse commonly describes the relationship between a policy and a governable object known as a data asset. In the simplest traditional sense, a data asset is a TABLE or VIEW, and a policy is a GRANT permission.

**ABAC**

- Columns are tagged to indicate if they can be read or should be masked for general access
e.g. only consumer_privileged group has access to columns with "pii" tag of customer orders table