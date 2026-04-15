Security baseline needed
Scanning tool that checks baseline for all github actions on DSH

Questions:
- do we pin third party actions on hash? same for workflows (if we reuse third party ones)
- do we enforce pinning on hash for actions?
- do we ensure least privilege on actions? -> permissions, scope
- do we use GH runners of self-hosted? I think GH?
	- monitor network activity of GH actions -> commercial tools, any OSS available?
- do we run OSSF scorecard action on third party?
- do we do anything with  GH actions audit logs?
- Do we receive alerts when actions have a vulnerability?
- Do we use dependabot to upstep actions? How?
	- Dependabot doesn't upstep actions pinned to a hash, https://github.com/suzuki-shunsuke/pinact can
- Are our own actions safe for injection (private repo so lower prio, but good to check in any case)
- Can we roll our credentials atomically? 
- Review use of secrets in actions 
	- Are they really needed?
	- Is access restricted to only parts of the pipeline that need access?
- Review of permissions in the whole CI/CD pipeline
	- cfr https://snyk.io/blog/so-you-think-your-ci-cd-environment-is-secure/#the-typical-ci-cd-pipeline