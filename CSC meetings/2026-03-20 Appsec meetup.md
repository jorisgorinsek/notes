
### Security requirements & testing
IEC 62443-4-2 gebruiken om af te checken of security requirements goed getest worden. Daarnaast de asvs natuurlijk, heeft heel concrete specs voor tests.

  
### SAST
Er bestaat een benchmark voor dat tooling- Juliet by the nsa? 
- Requires quite a bit of analysis to evaluate 
-  Appsec Santa compares appsec tools
- Output van sast tools kan geverifieerd worden tegen threat model om false positives te checken.

  
### Metrics
Metrics: measure time to triage: long ttt indicates that devs don't really understand the issue

  
### DSH
DSH: alle user input komt uiteindelijk uit als topic naam op Kafka of service naam in k8s etc. Input validatie zit op het niveau van de individuele componenten (is op zich ok) maar betekent dat data op Kafka untrusted is. Dit input val testen is een requirement voor elke system consument van Kafka. Testen met fuzzing. Abuse tests toevoegen.