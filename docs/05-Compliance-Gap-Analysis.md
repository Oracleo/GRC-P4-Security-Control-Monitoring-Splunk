# 5. Compliance Gap Analysis (ISO 27001, NIST CSF, PCI DSS)

This document maps the identified security control implementation to specific regulatory frameworks.

## ISO 27001:2022 Annex A Mappings
| Control ID | Control Name | Gap Evidence | Compliance Status |
|:---:|:---|:---|:---:|
| **A.8.16** | Monitoring Activities | Splunk SIEM successfully ingests Windows Security logs and alerts on brute-force attempts. | ✅ **Compliant** |
| **A.8.15** | Logging | Windows Security Events (EventID 4625) are forwarded to Splunk and indexed. | ✅ **Compliant** |
| **A.5.17** | Authentication Information | Lack of MFA on privileged accounts leaves authentication information vulnerable to dictionary attacks. | 🟡 **Control Gap** |
| **A.8.5** | Secure Authentication | No account lockout policy is configured, violating standard authentication hardening principles. | 🔴 **Non-Compliant** |

## NIST Cybersecurity Framework (CSF) Mappings
| NIST Function | NIST Category | Gap Evidence |
|:---|:---|:---|
| **IDENTIFY (ID)** | ID.RA-1 (Asset Vulnerabilities) | Identified that authentication mechanisms lack lockout protection. |
| **PROTECT (PR)** | PR.AC-7 (Authentication) | The brute-force simulation succeeded in bypassing the authentication control because MFA and Lockout are absent. |
| **DETECT (DE)** | DE.CM-1 (Network is monitored) | Splunk logs are flowing correctly; the detection SPL validates the monitoring of network/access events. |
| **RESPOND (RS)** | RS.AN-1 (Notifications from Detection Systems are investigated) | The alert has been created, but it is not yet routed to a ticketing system or an analyst. |

## PCI DSS v4.0 Mappings
*   **Requirement 10.2:** Audit trails for system components must be enabled and active. (Passed: Windows Security logs are forwarding).
*   **Requirement 10.6:** Review logs and security events. (Passed: Splunk is actively searching).
*   **Requirement 8.5:** The use of group, shared, or generic IDs must be prevented. (Not addressed: Accounts were tested individually, but the project does not test for shared account usage).

## GDPR (General Data Protection Regulation) Implications
If an employee's account is brute-forced successfully, it could lead to a data breach. Failure to implement MFA and account lockouts constitutes a failure of **Article 32 (Security of Processing)**, requiring the organization to implement appropriate technical measures to protect personal data.
