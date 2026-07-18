# Security Control Monitoring & Risk Assessment | Splunk SIEM

![Splunk](https://img.shields.io/badge/Splunk-Enterprise%2010.2.0-65A637?style=for-the-badge&logo=splunk&logoColor=white)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE-ATT%26CK%20T1110-red?style=for-the-badge&logo=mitre&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-28A745?style=for-the-badge)
![Framework](https://img.shields.io/badge/Framework-ISO%2027001%20%7C%20NIST%20CSF-0057A8?style=for-the-badge)
![Control](https://img.shields.io/badge/Control%20Area-Access%20Control%20Monitoring-blueviolet?style=for-the-badge)

**A structured access control monitoring lab using Splunk SIEM to detect brute-force authentication activity.** 
This project establishes a continuous compliance oversight framework—from deploying Splunk Enterprise and ingesting Windows Security Event Logs (EventCode 4625), to developing SPL detection logic, configuring real-time threshold-based alerting, and producing an audit-grade incident report mapped to ISO 27001, NIST CSF, and MITRE ATT&CK.

---

## Project at a Glance (For Recruiters & Hiring Managers)

| Area | Details |
|:---|:---|
| **Threat Detected** | Brute Force Authentication Attack (T1110.001 - Password Guessing) |
| **Detection Method** | Splunk SPL query searching for 3+ failed logon events (EventCode 4625) per account. |
| **Lab Environment** | Splunk Enterprise (Kali) ↔ Windows 10 (UF) on Bridged Network. |
| **Key Finding** | 8 failed attempts (4 per account) detected. Real-time alert operationalized. |
| **Framework Mapped** | ISO 27001 (A.8.15, A.8.16, A.5.17), NIST CSF (DE.CM-1, PR.AC-7). |

---

## Repository Contents

Navigate this repository to view the formal GRC artifacts:

| Document | Description |
|:---|:---|
| **`docs/01-Executive-Summary.md`** | A non-technical summary for senior leadership detailing the SIEM implementation and the successful detection of a brute-force attempt. |
| **`docs/02-Executive-Dashboard.md`** | A management-level KPI dashboard tracking monitoring status, control posture, and strategic recommendations. |
| **`docs/03-Scope-Methodology.md`** | The formal Terms of Reference for the SIEM lab, covering network topology, log ingestion, and SPL development. |
| **`docs/04-Assumed-Business-Context.md`** | Contextualizing the organization as a financial services firm to drive accurate risk scoring. |
| **`docs/05-Risk-Register.md`** | A formalized risk register entry for the Credential Access vulnerability, scoring the gap between detective and preventive controls. |
| **`docs/06-Remediation-Tracker.md`** | A project management artifact tracking the implementation of MFA and Account Lockouts, complete with Cost-Benefit Analysis. |
| **`docs/07-Compliance-Gap-Analysis.md`** | A control-by-control analysis mapping the monitoring setup to ISO 27001 Annex A, NIST CSF, and PCI DSS. |
| **`docs/08-Asset-Business-Criticality.md`** | Contextualizing the target Windows endpoint as a sensitive Finance/HR server. |
| **`docs/09-MITRE-ATTACK-Mapping.md`** | Detailed mapping of the attack to T1110.001 and the SIEM's detection logic. |
| **`docs/10-Residual-Risk-Assessment.md`** | A post-remediation analysis evaluating the remaining risk after controls are implemented. |
| **`docs/11-Lessons-Learned.md`** | A post-implementation review (PIR) documenting successes, challenges, and next iterations. |
| **`artifacts/`** | Raw technical documentation, including the complete SPL `detection_query.md` and the `setup_guide.md` for replicating the lab. |
| **`screenshots/`** | 18 annotated screenshots documenting the installation, log ingestion, and alert configuration. |

---
