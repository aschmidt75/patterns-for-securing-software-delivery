# CAPEC Attacks Mapped to Software Supply Chain Security (CI/CD/Pipelines)

Below is a curated list of CAPEC (Common Attack Pattern Enumeration and Classification) entries that most directly map to threats against the software supply chain, particularly focusing on continuous integration (CI), continuous delivery (CD), and pipeline environments. While some attacks are explicitly labeled as “supply chain” in CAPEC, others relate more broadly to CI/CD pipelines, build processes, or code repositories.

- **CAPEC-651, 659, 661, 662, 663, and 664** directly target software supply chain mechanisms, including code repositories, build systems, package repositories, and CI/CD pipelines.  
- These attacks are particularly dangerous because **automated pipelines tend to be highly trusted**. Once compromised, malicious changes can propagate quickly and widely.  
- **Broader CAPEC patterns** (e.g., command injection or interception attacks) are also applicable to pipeline stages if attackers can manipulate script inputs or intercept software components in transit.


## CAPEC-651: Malicious Software Delivery

**Why It’s Relevant**  
- Attackers compromise a step between development and delivery (e.g., package repository, artifact storage, or distribution mechanism).  
- Directly impacts supply chain security because the software delivered to end users (or to downstream internal stages) is tampered with.

**Typical Scenario / Rationale**  
- An attacker intercepts or tampers with software binaries after they are built but before they reach production or end consumers.  
- This can compromise CI/CD if the pipeline automatically consumes these tampered binaries during integration or deployment steps.


## CAPEC-659: Software Supply Chain Attack – Build System Modification

**Why It’s Relevant**  
- Targets the build process itself—attackers insert malicious changes where software is compiled or packaged.  
- Threatens CI pipelines directly because CI systems often run automated builds using scripts and configuration files that can be replaced or edited.

**Typical Scenario / Rationale**  
- Attacker gains unauthorized access to the build server or modifies build scripts (for example, injecting malicious code or altering compiler settings).  
- Since the build pipeline is automatically trusted in many organizations, any change made there propagates into all deployed applications.

## CAPEC-662: CI/CD Pipeline Poisoning

**Why It’s Relevant**  
- Specifically focuses on compromising the CI/CD pipeline itself.  
- Attackers can tamper with the pipeline configuration, environment variables, or script logic to introduce malicious artifacts, exfiltrate secrets, or bypass security checks.

**Typical Scenario / Rationale**  
- Injecting malicious steps into automated test or deployment jobs.  
- Modifying pipeline configuration to pull from a malicious repository or skip critical security scanning.  
- Poisoned pipelines pass malicious code down the line or embed backdoors in final artifacts.

## CAPEC-661: Software Supply Chain Attack – Infected Third-Party Software

**Why It’s Relevant**  
- Many CI/CD pipelines import third-party libraries or open-source components.  
- If those dependencies are infected or replaced with malicious versions, the entire supply chain is compromised.

**Typical Scenario / Rationale**  
- Attackers compromise an open-source project or a package repository (e.g., npm, PyPI, Maven Central).  
- Once malicious packages are included in a build, downstream builds and deployments automatically propagate the infection.

## CAPEC-663: Software Supply Chain Attack – Source Code Repackaging

**Why It’s Relevant**  
- Targets source code or repositories directly, repackaging them with hidden or altered logic.  
- Maps to scenarios where CI pipelines pull code from a source repository (Git, SVN, etc.) that has been replaced or subtly changed.

**Typical Scenario / Rationale**  
- A public fork of a popular repository is injected with malicious commits or hidden changes.  
- Build systems relying on that repository unknowingly compile the malicious logic.

## CAPEC-664: Software Supply Chain Attack – Malicious Source Code Injection

**Why It’s Relevant**  
- Similar in spirit to repackaging, but emphasizes direct injection of malicious logic rather than just re-bundling.  
- A developer or attacker with repository write-access inserts malicious code that appears legitimate, bypasses minimal or automated checks, and then flows through CI/CD.

**Typical Scenario / Rationale**  
- Attacker compromises developer credentials or leverages unprotected CI/CD tokens to push malicious commits.  
- Because the code injection looks like a normal commit, automated pipelines accept it and push malicious code into production.

## Other Potentially Relevant CAPECs

While not always explicitly labeled as “supply chain” or “CI/CD” attacks, the following can also threaten build pipelines or the broader software supply chain when combined with the above patterns:

1. **CAPEC-479: Malicious Code Injection**  
   - Broad category of injecting code into systems or applications. When aimed at build scripts, CI/CD config files, or repository hooks, it becomes a supply chain risk.

2. **CAPEC-438: Interception**  
   - Focuses on man-in-the-middle scenarios. In a CI/CD context, if network traffic to artifact storage or code repositories is intercepted, attackers can introduce modified artifacts or exfiltrate secrets.

3. **CAPEC-517: Obfuscated Files or Information**  
   - Attackers could hide malicious code in build artifacts or scripts. If the pipeline’s security checks fail to detect obfuscation, malicious logic can slip through.

4. **CAPEC-110: Command Injection**  
   - If CI/CD scripts allow unsanitized input or environment variables, attackers may craft input leading to arbitrary command execution in the pipeline environment.


