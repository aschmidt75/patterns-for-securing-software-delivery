# Pattern: Enforce Proper Access Control & Least Privilege in CI/CD Pipelines

This pattern ensures that CI/CD pipelines operate with the minimal necessary privileges, guarding against unauthorized actions and reducing the overall risk to software delivery workflows.

## Context
Modern software delivery relies on Continuous Integration and Continuous Delivery (CI/CD) pipelines, often spanning multiple services, build agents, and cloud infrastructures. These pipelines must handle sensitive operations such as code compilation, artifact storage, secrets management, and deployment to production systems. As a result, rigorous Identity and Access Management (IAM) policies and Role-Based Access Control (RBAC) are critical to safeguard these processes.

## Problem
Without proper access controls, unauthorized parties or processes can gain elevated privileges, manipulate the pipeline, access sensitive secrets, or compromise production environments. Inadequate RBAC and violation of the [Principle of Least Privilege (PoLP)](https://en.wikipedia.org/wiki/Principle_of_least_privilege) create significant vulnerabilities, including:

- Build pipelines that can accidentally deploy to production with improper approvals.
- Service accounts with overly wide permissions, increasing the blast radius of a breach.
- Credentials stored in insecure ways, allowing lateral movement within systems.
- Challenges identifying and auditing who performed which actions in the pipeline.

## Forces
1. **Complex Access Requirements**: Multiple teams, systems, and automated processes need varying levels of access (read-only vs. write vs. deploy).
2. **Frequent Pipeline Changes**: CI/CD pipelines evolve, sometimes under Time-to-Market pressure, requiring continuous updates to policies and permissions.
3. **Shared Infrastructure**: Common build agents and repositories can become choke points if misconfigured, allowing unintended cross-team access.
4. **Audit and Compliance**: Organizations must meet regulatory requirements, requiring clear visibility and traceability of who has access to what.

## Solution
1. **Plan and Implement RBAC**  
   Determine 
   - the purpose of a pipeline (CI vs. CD, see also related pattern)
   
   and decide 
   - which teams shoud have access to both pipeline code and/or configuration,
   - how the pipeline is going to be accessed,
   - which systems the pipeline should have access to, and how.

   Carefully plan Access Controls for your CI/CD environment. Assign specific roles or permissions to each team, service account, or automated process. Ensure each role has only the permissions needed to perform its tasks. Also plan ahead for reviews of the role assignments to verify ongoing adherence to least privilege.  

2. **Use Strong Identity and Access Management (IAM)**  
   Integrate with a centralized IAM system (e.g., OAuth, SAML, or the cloud provider's built-in IAM) to unify access across source control, build servers, artifact repositories, and deployment targets. Enforce multi-factor authentication (MFA) where applicable to reduce risk of credential theft.

3. **Adopt Least Privilege by Default**  
   Determine how your CI/CD software stack allows to fine-tune permissions. Start with the minimal set of permissions for every pipeline stage and build up as necessary.  

4. **Implement Continuous Monitoring and Auditing**  
   Determine what capabilities your CI/CD software stack has for logging and auditing. This includes logging of pipeline activities, in particular who is modifying pipeline definitions and changes to access policies.  

## Related Patterns

- [**01** - Separate Continuous Integration from Continuous Delivery Pipelines](./patterns/01%20CI%20CD%20Separation.md)

## Referenced CWEs
- [**CWE-923: Improper Restriction of Communication Channel to Intended Endpoints**](https://cwe.mitre.org/data/definitions/923.html) relates to CI/CD pipeline that communicates with another endpoint (e.g. production environment), but it does not ensure it's the correct endpoint. 
- [**CWE-269: Improper Privilege Management**](https://cwe.mitre.org/data/definitions/269.html) - Example: Pipelines run under a machine identity with a configurable set of permissions. If this permission set is too broad, adversaries can gain additional options in their attacks.
- [**CWE-732: Incorrect Permission Assignment for Critical Resource**](https://cwe.mitre.org/data/definitions/732.html) - A pipeline may have a proper permission set, but this is still too broad for the resources being accessed (e.g. writing to configuration items, or reading from variables)
- [**CWE-862: Missing Authorization**](https://cwe.mitre.org/data/definitions/862.html) - 
