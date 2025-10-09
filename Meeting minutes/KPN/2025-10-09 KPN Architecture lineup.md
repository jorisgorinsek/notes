Topics
- external secrets operator for strimzi
- Audit logging over LFM

### Audit logging over LFM
- integrating with SOC needs to be implemented
- AJ wants it for political reasons
- We need to document why we think this is a bad idea -> data in flight is the issue
	- how long is the window in time that logs are in flight
	- worst case -> overload the Kafka increasing delays up to 5 or 10 minutes
	  the moment a log line is put in Kafka, it will be processed unless Kafka is stopped at which point the SIEM should start alarming
	- typical case -> seconds at most
- Stream = Service logs to Loki, Alloy to Kafka
- ![[Pasted image 20251009161849.png]]
- Logs will arrive on Kafka sooner than in Loki, because there is no buffer
	- Should be nearly instantaneously\


### External secrets operator for Strimzi
- not really useful for this feature?
- Pieter will look into this
- This would be really useful to replace all SOPS encrypted secrets now in Git
- Now the sops encrypted secrets are decrypted using key in lcoud KMS and synced to the platform as k8s secrets
- For tenant secrets, ESO would be interested
	- right now in plain text secrets -> at rest etcd is encrypted
	- in the cloud on the backup is encrypted
- Is the same with ESO unless the secrets are injected directly into the container (e.g. with a sidecar the way Vault does it)
- Even for tenant secrets ESO does not bring a lot unless you can get rid of the k8s step in between and move to direct injection
- 
