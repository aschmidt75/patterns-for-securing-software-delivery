# Pattern: Pull-Based Deployment for Improved Pipeline Security

## Context
In modern software delivery pipelines, continuous integration and continuous delivery (CI/CD) processes rely on automated mechanisms to move code, configurations, and other assets from build servers to target environments (e.g., staging or production). Traditionally, the build server actively pushes artifacts and configuration changes to the target servers.

## Problem
When a pipeline or build server actively connects to target machines to push updates, several security issues arise:

1. **Exposed Credentials**  
   The build server needs to store access credentials for each target machine. Compromise of these credentials can lead to unauthorized access to critical systems.

2. **Network Path Exposure**  
   In a push model, the pipeline initiates connections and sends data across the network. This requires a network path from the pipeline to the target environemtn, which can be exploited if not secured properly.

3. **Expanded Attack Surface**  
   A compromised build server or intercepted pipeline could be leveraged by an attacker to pivot into production systems and escalate privileges.

## Forces
1. **Zero-Trust Environment**  
   Modern security paradigms discourage flat networks and direct trust relationships. Minimizing inbound connections aligns with least-privilege principles.

2. **Ease of Management vs. Security**  
   A push model can be more straightforward to conceptualize, but a pull model offers stronger security in exchange for potentially greater complexity.

3. **Automation Needs**  
   CI/CD pipelines rely heavily on automation. Balancing robust authentication and encryption with seamless operation can be challenging.

4. **Operational Overhead**  
   Pull-based deployments may require additional orchestration, especially for large-scale environments, but the security benefits often justify the effort.

## Solution
<u>Adopt a pull-based deployment model</u>, where each target server is responsible for <u>fetching</u> (pulling) artifacts and configurations <u>from a trusted artifact repository</u>.

### Benefits
- **Credential Isolation**  
  Each target server only needs to authenticate against the artifact repository (e.g., using tokens or certificates). The build server no longer stores broad access credentials.

- **Reduced Attack Surface**  
  By eliminating direct push from build servers, there’s less risk of a compromised pipeline being used as an attack vector on production environments.

- **Integrity & Verification**  
  Each target server can verify artifact integrity (e.g., checksums or signatures) before deployment. This helps mitigate tampering risks during transit.

- **Granular Control**  
  Each environment like staging, production, etc. can enforce its own security policies, limiting deployment only to approved and validated artifacts.

## Referenced CWEs

This pattern references the following items from Mitre's [CWE Top 25 Most Dangerous Software Weaknesses](https://cwe.mitre.org/top25/)

- - [**CWE-287**: Improper Authentication](https://cwe.mitre.org/data/definitions/287.html)  

Additionally, the following CWEs can become relevant:

- [**CWE-494**: Download of Code Without Integrity Check](https://cwe.mitre.org/data/definitions/494.html)  
- [**CWE-829**: Inclusion of Functionality from Untrusted Control Sphere](https://cwe.mitre.org/data/definiti
ons/829.html)
- [**CWE-1390**: Weak Authentication](https://cwe.mitre.org/data/definitions/1390.html)  