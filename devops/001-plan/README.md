# Plan

## Threat Modelling

### STRIDE

| Threat Type | Description | Examples |
|--------------|--------------|-----------|
| **Spoofing** | Impersonating another identity to gain unauthorised access | Using stolen credentials to log in, forging JWT tokens, spoofing IP/MAC addresses, faking OAuth tokens |
| **Tampering** | Modifying data or code without permission | Altering parameters in API requests, injecting malicious JavaScript, modifying configuration files, changing firmware |
| **Repudiation** | Denying actions to avoid accountability | Deleting or altering audit logs, sending unauthenticated API calls, bypassing transaction records |
| **Information Disclosure** | Exposing confidential data to unauthorised entities | Returning stack traces in production, exposing S3 buckets, leaking PII in error messages, insecure backup downloads |
| **Denial of Service (DoS)** | Disrupting system availability or resources | Flooding API endpoints, locking shared resources, exhausting file descriptors, recursive function abuse |
| **Elevation of Privilege** | Gaining higher permissions than intended | Exploiting misconfigured IAM roles, bypassing RBAC checks, privilege escalation via injection or kernel bug |

### DREAD

| Factor | Description | Scoring Question | Examples |
|---------|--------------|------------------|-----------|
| **Damage Potential** | Measures the impact if the vulnerability is exploited | How severe would the impact be if exploited? | Full system compromise, customer data breach, loss of availability, financial fraud |
| **Reproducibility** | Indicates how easily the attack can be reproduced | Can anyone repeat the exploit easily? | Public PoC exploit available, deterministic race condition, complex chained attack |
| **Exploitability** | Assesses how much effort or resources are required to perform the attack | How hard is it to perform the attack? | Simple script, requires physical access, social engineering, insider knowledge |
| **Affected Users** | Determines the proportion of users impacted | How many users would be affected? | One tenant, subset of accounts, all customers globally |
| **Discoverability** | Evaluates how easily the vulnerability can be identified | How likely is the issue to be found? | Visible in error message, found through fuzzing, deep code inspection required |

### PASTA

| Stage Name | Focus | Output | Examples |
|-------------|--------|---------|-----------|
| **Definition of Objectives** | Define business impact and security goals | Business impact statement | Identify critical assets (payment gateway, user data), define acceptable risk levels |
| **Definition of the Technical Scope** | Identify applications, APIs, data flows, and components | Architecture diagrams and data flow maps | Inventory of microservices, third-party APIs, cloud services, network zones |
| **Application Decomposition & Analysis** | Break down the system into smaller components | Detailed component inventory | Decompose authentication flow, identify input points, map data storage paths |
| **Threat Analysis** | Identify potential threat agents and vectors | Threat catalogue | Map OWASP Top-10 to architecture, consider insiders, external attackers, automated bots |
| **Vulnerability & Weakness Analysis** | Assess security controls and known weaknesses | Vulnerability mapping | Perform code reviews, run SAST/DAST, assess patch levels, review misconfigurations |
| **Attack Modelling** | Simulate realistic attack scenarios | Attack trees and sequence diagrams | Model SQLi chain via API, credential-stuffing flow, privilege escalation path |
| **Risk & Impact Analysis** | Evaluate risk levels and prioritise mitigations | Risk matrix and mitigation plan | Prioritise threats by likelihood × impact, propose compensating controls, mitigation roadmap |

### Further reading

* [Comparison of STRIDE, DREAD & PASTA][1]

[1]: https://www.softwaresecured.com/post/comparison-of-stride-dread-pasta
