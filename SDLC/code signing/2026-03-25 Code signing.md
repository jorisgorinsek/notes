**What needs to be signed?**
- rpms
- mostly TAR files and other binary blobs

Currently use checksum + checksum tool to check integrity (avoid corruption)
- todo = everywhere the install looks at checksum, it need to look at the signature instead

New = signature (kind of checksum created with public key)

ASML IT uses  Keyfactor signserver (commercial tool)
- Post binary
- Server creates signature (HSM to store keys)
- Delegate all signing to this service
- Access to signing server is protected using OAuth

Downside 
- dependency on external service
- need to submit full binary (can be >20GB)

Alt: pgp signatures
pro:
con: getting old (uses older crypto - not elliptic curve?)

Alt: cosign
pro: 
- no need to submit the full binary to signserver
- can sign generic binary blobs, so technology agnostic (can sign jar files, tar files, container images, ...)
con: 

**Note**
- Need to refer to SHA id of image instead of a (spoofable) tag when installing to ensure the image is genuine

**Bootstrapping**
- unsigned rpm that can be installed to install the signatures

**Signing**
- option 1: integrate signing into the pipelines
- alt option: upload in artifactory triggers the signing process -> what is threat model here?
- 