# Eclipse Foundation Secure Software Supply Chain Levels

v0.2

> The EF3SCL specification (also known as _gradually_) is an internal framework developed by the Eclipse Foundation to assess the security posture of its 420+ projects. The intended audience for this specification is the Eclipse Foundation projects. It is neither intended nor proposed as an alternative to other frameworks such as SLSA and SSDF. Instead, it builds upon these frameworks to define policies, interpretations, and practices specific to the Eclipse Foundation.
>
> The project is in a very early stage, and feedback is currently being solicited from leaders within Eclipse Foundation projects, notably the Eclipse Foundation Architecture Council.

## Introduction

This document outlines a comprehensive framework to enhance the security of software supply chains through progressive security practices. The framework is organized into key categories, each addressing different aspects of software development and deployment:

* **Secure the Developers**: Focuses on equipping developers with the knowledge and tools needed to protect their development environments from unauthorized access and vulnerabilities. Emphasizes continuous education and the adoption of best security practices.

* **Secure the Source Code Repositories**: Highlights the importance of securing access to code repositories. Includes practices such as using modern cryptographic algorithms, short-lived tokens for access management, restricting direct pushes to branches, and ensuring transparency in granting privileged access.

* **Secure the Code**: Concentrates on maintaining the integrity and security of the code. Includes peer review of commits to main branches, automated secret detection, and stringent security policies for vulnerability management.

* **Secure the Build**: Ensures that the build process is protected from unauthorized access and tampering. Involves using CI as Code for automation, employing ephemeral build environments, and generating build provenance to maintain integrity and traceability.

* **Secure the Dependencies**: Stresses rigorous management of third-party dependencies to prevent introducing vulnerabilities. Covers license compliance, software composition analysis, and secure channels for dependency retrieval.

* **Secure the Deployment**: Addresses practices for secure software deployment, such as release management automation and digital signing of released artifacts, to maintain security and integrity in production environments.

* **Secure the Consumption**: Focuses on the needs of downstream users, including continuous monitoring of SBOMs for vulnerabilities and establishing service level objectives for quick remediation of security alerts.

Each category is essential for building and maintaining a secure software supply chain. This structured approach by the Eclipse Foundation helps implement security practices at various levels, from basic measures to advanced practices that anticipate and mitigate emerging threats.

## Level 1: Launchpad 🚀

*Launchpad* represents the foundational level of security measures essential for any software project. It establishes basic yet critical practices to secure source code repositories, the code itself, its dependencies, and the build process. The focus is on setting up a strong base for security that will support more advanced practices in subsequent levels. This level ensures that projects are equipped with the necessary tools and processes to start their journey towards a secure software supply chain.

### Secure the source code repositories

* **Source code is managed in SCM:** Utilizes SCM systems for organizing and storing source code, enabling tracking of changes and collaboration among developers.
  * ✅ guaranteed by default by virtue of being an Eclipse Foundation project
* **Public record-keeping of granting privileged access to source code repositories:** Privileged access to source code repositories is awarded through a transparent, publicly recorded election.
  * ✅ guaranteed by default by virtue of being an Eclipse Foundation project and adhering to Eclipse Foundation Development Process.
* **MFA for privileged repository access:** Write access to source code repositories is protected by MFA.
  * ✅ guaranteed by default by virtue of being an Eclipse Foundation project and adhering to Eclipse Foundation Development Process.
* **Restriction on Force-Push (default branch only):** Prohibits the use of force-push to repositories, preventing the risk of overwriting history and losing code.

### Secure the source code

* **Security policy with a point of contact:** Implements a security policy (`SECURITY.md` file) listing a designated POC, centralizing communication and responsibility for security matters.

### Secure the dependencies

* **License compliance:** all third-party dependencies are properly vetted for license compliance and IP cleanliness.
  * ✅ guaranteed by default by virtue of being an Eclipse Foundation project and adhering to Eclipse Foundation Development Process.

### Secure the build

* **CI as Code:** Adopts a scripted approach to CI, enabling automation of the build process in a consistent and repeatable manner.
* **Use of build service:** Leverages a build service for compiling and testing code, ensuring builds are conducted in a controlled environment.

## Level 2: Orbit 🛰️

At the *Orbit* level, projects build upon the foundational practices established at the *Launchpad* level, implementing basic security measures with consistent application across the board. The goal is to enhance the security posture through stronger access controls, automated secret detection, and continuous security checks within the development lifecycle. This level aims to create a robust and resilient environment where basic security practices are not just implemented but are part of the project's ongoing operations.

### Secure the source repositories

* **Strong access control:** Enforces the use of strong and modern cryptographic algorithms (e.g., Ed25519 over RSA) for access keys (SSH) to source code repositories. Additionally, requires all access keys to be protected with a passphrase.

### Secure the source code

* **Automated secret detection:** Implements automated processes to continuously scan the source code repository for accidentally leaked secrets (e.g., API keys, passwords).
* **Continuous security checks:** Integrates a Static Application Security Testing (SAST) tool into your continuous integration workflows to identify potential vulnerabilities within the source code throughout the development lifecycle.
* **Enhanced security policy:** Maintains a comprehensive security policy that outlines supported versions and provides a secure persitent channel for reporting and discussing vulnerabilities (e.g., GitHub private vulnerability reporting, GitLab confidential issues).

