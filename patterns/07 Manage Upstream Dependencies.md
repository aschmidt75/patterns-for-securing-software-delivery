# Pattern: Manage Upstream Dependencies explicitely

## Context

Modern CI/CD pipelines often rely on upstream dependencies (framework, libraries, tooling), which can affect both your application code and the build environment. These dependencies, if not properly managed, can introduce vulnerabilities that compromise the security of your software supply chain.

## Problem

When third-party libraries or frameworks are used without proper vetting, they introduce potential vulnerabilities into the software delivery process. Attackers can exploit weaknesses in unpatched or malicious dependencies, placing the entire application at risk.


## Forces

- **Automation vs. Oversight**: Automated build pipelines allow for rapid iteration, but require continuous security checks to avoid blindly incorporating vulnerable dependencies.

- **Complex Dependency Trees**: Modern applications often rely on nested libraries. Even if direct dependencies are safe, transitive dependencies might not be.

- **Version Conflicts**: Pinning or locking versions can avoid introducing vulnerabilities unintentionally, but requires careful management to avoid dependency conflicts or missing out on important upgrades.

    The most recent functionalities or security fixes can get taken automatically using a "latest-version" approach, but this contrast #4 above.

- **Tooling and Integration Complexity**: Security scanning tools, such as Software Composition Analysis (SCA) solutions, need to integrate smoothly with existing CI/CD pipelines without causing excessive performance overhead or complicating workflows.


## Solution

To secure CI/CD pipelines, consider the following practices:

**Pin and Lock Dependencies**  
- Use dependency management files (e.g., `package-lock.json` for JavaScript or `go.mod` for Go) to pin specific versions.  
- When possible, pin via **hashes** to ensure the exact same artifact is retrieved, mitigating supply chain attacks.

**Scan for Vulnerabilities**  
- Integrate **SCA (Software Composition Analysis)** tools into the CI pipeline to detect known vulnerabilities in upstream libraries.  
- Configure automated scans at every build or pull request to ensure new vulnerabilities are flagged early.

**Establish a Review Process**  
- Require code reviews for dependency updates.  
- When upgrading, check release notes or security advisories for new vulnerabilities or patched exploits.

**Align with Secure Build Infrastructure**  
- When using system-provided build extensions, make sure to follow their versioning scheme as well, use recent versions.
- Use secure registries or artifact repositories with stringent access controls for storing trusted dependencies.

## Referenced CWEs

The following weaknesses are related:

- [CWE-1104: Use of Unmaintained Third-Party Components](https://cwe.mitre.org/data/definitions/1104.html)
- [CWE-829: Inclusion of Functionality from Untrusted Control Sphere](https://cwe.mitre.org/data/definitions/829.html)
- [CWE-494: Download of Code Without Integrity Check](https://cwe.mitre.org/data/definitions/494.html)


--------
By adopting this secure upstream dependency management pattern, you can substantially reduce the risk of introducing vulnerabilities through external libraries.