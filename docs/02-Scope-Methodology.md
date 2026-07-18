# 2. Assessment Scope & Investigation Methodology

*  **Document Type:** Terms of Reference (ToR)
*  **Assessment Period:** February 6, 2026
*  **Assessment Owner:** Niladri Biswas

## 2.1 Purpose
To implement and validate a continuous access control monitoring control using Splunk SIEM. The objective is to detect brute-force credential guessing attacks (MITRE ATT&CK T1110), operationalize a real-time alert, and map the entire workflow to ISO 27001 and NIST CSF compliance requirements.

## 2.2 In-Scope & Out-of-Scope Assets
| **Scope Element** | **Details** |
|:---|:---|
| **SIEM Platform** | Splunk Enterprise 10.2.0 (Hosted on Kali Linux VM at `10.232.194.7`). |
| **Log Source** | Windows 10 Build 19045 (Host IP `10.232.194.141`) via Splunk Universal Forwarder. |
| **Event Type** | Windows Security Event ID 4625 (Failed logon attempts). |
| **Attack Simulation** | Authenticated user attempting to run `runas` with incorrect passwords against local accounts. |
| **Out-of-Scope** | Cloud identity providers (Azure AD), Linux SSH logs, and Syslog integration. |

## 2.3 Technical Methodology (6 Phases)

**Phase 1 — SIEM Platform Deployment & Network Config**
Splunk Enterprise was deployed on Kali Linux within a VirtualBox Bridged network (`10.232.194.0/24`). Port 9997 was configured on the SIEM to receive log data from external forwarders.

**Phase 2 — Forwarder Installation & Log Ingestion**
The Splunk Universal Forwarder was installed on the Windows 10 target. Inputs were configured to monitor the Windows Security Event Log (`[WinEventLog://Security]`). Windows native audit policy (`auditpol`) was configured to log both Success and Failure logon attempts.

**Phase 3 — Attack Simulation**
A test user (`testuser`) was created. Multiple failed login attempts were generated against `testuser` and a local privileged user (`Dr.Nee`) using the `runas` command with incorrect passwords. This generated authentic Event ID 4625 entries.

**Phase 4 — SPL Detection Development**
Search Processing Language (SPL) queries were developed to identify accounts exceeding a defined threshold of failed login attempts:
*   `EventCode=4625`
*   `stats count by Account_Name`
*   `where count >= 3`

**Phase 5 — Real-Time Alert Configuration**
The detection query was operationalized as a real-time Splunk alert. The alert is configured to trigger **per-result** and log a dynamic text string: *"Brute Force Alert: User $result.Account_Name$ has $result.count$ failed login attempts."*

**Phase 6 — GRC Documentation**
The entire setup and the resulting findings were documented, mapped to ISO 27001 Annex A controls, NIST CSF functions, and MITRE ATT&CK TTPs.

## 2.4 Constraints & Limitations
*   **Detection Only:** This implementation is strictly a **detective control**. It detects the attack after it has already occurred. Without account lockout or MFA, it does not prevent the attacker from guessing the password.
*   **Limited Scope:** The monitoring currently covers only Windows Local Authentication. It does not monitor SSH, VPN, or Cloud Authentication.
