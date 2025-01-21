# Pattern: Separate CI and CD Pipelines

## Context
Organizations adopting DevOps practices typically rely on continuous integration (CI) and continuous delivery (CD) pipelines to rapidly deliver new features. While CI focuses on building and testing code, CD automates the deployment process. These pipelines, if not well-segregated, can become a target for attackers, who might exploit build credentials or dependencies to gain broader access.

## Problem
When CI and CD steps happen in the same monolithic environment (i.e. the same pipeline), they collectively store sensitive credentials, sign artifacts, and perform deployments. A single security breach in one area may allow attackers to jump to other critical systems, compromising the entire software supply chain. 

## Forces
1. **Availability vs. Security**  
   DevOps teams prioritize rapid, streamlined workflows, but requiring isolation or stricter controls can slow down delivery.

2. **Credential Management**  
   Build systems often handle secrets used by both CI and CD. Poorly controlled secrets may leak, granting unauthorized access to critical environments.

3. **Deployment Automation**  
   Automated pipelines need direct access to production for seamless delivery. Overprivileged CI systems can drastically increase the blast radius if compromised.

4. **Compliance Requirements**  
   Regulatory standards often mandate logical separation between development/test and production environments, adding overhead to the pipeline architecture.

## Solution

<u>Maintain distinct environments</u> and often distinct toolchains <u>for CI and CD</u> to compartmentalize build and deployment processes. This can be achieved by:

1. **Logical Separation**  
   Use separate pipelines, credentials, storage, and build agents. Ensure CI never directly controls deployment to production; it merely produces artifacts for CD. Optionally use separate networks for CD.

2. **Minimal Access Privileges**  
   Grant only necessary permissions to each environment. CI servers should not have the ability to deploy directly to production. Grant segregated permissions to roles or departments.

3. **Artifact Repository**  
   Store build outputs in a secure artifact repository. CD pipelines fetch signed artifacts from this repository rather than from the CI environment. Separate AuthZ for CI (writing to the artifact repository) and CD (reading from the artifact repository).

4. **Regular Reviews**  
   Periodically assess configurations, credentials, and permissions to ensure continued alignment with best practices and compliance requirements.

## Related Patterns

- N/A

## Referenced CWEs

This pattern references the following items from Mitre's [CWE Top 25 Most Dangerous Software Weaknesses](https://cwe.mitre.org/top25/)

- [**CWE-862: Missing Authorization**](https://cwe.mitre.org/data/definitions/862.html) – Insufficient authorization checks on pipeline steps can lead to unauthorized deployment actions.

Additionally, the following CWEs can become relevant:

- [**CEW-732: Incorrect Permission Assignment for Critical Resource**](https://cwe.mitre.org/data/definitions/732.html) – Permissions for a security-critical resource are specified in a way that allows that resource to be read or modified by unintended actors.
- [**CWE-923: Improper Restriction of Communication Channel to Intended Endpoints**](https://cwe.mitre.org/data/definitions/923.html) - Pipeline established a communication channel to an endpoint for privileged or protected operations, but it does not properly ensure that it is communicating with the correct endpoint.
- [**CWE-306: Missing Authentication for Critical Function**](https://cwe.mitre.org/data/definitions/306.html) – Underlines the need for strong authentication and segregation of duties between build and deploy phases.
- [**CWE-669: Incorrect Resource Transfer Between Spheres***](https://cwe.mitre.org/data/definitions/669.html) – Focuses on secure artifact handoff between isolated environments.