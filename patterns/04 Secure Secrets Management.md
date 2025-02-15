# Pattern: Secure Secrets Management in CI/CD Pipelines

## Context
Continuous Integration and Continuous Delivery (CI/CD) pipelines are central to automating the build, test, and deployment processes. These pipelines often require credentials like API keys, passwords, SSH keys, and certificates to interact with external services (e.g., cloud platforms, code repositories, and container registries). 

Securely managing these sensitive assets is crucial for protecting the integrity and confidentiality of the software supply chain.

## Problem

When credentials or other sensitive information are stored or handled improperly in CI/CD pipelines (e.g., embedded in code, stored in cleartext, or logged by scripts), they can be exposed or leaked.

Attackers can exploit such weaknesses to gain access to systems, databases, or services, putting production environments at risk.

## Forces
1. **Automation vs. Security**: CI/CD pipelines must be efficient and scalable while balancing the need to keep credentials out of source code and away from unauthorized access.  
2. **Developer Convenience vs. Best Practices**: Storing credentials in environment variables, code, or configuration files might simplify setup but significantly increases the risk of accidental exposure.  
3. **Distributed Teams**: In many organizations, multiple teams and tools integrate with the pipeline. This increases the attack surface if secrets are not carefully managed.  
4. **Audit & Traceability**: Compliance with industry regulations often requires secure audit trails and the ability to trace access to sensitive information. A solution must help maintain compliance while reducing friction.  
5. **Frequent Credential Rotation**: Regularly rotating credentials improves security but introduces operational complexity if not automated or well-integrated into the pipeline.

## Solution

Probably the most important element is to never *hardcode* any kind of secret, but instead **securely inject secrets into the build execution**. This can typically be done through secure environment variables or pull mechanisms within the build steps.

Secondly, this injection should use a **dedicated secret management solution**. This can be a Key Vault as an external service, or functionality within the pipeline environment. Example of this are Open Source Secret Management Solutions such as Hashicorp Vault, Cloud provider services or equivalents. One of the advantages about these solutions is their implementation of role-based access control on the secrets to determine who (or which process) may read or write the data.

Consider **Credential Rotation and a short lifespan**. This will help to reduce the impact of compromised tokens. At the same time, a (typically external) process is required to rotate credentials and make them available to the pipeline.

A further security improvement is to **limit the scope of credentials** that are being used in a pipeline. This closely relates to the principle of least privilege, where only the minimum required priviledges are provided to fulfil the request.

Most modern build execution environments will redact injected secrets in text outputs as to **protect logs and configuration files** by default.  

As an additional security measure, it can be helpful to introduce **Audit logs and Monitoring**. Enabling the logging of secrets retrieval events helps to provide a complete audit trail. Upon that, alerts can be configured to detect and report anomalies such as unusual access times or repeated failed attempts to retrieve secrets. 

## Referenced CWEs and CAPEC

The following weaknesses are related:

- [**CWE-256**: Plaintext Storage of a Password](https://cwe.mitre.org/data/definitions/256.html)  
- [**CWE-312**: Cleartext Storage of Sensitive Information](https://cwe.mitre.org/data/definitions/312.html)  
- [**CWE-319**: Cleartext Transmission of Sensitive Information](https://cwe.mitre.org/data/definitions/319.html)  
- [**CWE-798**: Use of Hard-coded Credentials](https://cwe.mitre.org/data/definitions/798.html)  
- [**CWE-522**: Insufficiently Protected Credentials](https://cwe.mitre.org/data/definitions/522.html)  

Related pattern from CAPEC are [**CAPEC-545**: Pull Data from System Resources](https://capec.mitre.org/data/definitions/545.html) and [**CAPEC-150**: Collect Data from Common Resource Locations](https://capec.mitre.org/data/definitions/150.html) where attackers who managed to compromise a build environment could try to collect sensitive data from logs or scripts.

---

By systematically implementing a secure secrets management strategy in your CI/CD pipelines, you enhance the overall security posture of your software delivery process and reduce the risk of credential compromise.
