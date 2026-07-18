# 4. Assumed Business Context & Corporate Profile

In a professional GRC assessment, security controls are never implemented in a vacuum. They are driven by the organization's business goals, regulatory obligations, and operational structure. For this risk engagement, we project the following fictitious business context to accurately score risk and prioritize control recommendations.

## 4.1 Organization Profile

| Attribute | Assumed Context |
|:---|:---|
| **Organization Name** | *NovaFin Financial Services Ltd.* |
| **Industry Sector** | Financial Services / Investment Banking |
| **Size** | Mid-market enterprise (~5,000 employees globally). |
| **Primary Business Function** | Portfolio management, real-time financial trading, and client wealth management. |
| **Regulatory Footprint** | Subject to **GDPR** (EU clients), **PCI DSS** (Handling client credit card data for fund transfers), and local financial fraud regulatory bodies. |

## 4.2 IT & Security Operating Model

| Asset Attribute | Assumed Context |
|:---|:---|
| **IT Infrastructure** | Hybrid environment (On-premise Windows 10 endpoints hosted on Hyper-V, alongside Azure AD). |
| **Security Operations (SOC)** | A small, in-house SOC team responsible for monitoring on-premise SIEM logs, triaging alerts, and responding to incidents. |
| **High-Value Assets** | The `NATIONALIST` Windows 10 endpoint belongs to the **Finance and HR Department**, acting as the gateway to the internal payroll database and financial transfer systems. |
| **Critical Accounts** | `Dr.Nee` is a delegated local Administrator on the `NATIONALIST` host, possessing privileges to access financial spreadsheets and RDP into critical financial servers. |

## 4.3 Business Justification for the Control
As a financial services firm, the primary threat we face is **data exfiltration** and **financial fraud** via credential theft. A successful brute-force attack against a privileged account (like `Dr.Nee`) would result in:
1.  **Financial Impact:** Unauthorized wire transfers or insider trading data leaks costing millions.
2.  **Regulatory Impact:** Immediate GDPR Article 33 breach notification within 72 hours, and potential PCI-DSS fines.
3.  **Reputational Impact:** Irreparable loss of client trust in the security of their wealth management funds.

**GRC Conclusion:** The controls implemented in this lab (Splunk SIEM monitoring) are not optional technical toys. They are foundational business controls designed to protect the organization from existential financial and reputational ruin.
