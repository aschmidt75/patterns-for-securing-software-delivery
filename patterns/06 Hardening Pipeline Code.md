# Pattern: Pipeline-as-Code Hardening

## Context
Continuous Integration (CI) and Continuous Delivery (CD) pipelines are defined by code, i.e. scripts, configuration files, and infrastructure definitions. This context is often referred to as "Pipeline-as-Code." 

Because pipelines touch all stages of the software lifecycle, any security weakness in this pipeline code or its execution environment can lead to compromised builds, unauthorized access, or manipulated artifacts.

## Problem
When CI/CD pipelines are not treated and protected as carefully as production code, they can become prime targets for attackers. Malicious users can exploit insecure scripts, hard-coded secrets, inadequate permissions, or misconfigurations to alter build outputs, gain elevated privileges, or exfiltrate sensitive data.

This pattern in particular relates to the execution of shell scipts/scriptlets in languages such as bash or PowerShell, and potential pitfalls.

## Forces

- **Script Execution**: Pipeline scripts often run with elevated privileges, which magnifies the impact of any compromise.
- **Multiple Services and Tools**: CI/CD involves various integrations (e.g., code repositories, artifact registries, container registries). Each integration is a potential exposure point.
- **Secrets and Credentials**: Pipelines rely on credentials and tokens (for repositories, cloud services, etc.) that, if poorly managed, can be leaked or misused. See also pattern [**04** - Secure Storage and Usage of Credentials in Pipelines](../patterns/04%20Secure%20Secrets%20Management.md)
- **High Velocity**: CI/CD is about rapid iteration and frequent deployments; security checks must be automated and unobtrusive to avoid slowing the development cycle.
- **High Turnaround Time**: Pipelines are essential for keeping systems up-and-running. Changes to pipeline code and setups are often done under time pressure.

## Solution

- The definition of a pipeline should be **treated as code**, much the same way as for the application itself. Prefer code-based pipeline definitions instead of visually constructed ones, which keep code internal.
Store the pipeline definition in code repositories, using the same protection policies (reviews, approvals, branch protections) as application code.  

    This way, regular scans of build scripts and configurations are possible. Static analysis tools ("SAST") can help to identify vulnerabilities such as command injection, hard-coded credentials and others.

- **Unit-test pipeline scripts**: when embedded scriptlets become too large or complex, move them out of the pipeline and refactor them into shell script files.Use unit testing frameworks to test the refactored scripts. Examples are [bats]() for bash and [Pester]() for Powershell.

- Additionally, **use linting tools for pipeline code/configuration where possible**. For example, use shell linters for shell scripts.

- **Validate Inputs and Environment variables**: A common source of weaknesses is from unsanitized inputs. Strictly validate any inputs to build scripts, including user-supplied parameters. Sanitize command-line arguments or environment variables, mitigating the risk of OS command injections.

- When writing shell script code, think about what the code can potentially do given the permissions it is executing with, and the credentials it has access to. 

- Enable **detailed logging of build steps**, especially around credential use and package retrieval. Alert on suspicious behaviors (e.g., unusual commands, excessive resource usage).


## Referenced CWEs

The following CWEs related to the above pattern:

- [CWE-78: OS Command Injection](https://cwe.mitre.org/data/definitions/78.html)  
- [CWE-522: Insufficiently Protected Credentials](https://cwe.mitre.org/data/definitions/522.html)  
- [CWE-798: Use of Hard-coded Credentials](https://cwe.mitre.org/data/definitions/798.html)  
- [CWE-89: SQL Injection](https://cwe.mitre.org/data/definitions/89.html) (if pipelines integrate with databases)  
- [CWE-532: Insecure Logging of Sensitive Information](https://cwe.mitre.org/data/definitions/532.html), although most of modern CI/CD pipeline system implement proper redaction of sensitive data in logs.

-----

When using shell scripts in pipelines, make sure to treat it the same way as application code from a security perspective: It needs special care to mitigate common weaknesses.