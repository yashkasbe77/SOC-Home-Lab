# SOC Alert Monitoring & Log Correlation Lab

## 📌 Project Overview
This project is a comprehensive Security Operations Center (SOC) home lab designed to simulate a targeted attack lifecycle and demonstrate how a SIEM detects these threats in real-time. The lab focuses on bridging the gap between raw endpoint telemetry and actionable threat intelligence.

## 🏗️ Lab Architecture
* **Attacker Machine (Hardware):** Dedicated laptop running Kali Linux, utilizing `crackmapexec` to simulate external threat actor behaviors.
* **SIEM Infrastructure (Virtual):** Desktop running VMware hosting an Ubuntu Server (Wazuh Manager & Wazuh Indexer).
* **Target Endpoint (Hardware/Host):** Windows Desktop host configured with advanced security auditing, Sysmon, and the Wazuh Agent to forward real-time telemetry.

## 🔍 Investigation Flow & Kill Chain Mapping

### Phase 1: Initial Access (T1110)
* **Attack:** Dictionary brute-force attack against the endpoint's SMB service.
* **Detection:** Correlated a rapid succession of **Event ID 4625** (Logon Failure) followed by a successful authentication **Event ID 4624**.

### Phase 2: Privilege Escalation & Persistence (T1136.001, T1098)
* **Attack:** Attacker escalated privileges to create a rogue user ("hacker") and added it to the local Administrators group.
* **Detection:** Captured **Event ID 4720** (User Creation) and **Event ID 4732** (Security Group Management), triggering a critical severity alert for unauthorized privilege escalation.

### Phase 3: Defense Evasion (T1059.001)
* **Attack:** Utilized PowerShell bypass techniques to execute payloads without interference from local execution policies.
* **Detection:** Analyzed **Event ID 4688** (Process Creation) to correlate the `powershell.exe` process with the explicit flags `-ExecutionPolicy Bypass -NoProfile`.

### Phase 4: Data Manipulation & FIM (T1565.001)
* **Attack:** Manipulated and deleted files within critical user directories.
* **Detection:** Leveraged Wazuh's **File Integrity Monitoring (Syscheck)** to create a forensic timeline of all "added", "modified", and "deleted" actions.

## 📈 Impact Analysis & Rule Tuning
To optimize the SIEM and reduce SOC analyst alert fatigue, the following tuning measures were developed:
1. **Brute Force Thresholds:** Adjusted correlation rules to trigger only if >15 failures occur within a 3-minute window from the same source IP.
2. **Process Creation Filtering:** Whitelisted known updater services while flagging high-severity alerts for suspicious arguments like `-ep bypass`.

## 📄 Full Project Report
For the complete step-by-step investigation workflow, dashboard visualizations, and DQL queries, please view the full PDF report included in this repository:
[➡️ Click here to view the SOC Alert Monitoring PDF](Upload your PDF to the repo and link it here)
