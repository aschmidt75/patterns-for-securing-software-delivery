# Patterns for Securing of Software Delivery

This repository aims to be Catalog of Patterns For Improving the Security of Software Delivery Systems. Topics are around the Software Development Lifecycle (SDLC), DevSecOps and IT Security. 

It is based on Common Weaknesses of Mitre's Common Weakness Enumeration&trade;, in particular [2024's CWE Top 25 Most Dangerous Software Weaknesses](https://cwe.mitre.org/top25/) as well as [CAPEC, the Common Attack Pattern Enumeration and Classification&trade;](https://capec.mitre.org/index.html).

# Patterns

- [**01** - Separate Continuous Integration from Continuous Delivery Pipelines](./patterns/01%20CI%20CD%20Separation.md)
- Review and Enforce proper Access Control and Least Privilege
- [**03** - Consider Pull Model Deployments instead of Pushing Deployments](./patterns/03%20From%20Push%20to%20Pull%20model%20Deployment.md)
- Secure Storage and Usage of Credentials in Pipelines
- Validate Inputs
- Harden Pipeline Code and Environments
- Run Security Scans in Pipelines
- Manage Upstream Dependencies

# Supplementary Material

- [SDLC-related CAPEC entries](./supplementary/S01%20SDLC-related%20CAPEC.md)

# License

(C)opyright 2024,2025 @aschmidt75

Licensed under [Creative Commons BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

<img src="https://mirrors.creativecommons.org/presskit/buttons/88x31/png/by-sa.png" width="100px"> 

CWE is a trademark of The MITRE Corporation. CAPEC is a trademark of The MITRE Corporation.