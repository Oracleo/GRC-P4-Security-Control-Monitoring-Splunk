# 9. MITRE ATT&CK Mapping

Mapping the brute-force event to MITRE ATT&CK allows us to understand the adversary's tactic and apply the specific mitigations required to disrupt the attack.

## Tactical Kill-Chain Mapping
| Vulnerability / Threat | MITRE Tactic | MITRE Technique ID | Technique Name | Adversary Action Enabled |
|:---|:---|:---|:---|:---|
| **Multiple Failed Logins (EventID 4625)** | **TA0006 (Credential Access)** | **T1110.001** | Brute Force: Password Guessing | The adversary repeatedly attempts to guess valid credentials for the `testuser` and `Dr.Nee` accounts to gain unauthorized access to the workstation. |
| **Absent MFA & Lockout** | **TA0006 (Credential Access)** | **T1110** | Brute Force | The lack of MFA and lockout reduces the adversary's required time to compromise a privileged account, enabling Credential Access. |

## Attack Chain Scenario (The "Real World" Threat)
1.  **Reconnaissance (TA0043):** Attacker identifies the `NATIONALIST` hostname and a valid Admin account (`Dr.Nee`) via OSINT or network scanning.
2.  **Resource Development (TA0042):** Attacker utilizes a password dictionary or common wordlist.
3.  **Credential Access (TA0006 - T1110.001):** Attacker repeatedly attempts to log in as `Dr.Nee` using `runas` or RDP.
4.  **Privilege Escalation (TA0004):** If the password is discovered, the attacker gains ADMIN access.
5.  **Exfiltration (TA0010) / Impact (TA0040):** The attacker pivots to the HR database, exfiltrates payroll data, or encrypts files (Ransomware).

**GRC Recommendation:** To disrupt this kill-chain:
1.  Implement **Account Lockout policies** to stop T1110.001 after 5 failed attempts.
2.  Implement **Multi-Factor Authentication (MFA)** to immediately neutralize T1110, as the password alone would be insufficient to login.
3.  Enhance **SIEM Detection** to trigger a **SOAR** response to automatically block the attacking IP (`src_ip`) if 3+ failures occur in a 10-minute window.
