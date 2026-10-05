The more I think about security, the more it becomes clear I don't have a clear picture of what the security boundaries internally are.

I'm very curious about the complete picture and what should be isolated.

What I think, in KPN context, that I have access to

- we have the klarrio IDP domain, with internal tooling, confluence, ....
- we have the google domain
- we have the lastpass domain
- we have the github domain
- we have the KPN domain (gating AWS, Azure, ... )
What is not clear is what is supposed to be isolated.

e.g.

- breach of github can lead to breach of AWS, Azure,... (it can trigger admin level deployments)
- breach of lastpass may lead to complete breach (I separate most 2FA's with at most one in lastpass, but I don't think that is mandatory. I don't know if 2FA via mail is possible anywhere)
- breach of google domain could be used for password resets (I don't know)
- klarrio idp (confluence) is I think the most innocent (lots of interesting info, but no credentials)

Samenzitten hierover om threat model KPN DSH voor te bereiden. Roel betrekken.