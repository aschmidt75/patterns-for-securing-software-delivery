# Pattern: Use Pipeline Built-In Commands Instead of Shell Scripts

## Context
Modern software delivery relies heavily on Continuous Integration (CI) and Continuous Delivery (CD) pipelines. These pipelines often include various stages such as build, test, security scanning, and deployment, to ensure the code is stable, secure, and ready for release. While shell scripts have traditionally been used for many of these tasks, most CI/CD platforms now offer built-in commands or native steps that provide standardized, more secure functionalities.

## Problem

Shell scripts can harbor hidden security and maintainability issues, such as command injection and inconsistent environment handling. 

Shell scripts might

- depend on correct shell environment settings that may vary per platform or agent.  
- Are prone to user input issues, which can lead to unintended command execution if not validated properly.  
- May have insufficient logging and auditing, hindering troubleshooting and compliance.  

Reliance on these scripts exposes the pipeline to a wide range of potential weaknesses that could compromise security or lead to inconsistent pipeline execution.

## Forces
1. **Maintainability vs. Flexibility**  
   Shell scripts are flexible, but that flexibility often leads to ad hoc solutions that become difficult to maintain over time.

2. **Security vs. Speed**  
   Quickly coding a shell script might seem efficient in the short term, but security vulnerabilities or misconfigurations can be introduced if not carefully reviewed. 

3. **Visibility vs. Complexity**  
   Pipeline steps built into the CI/CD tool typically provide standardized logs and simpler configuration. Shell scripts, on the other hand, can have complex, nested logic that obscures what is actually happening.

4. **Portability vs. Specificity**  
   Shell scripts may rely on specific command-line tools available in certain environments only. Native pipeline steps usually handle cross-platform requirements more smoothly.

## Solution

The solutions is to **adopt built-in or native pipeline commands wherever possible**.

1. **Native Steps**  
   Use CI/CD-native steps for routine tasks such as code checkout, artifact management, unit testing, scanning, and deployment. These steps usually come with pre-configured security controls and standardized logging.

2. **Secure Inputs**  
   If shell-like commands are unavoidable (for example, for small tasks not covered by native steps), encapsulate them in the pipeline’s own command builder or parameterized environment. This mitigates direct command injection risks.

3. **Audited Pipeline Configuration**  
   Document pipeline stages using the CI/CD system’s declarative configuration (e.g., Jenkinsfile, GitHub Actions YAML, GitLab CI YAML). This approach ensures transparency in what the pipeline executes and how it’s configured.

4. **Validate Environment**  
   Verify that environment variables and tool dependencies used in the pipeline are well-defined. Avoid passing sensitive data (e.g., credentials) through shell arguments. Instead, use native credential management mechanisms provided by the CI/CD platform.

## Referenced CWEs

The following weaknesses are related:

- [CWE-77: Command Injection](https://cwe.mitre.org/data/definitions/77.html)  
- [CWE-78: OS Command Injection](https://cwe.mitre.org/data/definitions/78.html)  
- [CWE-94: Code Injection](https://cwe.mitre.org/data/definitions/94.html)  

----

By replacing shell scripts with built-in commands and features, teams reduce the overall risk surface, simplify maintenance, and improve the clarity of the pipeline’s function and security posture.
