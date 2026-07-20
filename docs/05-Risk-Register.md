# 5. Risk Register

This register serves as the master document for tracking the identified threat and the underlying control gaps in the Windows authentication infrastructure.

## Risk Scoring Context
*   **Asset:** Windows 10 Active Directory/Local Authentication infrastructure.
*   **Threat:** External or internal adversary utilizing brute-force password guessing to gain initial access.
*   **Vulnerability:** Absent account lockout policy and lack of MFA on privileged accounts, combined with a detection-only SIEM setup.

## Risk Register Entries
| Priority | Vulnerability / Threat | Likelihood | Business Impact | Risk Score | Recommended Owner |
|:---:|:---|:---|:---|:---|:---|
| **P1** | **Absent Account Lockout Policy (Preventive Control Gap)** | Very High (Brute-force is a common, low-skill attack technique) | **Critical:** Privileged account takeover (`Dr.Nee`) could lead to full system compromise, data exfiltration, or ransomware deployment. | Critical | Security Engineer |
| **P2** | **SIEM Detection Without Automated Response** | High (Real-time alert triggers but requires human triage) | **Medium:** Delay between detection and incident response allows attackers additional attempts. | High | SOC Lead |
| **P2** | **Absent Multi-Factor Authentication (MFA)** | Medium (Attackers need valid credentials to bypass MFA) | **High:** Single-factor authentication makes privileged accounts highly vulnerable. | High | IAM Administrator |
