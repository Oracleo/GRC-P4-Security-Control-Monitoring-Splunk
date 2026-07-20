# 10. Residual Risk Assessment

Once a GRC analyst identifies vulnerabilities and recommends controls, the ultimate goal is to reduce risk to an **acceptable residual level** (within the organization's risk appetite). This document assesses the remaining risk *after* the recommended controls from `06-Remediation-Tracker.md` have been implemented.

## 10.1 Risk Mitigation Status

| Original Finding | Mitigating Control Applied | Residual Risk Status |
|:---|:---|:---:|
| **No Account Lockout** | Configured Windows Group Policy: 5 invalid attempts within 15 minutes results in a 30-minute lockout. | 🟢 **Low** |
| **No MFA on Privileged Accounts** | Enforced MFA requirement via Duo/Microsoft Authenticator for all local admin accounts (including `Dr.Nee`). | 🟢 **Low** |
| **Alert not routed to SOC** | Splunk alert integrated with Microsoft Teams/Email notifications to ensure the SOC is notified within 5 minutes of an alert firing. | 🟡 **Medium** |
| **Limited Monitoring Scope** | Monitoring expanded from Windows Security logs to include Linux `auth.log` and Azure AD sign-in logs. | 🟡 **Medium** |

## 10.2 Evaluation of Residual Risks

Despite implementing the preventative controls above, the following residual risks remain and are accepted by management:

**1. Insider Threat and Credential Theft (Risk Level: Low)**
*   **Context:** Even with MFA, a malicious insider or a socially-engineered user could still generate a valid MFA prompt to give the attacker access.
*   **Acceptance:** MFA is a major barrier to external attacks. This residual risk is accepted pending implementation of User Behavior Analytics (UEBA) tools in Q4.

**2. Zero-Day Vendor Vulnerabilities (Risk Level: Low)**
*   **Context:** A zero-day vulnerability in the Splunk SIEM software itself could disable our monitoring capability.
*   **Acceptance:** Vendor patching schedules and mandatory updates are implemented. This risk is deemed acceptable.

**3. Full Coverage Blind Spots (Risk Level: Medium)**
*   **Context:** While we expanded logging to Linux and Azure AD, the SIEM currently does not ingest logs from our physical network firewalls and switches (only network flows).
*   **Acceptance:** This gap is noted as a backlogged project. Monitoring is currently "Medium" risk because a network attack could bypass endpoint detection. This is temporarily accepted while awaiting budget approval for full network logging.

## 10.3 Formal Risk Acceptance

The organization's risk appetite dictates that we will accept **Medium** residual risk. 

| Scenario | Residual Risk Level | Sign-Off Authority |
|:---|:---|:---|
| **Unmonitored physical network traffic** | 🟡 Medium | CISO / Infrastructure Head |

**Management Exceptions:** The lack of network firewall log ingestion is accepted as a temporary 6-month exception, provided the endpoint security (Splunk UF on Windows) remains operational.
