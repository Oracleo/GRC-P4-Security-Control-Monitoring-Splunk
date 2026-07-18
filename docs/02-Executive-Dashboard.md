# 2. Executive Dashboard: Security Control Monitoring Status

**Reporting Period:** February 6, 2026
**Project:** GRC-P4 - Security Control Monitoring & Risk Documentation | Splunk SIEM
**Prepared For:** CISO / Senior Leadership
**Prepared By:** Niladri Biswas

---

## 📈 Executive Summary (Management KPIs)

This dashboard provides a high-level strategic view of our current Access Control Monitoring capabilities, following the validation of our Splunk SIEM deployment. 

| KPI | Status | Commentary |
|:---|:---:|:---|
| **SIEM Log Ingestion** | 🟢 **Operational** | 978 Windows Security Events successfully indexed. Log pipeline is healthy. |
| **Brute Force Detection** | 🟢 **Confirmed** | 8 failed logins (Event ID 4625) detected across 2 accounts within 24 hours. |
| **Real-Time Alerting** | 🟢 **Active** | Splunk alert configured and successfully triggered on detection threshold (`count >= 3`). |
| **Automated Prevention** | 🔴 **Gap Identified** | No Account Lockout or MFA controls currently active on local accounts. |
| **Incident Response Workflow** | 🟡 **Manual** | Alerts are logged but not automatically routed to the SOC ticketing system/email. |

---

## 🔒 Control Posture Heatmap (ISO 27001 Mapping)

| Control ID | Control Name | Current Status | Compliance Gap |
|:---:|:---|:---:|:---|
| **A.8.15** | Logging | ✅ **Compliant** | None. |
| **A.8.16** | Monitoring Activities | ✅ **Compliant** | None. |
| **A.5.17** | Authentication Information | ⚠️ **Control Gap** | MFA is not enforced. |
| **A.8.5** | Secure Authentication | ❌ **Non-Compliant** | Account lockout policy is absent. |

---

## 🎯 Top 3 Strategic Action Items

1.  **Mitigate Brute-Force Immediate Risk (P1):** Enable Windows Account Lockout Policy (5 attempts within 15 mins). Cost: **~$200** (1 admin hour). 
    - *Rationale:* This shifts our posture from *reactive* detection to *proactive* prevention.
2.  **Protect Privileged Accounts (P2):** Enroll the `Dr.Nee` (Admin) account into a Multi-Factor Authentication (MFA) solution. 
    - *Rationale:* A compromised `Dr.Nee` allows lateral movement to the finance infrastructure, representing a **$500,000+** breach risk.
3.  **Operationalize Alert Response (P2):** Integrate Splunk alerts with a SOAR or email ticketing system.
    - *Rationale:* Ensures the SOC identifies and acts on alerts within an SLA, fulfilling NIST RS.AN-1 requirements.

---

## 📊 Risk Appetite & Board-Level Statement

> *"Our current configuration successfully detects authentication anomalies and provides a clear audit trail. However, our reliance on *detection only* leaves a critical window of vulnerability. We must immediately implement preventive controls (Lockout & MFA) to align our Access Control Monitoring with the organization's low-risk appetite and meet ISO 27001 compliance standards."*
