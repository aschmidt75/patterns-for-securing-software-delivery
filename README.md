# Patterns for Securing of Software Delivery

This repository aims to be catalog of patterns for improving the security of software delivery systems. Topics are around the Software Development Lifecycle (SDLC), DevSecOps and IT Security. 

It is based on *Common Weaknesses* of Mitre's Common Weakness Enumeration&trade;, in particular [2024's CWE Top 25 Most Dangerous Software Weaknesses](https://cwe.mitre.org/top25/) as well as [CAPEC, the Common Attack Pattern Enumeration and Classification&trade;](https://capec.mitre.org/index.html).

# Patterns

- [**01** - Separate Continuous Integration from Continuous Delivery Pipelines](./patterns/01%20CI%20CD%20Separation.md)
- [**02** - Review and Enforce proper Access Control and Least Privilege](./patterns/02%20Enforce%20Proper%20Access%20Control%20and%20Least%20Privilege%20in%20CICD%20Pipelines.md)
- [**03** - Consider Pull Model Deployments instead of Pushing Deployments](./patterns/03%20From%20Push%20to%20Pull%20model%20Deployment.md)
- Secure Storage and Usage of Credentials in Pipelines
- Replace shell scripts by Pipeline extensions
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