### Secure the dependencies

* **Software Composition Analysis (SCA):** Employs an SCA tool to automatically identify vulnerable dependencies before release, mitigating potential security risks.
* **Manage dependencies with package managers:** Leverages package managers for dependency management to ensure consistency and control.
* **Secure dependency retrieval:** Enforces the use of secure channels (e.g., TLS, SSH) for fetching all dependencies.
* **Prioritize trusted repositories:** Gives preference to trusted, ecosystem-specific primary registries when selecting dependencies.

### Secure the build

* **Automatically build trusted sources only**: Implement safeguards to ensure that only trusted sources are used in the build process. This could involve requiring committer approval for pull requests before builds or employing protective measures to prevent unauthorized modifications of build pipelines and scripts.

## Level 3: Selenic 🌖

*Selenic* signifies an advanced level of security, introducing sophisticated practices that provide substantial protection against vulnerabilities and tampering. This level focuses on enforcing strict code repository management, enhancing source code security, rigorous dependency management, and securing the build and deployment processes. The objective is to fortify the software supply chain against more sophisticated threats and ensure the integrity and security of the software development lifecycle.

### Secure the source code repositories

* **No direct push to main branches:** enforces mandatory pull request before merging code into main branches (no direct pushes allowed).
* **Mandatory commit signing:** enforces digital signing for all commits to source code repositories.
* **Automatic protection against secret exposure:** Implements a process to prevent the accidental exposure of secrets and credentials in the source code repository.
* **Branch and tag protection**: implements controls to prevent accidental or unauthorized deletion of branches and tags in source code repositories.
  
### Secure the source code

* **Automatic policy to prevent contributions and releases from containing known vulnerabilities**, strict policy to prevent contributions with vulnerabilities from being merged and for releases to be made with vulnerabilities as identified by SAST tools.
* **Security policy and vulnerability management:** establishes a comprehensive security policy with a clear Service Level Objective (SLO) for reported vulnerability management.
* **Restriction on Force-Push (all development branches):** Prohibits the use of force-push to repositories, preventing the risk of overwriting history and losing code.

### Secure the dependencies

* **Vulnerability scanning in contributions:** Integrates a Software Composition Analysis (SCA) tool into the contribution workflow to automatically identify and vet dependencies for vulnerabilities.
* **Software Bill of Materials (SBOM) generation:** generates a Software Bill of Materials (SBOM) for every software release, providing a complete list of third party dependencies and their versions.
* **Dependency Verification:** implements automated processes to verify checksums and signatures of all downloaded dependencies.

### Secure the build

* **Ephemeral Build Environments:** utilizes ephemeral agents (containers or VMs) for all CI/CD builds to minimize attack surface.
* **Secret Management:** ensures all CI/CD pipeline secrets are known only to the Eclipse Foundation and can be rotated for enhanced security.
* **Webhook Security:** enforces authentication and secured transport channel (e.g., TLS) for webhooks used in integrations and notifications.
* **Build Provenance:** generates and signs build provenance information to track the complete software build history and ensure reproducibility.

### Secure the deployment

* **Release management automation:** leverages infrastructure as code for continuous delivery and release pipelines.
* **Digitally sign all released artifacts:** signs all released artifacts (e.g., packages, installers) to ensure authenticity and integrity.
* **Vulnerability disclosure process:** maintains a mature and coordinated vulnerability disclosure process. Publishes security advisories for identified vulnerabilities.

## Level 4: Stellar ⭐️

*Stellar* reflects a near-comprehensive suite of security measures, complemented by continuous improvement processes. At this level, emphasis is placed on securing the development environment, implementing peer review processes for all code commits, and further hardening CI environments. The goal is to achieve a high degree of security maturity, ensuring that developers, code, and build processes are not only protected against current threats but are also resilient to future vulnerabilities.

### Secure the developers

* **Developers are well-versed in best practices:** safeguards their development environments, ensuring that they are equipped with the knowledge and tools to prevent unauthorized access and potential vulnerabilities within their local workspace.

### Secure the source code repositories

* **Access to source code repositories is tightly controlled:** uses fine-grained, short-lived tokens. This minimizes the risk of unauthorized access and potential data breaches by ensuring that access rights are given only for the necessary duration and with the appropriate level of permissions.

### Secure the code

* **All commits to main branches are subjected to peer review by another committer:** ensures that changes are scrutinized for security vulnerabilities, coding standards, and overall quality before being integrated into the main codebase.

### Secure the build

