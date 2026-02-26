# TryHackMe SOC Analyst Simulation - Incident Triage Portfolio
Alperen Metin


## Overview
This repository contains a collection of 10 technical incident reports generated during the [TryHackMe SOC Simulation](https://tryhackme.com/soc-sim/alert-queue) environment. The project demonstrates hands-on experience in a Security Operations Center (SOC) role, focusing on alert triage, log analysis, threat hunting, and incident escalation.

## Environment & Tools
* **SIEM:** Splunk
* **Log Sources:** Sysmon (Windows Event Logs), Email Gateway Logs
* **Skills Demonstrated:** * Alert Triage (Determining True Positives vs. False Positives)
    * Log Correlation & Timeline Reconstruction
    * Identifying MITRE ATT&CK Techniques (Living off the Land, Defense Evasion, Exfiltration)
    * Professional Technical Writing & Incident Reporting

## Incident Highlights
During this simulation, I investigated various alerts ranging from harmless spam to a critical, multi-stage data exfiltration attack. The reports detail my investigation processes across two main categories:

### 1. The Advanced Attack Chain (APT Simulation)
I successfully correlated multiple isolated alerts to uncover a continuous attack by a threat actor on host `win-3450`. The investigation tracked the following kill chain:
* **Tool Ingress:** Identified the malicious drop of `PowerView.ps1` in a user's directory for Active Directory enumeration.
* **Lateral Movement:** Tracked the unauthorized mapping of highly sensitive financial network shares (`net.exe`).
* **Data Collection:** Discovered the use of native Windows tools (`Robocopy.exe`) to quickly stage stolen data into a hidden exfiltration folder.
* **Defense Evasion:** Identified the attacker's attempt to cover their tracks by abruptly deleting network mappings.
* **Data Exfiltration:** Detected and contained DNS Tunneling (`nslookup.exe`) used to smuggle encoded financial data out of the network.

### 2. Routine SOC Operations
* **Phishing & Scams:** Analyzed, documented, and contained inbound social engineering and advance-fee scam attempts.
* **False Positive Tuning:** Investigated alerts triggered by normal Windows background processes (e.g., `svchost.exe` spawning `taskhostw.exe`) and provided context to reduce alert fatigue.

## Repository Structure
* `/Reports/` - Contains the detailed Markdown (`.md`) incident tickets and analysis steps.
* `/Images/` - Contains the corresponding Splunk log evidence, SIEM dashboards, and queries used to build the reports.

---
*Disclaimer: All data, IP addresses, and user information within these reports are part of a simulated lab environment provided by TryHackMe.*