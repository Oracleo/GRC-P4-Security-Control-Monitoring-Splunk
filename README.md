# Security Control Monitoring & Risk Documentation | Splunk SIEM

![Splunk](https://img.shields.io/badge/Splunk-Enterprise%2010.2.0-65A637?style=for-the-badge&logo=splunk&logoColor=white)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE-ATT%26CK%20T1110-red?style=for-the-badge&logo=mitre&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-28A745?style=for-the-badge)
![Framework](https://img.shields.io/badge/Framework-ISO%2027001%20%7C%20NIST%20CSF-0057A8?style=for-the-badge)
![Control](https://img.shields.io/badge/Control%20Area-Access%20Control%20Monitoring-blueviolet?style=for-the-badge)

**A structured access control monitoring lab using Splunk SIEM to detect brute-force authentication activity via Windows Security Event Logs — establishing continuous compliance oversight, threshold-based alerting, and an incident report mapped to ISO 27001, NIST CSF, and MITRE ATT&CK.**

---

## 📋 Table of Contents

1. [GRC Relevance — Why This Project](#-grc-relevance--why-this-project)
2. [Incident Summary](#-incident-summary)
3. [Lab Architecture](#-lab-architecture)
4. [Tools & Environment](#-tools--environment)
5. [Implementation — Step by Step](#-implementation--step-by-step)
6. [Detection Findings](#-detection-findings)
7. [MITRE ATT&CK Mapping](#-mitre-attck-mapping)
8. [Risk Register Entry & Control Recommendations](#-risk-register-entry--control-recommendations)
9. [GRC Concepts Applied](#-grc-concepts-applied)
10. [Screenshots Index](#-screenshots-index)
11. [Repository Structure](#-repository-structure)

---

## 🎯 GRC Relevance — Why This Project

Security control monitoring is a continuous GRC obligation, not a one-time activity. Access control monitoring specifically — detecting and responding to authentication anomalies — is mandated under nearly every major compliance framework. A GRC analyst evaluating or maintaining a control monitoring programme is expected to:

- Verify that logging and monitoring controls are actually operational, not just documented
- Confirm that detection thresholds align with organisational risk appetite
- Document the evidence trail from raw log to alert to incident response
- Map detective controls to the specific framework clauses they satisfy
- Assess whether monitoring coverage is sufficient to support audit evidence requests
- Produce incident documentation suitable for both technical teams and compliance review

This project simulates that control monitoring lifecycle end-to-end. **Splunk Enterprise** was deployed as the SIEM platform, ingesting **Windows Security Event Logs** via Splunk Universal Forwarder, to detect brute-force authentication attempts against two test accounts.

**Outcome: Real-time detection of 8 failed login attempts (4 per account) across two user accounts within 60 seconds. Threshold-based alerting configured and verified operational. Incident documented with MITRE ATT&CK mapping and control effectiveness assessment.**

> 💡 **GRC Context:** ISO 27001 Annex A.8.16 (Monitoring activities) requires networks, systems, and applications to be monitored for anomalous behaviour. A.8.15 (Logging) requires logs recording user activities and security events to be produced, stored, and reviewed. NIST CSF DE.CM-1 (Networks are monitored) and DE.CM-3 (Personnel activity is monitored) both require exactly this type of detective control. This project demonstrates the technical implementation and the audit evidence trail those controls require.

---

## 📊 Incident Summary

| Field | Detail |
|---|---|
| **Control Tested** | Access control monitoring — authentication anomaly detection |
| **Activity Type** | Brute-Force Authentication Attempt |
| **Target Accounts** | Dr.Nee (privileged), testuser (standard) |
| **Detection Time** | Real-time — under 60 seconds |
| **Failed Attempts** | 8 total (4 per account) |
| **Outcome** | ✅ Detected and alerted — no unauthorised access achieved |
| **MITRE ATT&CK** | T1110 — Brute Force / TA0006 — Credential Access |
| **ISO 27001 Controls** | A.8.16, A.8.15, A.5.17, A.8.5 |
| **NIST CSF** | DE.CM-1, DE.CM-3, PR.AC-7 |

---

## 🏗️ Lab Architecture

```
┌──────────────────────────────────────────────────────────────────┐
│                    Bridged Network — 10.232.194.0/24             │
│                                                                   │
│  ┌─────────────────────────────┐   ┌──────────────────────────┐ │
│  │   Kali Linux VM              │   │   Windows 10 Host        │ │
│  │   (SIEM Platform)            │◄──┤   (Target / Log Source)  │ │
│  │   10.232.194.7                │   │   10.232.194.141          │ │
│  │                               │   │                          │ │
│  │   Splunk Enterprise 10.2.0    │   │   Splunk Universal       │ │
│  │   Web UI: Port 8000           │   │   Forwarder 10.2.0       │ │
│  │   Receiving: Port 9997        │   │                          │ │
│  └─────────────────────────────┘   └──────────────────────────┘ │
│                                                                   │
│   Data Flow: Windows Security Event Log (EventID 4625)          │
│              → Universal Forwarder → TCP 9997 → Splunk Index    │
│              → SPL Detection Query → Real-time Alert             │
└──────────────────────────────────────────────────────────────────┘
```

| Component | Details |
|---|---|
| SIEM Platform | Kali Linux VM — 10.232.194.7 |
| Target / Log Source | Windows 10 Build 19045 — 10.232.194.141 |
| Network Mode | Bridged Adapter |
| SIEM Software | Splunk Enterprise 10.2.0 |
| Log Forwarding | Splunk Universal Forwarder 10.2.0 |
| Forwarding Port | TCP 9997 |
| Web Interface Port | 8000 |

---

## 🧰 Tools & Environment

| Tool | Version | Purpose |
|---|---|---|
| [Splunk Enterprise](https://www.splunk.com/) | 10.2.0 | SIEM platform — log aggregation, correlation, alerting |
| Splunk Universal Forwarder | 10.2.0 | Lightweight log collection and forwarding agent |
| Windows Event Viewer | Built-in | Security Event Log source — EventID 4625 |
| Kali Linux | Latest | SIEM hosting platform |
| Windows 10 | Build 19045 | Monitored endpoint and attack simulation target |
| VirtualBox | Latest | Virtualisation — bridged networking |

---

## 🔧 Implementation — Step by Step

### Phase 1 — SIEM Platform Deployment

Splunk Enterprise was deployed on Kali Linux as the central monitoring platform.

![Splunk Startup Terminal](screenshots/GRC4-01-splunk-start-terminal.png)

All prerequisite checks completed — port availability (8000, 8089, 8065, 8191), configuration validation, index integrity. Web interface confirmed available at `http://kali:8000`.

![Splunk Login Page](screenshots/GRC4-02-splunk-login-page.png)

![Splunk Home Dashboard](screenshots/GRC4-03-splunk-home-dashboard.png)

---

### Phase 2 — Network Configuration & Connectivity Verification

Kali Linux assigned IP `10.232.194.7` on bridged adapter:

![Kali Network Configuration](screenshots/GRC4-04-kali-network-ip.png)

Connectivity confirmed from Windows host — 0% packet loss, 1–2ms latency:

![Windows to Kali Connectivity](screenshots/GRC4-05-windows-ping-kali.png)

> **GRC Note:** Verifying connectivity before relying on a monitoring control is itself a control validation step — an auditor reviewing this monitoring programme would expect evidence that the log pipeline is confirmed operational, not assumed.

---

### Phase 3 — Log Collection Configuration

Splunk's receiving configuration enabled on port 9997 to accept Universal Forwarder traffic:

![Data Inputs Page](screenshots/GRC4-06-data-inputs-page.png)

![Receiving Port 9997](screenshots/GRC4-07-receiving-port-9997.png)

Universal Forwarder installed on the Windows host and pointed to the Kali SIEM instance. Log ingestion verified:

```spl
index=main sourcetype=WinEventLog:Security
```

![Windows Logs in Splunk](screenshots/GRC4-08-windows-logs-in-splunk.png)

**978 events** confirmed indexed — the log forwarding pipeline is operational and producing the audit trail required for control evidence.

---

### Phase 4 — Windows Audit Policy Configuration

Windows audit policy configured to capture both successful and failed authentication events:

```powershell
auditpol /set /subcategory:"Logon" /success:enable /failure:enable
```

> **GRC Note:** This single configuration step is a control in itself. ISO 27001 A.8.15 requires logging of security-relevant events — if audit policy is not explicitly enabled for both success and failure, an organisation cannot demonstrate this control is operating effectively. Many audit findings stem from exactly this gap.

---

### Phase 5 — Authentication Anomaly Simulation

A test account was created to generate controlled authentication activity for monitoring validation:

```cmd
net user testuser Test@123 /add
```

![Test User Created](screenshots/GRC4-09-test-user-created.png)

Failed authentication attempts generated against both accounts using incorrect credentials:

![Failed Login Attempts](screenshots/GRC4-10-failed-login-attempts.png)

Each attempt generated Windows error 1326 ("The user name or password is incorrect"), producing authentic EventID 4625 entries forwarded to Splunk in real time.

---

### Phase 6 — Detection Query Development

A detection query was developed in Search Processing Language (SPL) to identify accounts exceeding a failed-authentication threshold — the core logic of the access control monitoring control.

```spl
index=main sourcetype=WinEventLog:Security EventCode=4625
| stats count by Account_Name
| where count >= 3
| sort -count
```

**Query logic — control design rationale:**

| Step | Purpose | GRC Relevance |
|---|---|---|
| Filter EventCode=4625 | Isolate failed logon events | Scopes the control to the specific risk indicator |
| Aggregate by Account_Name | Identify per-account failure patterns | Enables per-asset risk assessment |
| Threshold `count >= 3` | Reduce false positives while catching genuine attacks | Balances detection sensitivity — a documented risk-based decision |
| Sort descending | Prioritise highest-risk accounts first | Supports triage prioritisation |

---

### Phase 7 — Real-Time Alert Configuration

A real-time alert was configured to operationalise the detection query — converting a manual search into a continuous monitoring control.

![Create Alert Dialog](screenshots/GRC4-14-create-alert-dialog.png)

**Alert configuration:**

| Setting | Value |
|---|---|
| Alert Name | Brute Force Detection — Failed Login Attempts |
| Alert Type | Real-time |
| Trigger Condition | Per-Result |
| Action | Log Event |

![Alert Trigger Configuration](screenshots/GRC4-15-alert-trigger-config.png)

Dynamic field substitution configured for alert clarity:
```
Brute Force Alert: User $result.Account_Name$ has $result.count$ failed login attempts
```

![Alert Saved Confirmation](screenshots/GRC4-16-alert-saved-confirmation.png)

![Alert Status Dashboard](screenshots/GRC4-17-alert-dashboard.png)

Alert confirmed enabled, Real-time type, Per-Result trigger, one action configured — control verified operational and ready for ongoing monitoring.

---

## 📊 Detection Findings

### Failed Logon Event Search

![Event ID 4625 Search](screenshots/GRC4-11-event-4625-search.png)

Search for EventCode 4625 returned 4 events within the monitored window, with a clear concentrated spike in the timeline visualization corresponding to the simulated attack period.

### Event Detail Analysis

![Event Details Expanded](screenshots/GRC4-12-event-details-expanded.png)

| Field | Value |
|---|---|
| Account Name | testuser |
| Failure Reason | Unknown user name or bad password |
| Logon Type | 2 — Interactive |
| Computer Name | NATIONALIST |

### Detection Query Results

![Detection Query Results](screenshots/GRC4-13-detection-query-results.png)

| Account | Failed Attempts | Threshold Status | Classification |
|---|---|---|---|
| Dr.Nee (privileged) | 4 | Above threshold (≥3) | Potential brute-force activity |
| testuser (standard) | 4 | Above threshold (≥3) | Potential brute-force activity |

### Visual Detection Analysis

![Detection Visualization](screenshots/GRC4-13-1-detection-visualization.png)

Column chart confirming both accounts at equal failure counts of 4 — immediate visual evidence supporting the control's effectiveness in surfacing the anomaly.

---

## 🎯 MITRE ATT&CK Mapping

| Field | Detail |
|---|---|
| **Tactic** | Credential Access — TA0006 |
| **Technique** | Brute Force — T1110 |
| **Sub-technique** | Password Guessing — T1110.001 |
| **Reference** | https://attack.mitre.org/techniques/T1110/ |

**Detection Method:** Monitor authentication logs for multiple failed login attempts, particularly those occurring in rapid succession or targeting multiple accounts. Threshold-based alerting on accounts exceeding normal failed authentication baselines is the primary detective control for this technique.

**MITRE Mitigations Applicable:**

| ID | Mitigation |
|---|---|
| M1032 | Multi-factor Authentication |
| M1027 | Password Policies |
| M1018 | User Account Management — account lockout |
| M1056 | Pre-compromise — limit exposure of account names |

---

## 📋 Risk Register Entry & Control Recommendations

### Risk Register Entry

| Field | Value |
|---|---|
| **Risk ID** | RISK-AC-002 |
| **Asset** | Windows authentication infrastructure / privileged and standard accounts |
| **Threat** | Brute-force credential guessing — internal or external actor |
| **Vulnerability** | No account lockout policy observed; detection is reactive, not preventive |
| **Likelihood** | Medium — brute force is a common, low-skill attack technique |
| **Impact** | High if a privileged account (e.g. Dr.Nee) is compromised |
| **Inherent Risk** | High |
| **Current Controls** | Splunk SIEM real-time detection and alerting — confirmed operational |
| **Control Gap** | Detection without automated lockout; no MFA observed on tested accounts |
| **Residual Risk** | Medium |
| **ISO 27001** | A.8.16, A.8.15, A.5.17, A.8.5 |
| **NIST CSF** | DE.CM-1, DE.CM-3, PR.AC-7 |

---

### Control Recommendations

**Immediate — Preventive Control Gap:**
Detection alone is insufficient. Implement account lockout after a defined threshold (e.g. 5 failed attempts within 15 minutes) to convert this from a purely detective control into a combined detective-preventive control. Without this, the SIEM correctly identifies the attack but does not stop it.

Maps to ISO 27001 A.8.5 (Secure authentication).

**Short-term — Privileged Account Hardening:**
The Dr.Nee account, identified as privileged, should be protected with multi-factor authentication as a priority over standard accounts. Brute-force attempts against privileged accounts represent materially higher business risk and should carry a lower alert threshold than standard accounts.

Maps to ISO 27001 A.5.17 and NIST CSF PR.AC-7.

**Medium-term — Alert Response Workflow:**
The current alert logs the event but does not route to an analyst or ticketing system. Integrate the alert action with a notification channel (email, SOAR, or ticketing system) to ensure a human reviews and responds to each triggered alert within a defined SLA — a requirement for demonstrating effective incident response under ISO 27001 A.5.24.

Maps to ISO 27001 A.5.24 and NIST CSF RS.AN-1.

**Ongoing — Monitoring Coverage Expansion:**
This control currently monitors only Windows authentication events. Expand log source coverage to include Linux SSH authentication (`auth.log`), VPN authentication, and cloud identity provider sign-in logs to ensure the monitoring control provides comprehensive coverage across the environment — a common audit finding is partial monitoring coverage presented as complete.

Maps to ISO 27001 A.8.16 and NIST CSF DE.CM-1.

---

## 📚 GRC Concepts Applied

| Concept | Application in This Project |
|---|---|
| Continuous Control Monitoring | Real-time SIEM alert operationalising a detective control |
| Audit Evidence Generation | 978 indexed events demonstrate the log pipeline is operational, not just configured |
| Control Effectiveness Testing | Simulated attack used to validate the control actually triggers as designed |
| Threshold-Based Risk Decision | `count >= 3` documented as a deliberate sensitivity/false-positive trade-off |
| Privileged Account Risk Differentiation | Dr.Nee (privileged) and testuser (standard) assessed at different inherent risk levels |
| Detective vs Preventive Control Gap Analysis | Identified that detection without lockout leaves a preventive control gap |
| Risk Register Entry | RISK-AC-002 produced in GRC risk register format |
| ISO 27001 Mapping | A.8.16, A.8.15, A.5.17, A.8.5, A.5.24 mapped to findings and recommendations |
| NIST CSF Mapping | DE.CM-1, DE.CM-3, PR.AC-7, RS.AN-1 mapped to control gaps |
| MITRE ATT&CK Mapping | T1110 / T1110.001 / TA0006 |
| Monitoring Coverage Assessment | Identified partial coverage (Windows only) as a control scope gap |

### Key Questions to Prepare From This Project

**Q: How does a SIEM alert become audit evidence?**
The alert configuration itself (screenshot, settings) demonstrates the control exists. The 978 indexed events demonstrate the control is actually receiving data. The detection query results demonstrate the control correctly identifies anomalous activity. Together these three layers — design, operation, and effectiveness — are exactly what an ISO 27001 internal or external auditor tests when sampling a control.

**Q: Why is detecting a brute-force attack not the same as preventing one?**
Detection is a detective control — it identifies that something happened, ideally in time to respond. Prevention requires a separate control such as account lockout or MFA. This project's SIEM alert is purely detective. A mature control environment pairs detective controls with preventive ones — relying on detection alone leaves a window where an attacker can still succeed before a human responds.

**Q: Why does the privileged account (Dr.Nee) matter more than the standard account in this incident?**
Risk is a function of likelihood and impact. Both accounts had identical failed-attempt counts, but compromise of a privileged account has materially higher impact — broader system access, ability to disable controls, or escalate further. A GRC risk assessment would assign these two findings different risk ratings despite identical technical evidence, because business impact differs.

**Q: What would an auditor ask about this monitoring control that this project doesn't yet answer?**
Three follow-up questions an auditor would raise: Is this alert routed to a person who reviews it, and within what SLA? Does the lockout policy exist to stop the attack, or only detect it? Does monitoring coverage extend to all authentication sources in the environment, or only Windows? This project's control recommendations section directly addresses all three gaps.

**Q: How would you explain a threshold-based detection rule's risk trade-off to a compliance officer?**
A lower threshold (e.g. 2 failed attempts) catches attacks faster but generates more false positives from legitimate users mistyping passwords — increasing analyst workload and alert fatigue. A higher threshold (e.g. 10) reduces noise but gives an attacker more attempts before detection. The threshold of 3 in this project represents a documented, deliberate risk-based decision — exactly the kind of judgement call a GRC analyst should be able to justify when an auditor asks "why this number?"

---

## 📸 Screenshots Index

| # | Filename | Description |
|---|---|---|
| 01 | GRC4-01-splunk-start-terminal.png | Splunk Enterprise service startup |
| 02 | GRC4-02-splunk-login-page.png | Splunk web interface login |
| 03 | GRC4-03-splunk-home-dashboard.png | Splunk home dashboard |
| 04 | GRC4-04-kali-network-ip.png | Kali IP — 10.232.194.7 |
| 05 | GRC4-05-windows-ping-kali.png | Connectivity verified — 0% packet loss |
| 06 | GRC4-06-data-inputs-page.png | Splunk data inputs configuration |
| 07 | GRC4-07-receiving-port-9997.png | Receiving port 9997 enabled |
| 08 | GRC4-08-windows-logs-in-splunk.png | 978 events ingested — pipeline verified |
| 09 | GRC4-09-test-user-created.png | Test account created |
| 10 | GRC4-10-failed-login-attempts.png | Failed authentication simulation |
| 11 | GRC4-11-event-4625-search.png | EventID 4625 search results |
| 12 | GRC4-12-event-details-expanded.png | Event detail — forensic fields |
| 13 | GRC4-13-detection-query-results.png | Detection query — 2 accounts flagged |
| 14 | GRC4-13-1-detection-visualization.png | Column chart — failure counts by account |
| 15 | GRC4-14-create-alert-dialog.png | Alert creation — Real-time, Per-Result |
| 16 | GRC4-15-alert-trigger-config.png | Alert action — dynamic field substitution |
| 17 | GRC4-16-alert-saved-confirmation.png | Alert saved confirmation |
| 18 | GRC4-17-alert-dashboard.png | Alert status — enabled and operational |

---

## 📁 Repository Structure

```
GRC4-Security-Control-Monitoring-Splunk/
│
├── README.md                              ← Complete control monitoring documentation
├── incident_report.docx                   ← Formal incident report
├── detection_query.txt                     ← SPL detection queries with explanations
├── setup_guide.md                          ← Step-by-step implementation instructions
│
└── screenshots/                            ← 18 annotated evidence screenshots
    ├── GRC4-01-splunk-start-terminal.png
    ├── GRC4-02 through GRC4-17 ...
```

---

## ⚖️ Disclaimer

> This lab was conducted entirely in a **controlled, isolated virtual environment** for educational and professional development purposes. All accounts, credentials, and authentication attempts were created and executed by the author on systems under the author's own control. No external systems, networks, or third-party accounts were involved. The methodologies demonstrated should only be applied within authorised organisational environments or designated security training platforms.

---

<div align="center">

*GRC4 · Splunk Enterprise · Windows Event Logs · SPL · MITRE ATT&CK T1110*

*Security Control Monitoring · Access Control · ISO 27001 · NIST CSF · Audit Evidence · Continuous Compliance*

</div>