* **CI environments are reinforced on hardened agents:** builds run on isolated agent to prevent unauthorized access and ensure a secure build process, e.g., by using additional security layer like [Harden-Runner](https://github.com/step-security/harden-runner).
* **Build provenance is generated by the build service:** establishes a verifiable record of the build process, from source to deployment. This transparency helps in tracking the integrity and origin of the code.
* **SBOM with build time information:** the SBOM for releases includes all tools, applications, and services utilized during the build process, offering a complete inventory for enhanced traceability and vulnerability management.

### Secure the Dependencies

* **Pin dependencies:** specifies exact versions to be used, thus avoiding the automatic update to potentially vulnerable versions and ensuring consistency across environments. Applicable to both code dependencies and to build dependencies.
* **Use a proxified package manager for managing dependencies:** proxy acts as a firewall for malicious and vulnerable dependenices and records all incoming dependencies. This setup adds a layer of security by monitoring and controlling the flow of external code into the project.

### Secure the Consumption

* **Continuously monitor SBOMs for past supported releases:** actively scans SBOMs for past releases, organizations can identify and address vulnerabilities in components that are in active use, thereby securing the software supply chain against emerging threats.

## Level 5: Galactic 🌌

*Galactic* introduces state-of-the-art security practices with cutting-edge enhancements and proactive threat mitigation. This level aims to push the boundaries of security practices by requiring dual committer reviews, implementing mature vulnerability disclosure processes, and adopting fuzz testing, among others. The focus is on not just defending against known threats but also anticipating and mitigating potential future vulnerabilities, ensuring the highest level of security preparedness.

### Secure the Code

* **All commits to main branches must be reviewed by two committers:** ensures that code changes are examined for security and quality by multiple eyes before being integrated.
* **Have a mature++ coordinated vulnerability disclosure process:**
  * **Has an embargo list (if any):** ensures sensitive information is only shared with those who need to know until a public announcement is made.
  * **Private patch development:** allows for secure fixing of vulnerabilities without exposing them to potential attackers.
  * **Publish advisories announcement:** ensures that users and stakeholders are informed about vulnerabilities and patches in a timely and clear manner.
* **Has fuzz testing:** involves automated testing techniques that feed unexpected or random data to the program to find potential security vulnerabilities or bugs.

### Secure the Build

* **Reproducible builds:** ensures that the build process can be independently verified by producing byte-for-byte identical outputs from the same source code.
* **Release build is non-falsifiable:** ensures the integrity and authenticity of the build process, so that the final product can be trusted.
* **Released artifacts are published to an immutable archive alongside the source code:** secures the build output by making sure that once an artifact is published, it cannot be tampered with or altered.
* **Release process is not under the control of the project:** decentralizes the release process, adds a layer of security against unauthorized or malicious changes.

### Secure the Dependencies

* **Proactive protection from malicious dependencies:** ensures that malicious (e.g., typo-squatted) dependencies are searched for, in addition to vulnerabilities. It protects the project from integrating potentially harmful external code.

### Secure the Consumption

* **Has an SLO for Mean Time To Remediate (MTTR) security alerts:** establishes a SLO for addressing security vulnerabilities (vulnerable dependencies, secret leak, etc.) to ensure a rapid response to threats.

## Level 6: Event Horizon ♾️

At the *Event Horizon* level, projects engage in practices that are above and beyond the established standards, exploring new frontiers in software supply chain security. This includes implementing hermetic builds and indefinitely retaining build environments for releases, among other pioneering practices. The goal is to create an impenetrable security posture that not only addresses current security challenges but also sets new benchmarks for the future of secure software development.

### Secure the build

* **Hermetic build:** ensures that the build does not rely on any external systems, tools, libraries, or runtime environments outside of those specifically included and version-controlled within the build process itself. May go up to air-gapped environment.
* **Build environments for releases are retained indefinitely:** This ensures that binaries can be rebuilt at any time, even long after the project has been terminated.

## Summary

| Security Level | Description | Practices |
|---|---|---|
| L1 Launchpad 🚀 | Entry-level security measures |  * Secure source code repositories (basic) <br/>* Secure the source code (basic) <br/>* Secure the dependencies (basic) <br/>* Secure the build (basic) |
| L2 Orbit ️🛰️ | Basic security measures consistently applied |  * Strong access control for repositories <br/>* Secure the source code (enhanced) <br/>* Manage dependencies with package managers <br/>* Secure dependency retrieval <br/>* Prioritize trusted repositories <br/>* Vulnerability scanning in contributions <br/>* Secure the build (automated) |
| L3 Selenic 🌖 | Advanced security practices |  * Secure source code repositories (advanced) <br/>* Automatic policy for secure code <br/>* Security policy and vulnerability management <br/>* SBOM generation <br/>* Dependency Verification |
| L4 Stellar ⭐️ | Near-comprehensive security with continuous improvement |  * Secure the developers <br/>* Tightly controlled access to repositories <br/>* Peer review for main branch commits <br/>* Hardened CI environments <br/>* Build provenance by build service <br/>* SBOM with build time information <br/>* Pin dependencies <br/>* Proxified package manager |
| L5 Galactic 🌌 | State-of-the-art security practices |  * Two-committer review for main branch <br/>* Mature vulnerability disclosure process <br/>* Fuzz testing <br/>* Reproducible builds <br/>* Non-falsifiable release builds <br/>* Immutable archive for artifacts <br/>* Decentralized release process <br/>* Proactive protection from malicious dependencies <br/>* MTTR SLO for security alerts |
| L6 Event Horizon ♾️ | Beyond established practices |  * Hermetic builds <br/>* Indefinite retention of release build environments |
