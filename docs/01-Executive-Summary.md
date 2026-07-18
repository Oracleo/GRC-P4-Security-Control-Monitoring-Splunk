# 1. Executive Summary

*  **Date of Investigation:** February 6, 2026
*  **Conducted By:** Niladri Biswas
*  **Assessment Type:** Security Control Monitoring Validation
*  **Target Asset:** Organizational Windows Authentication Infrastructure

## Situation Overview
A Security Control Monitoring lab was established to validate the organization's ability to detect anomalous authentication activity. A Splunk Enterprise SIEM was deployed, ingesting Windows Security Event Logs (EventCode 4625) via a Universal Forwarder. A simulated brute-force attack was executed against a privileged user account (`Dr.Nee`) and a standard user account (`testuser`).

## Key Findings
*   **Control Validation Success:** The SIEM successfully indexed **978 Windows Security events**, confirming the log pipeline is fully operational.
*   **Threat Detected:** 8 failed login attempts (4 per account) were logged. The SPL query successfully flagged both accounts as they crossed the **`count >= 3`** detection threshold.
*   **Automated Response:** A Real-time Splunk alert has been configured. It successfully triggers and logs an alert when the threshold is exceeded, proving the detective control is operational.

## Strategic Recommendations
1.  **Preventive Control Hardening (Immediate):** Implement account lockout policies (e.g., 5 attempts / 15 minutes) to convert this purely *detective* control into a *combined detective and preventive* control.
2.  **Privileged Account MFA (Short-term):** Enforce Multi-Factor Authentication (MFA) on privileged accounts like `Dr.Nee`, as a compromise here represents a much higher business risk.
3.  **SOAR Integration (Medium-term):** Configure the Splunk alert to trigger an automated email or ticketing system notification, ensuring the SOC reviews the incident within a defined SLA.

## Conclusion
This project proves that our SIEM is properly ingesting and analyzing authentication logs and is capable of detecting credential attacks in real-time. However, detection alone is not enough. Immediate action is required to implement account lockout and MFA to actively stop the attack, rather than just reporting it after the fact.
