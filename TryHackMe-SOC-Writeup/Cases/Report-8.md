INCIDENT TICKET: Malicious Tool Ingress (PowerView.ps1)

Date: February 18, 2026
Analyst: Alperen Metin
Verdict: True Positive – Escalated to Tier 2




![PowerView File Creation](Images/8-splunk-log.png)




A Sysmon Event ID 11 (File Create) alert was triggered on host win-3450, indicating that PowerView.ps1 was created in the path C:\Users\michael.ascot\Downloads\ by powershell.exe (PID: 9060). PowerView is a well-known PowerShell-based reconnaissance tool commonly used by attackers for Active Directory enumeration and discovery of sensitive network resources. The fact that this file was created directly by PowerShell in a standard user’s Downloads directory is a strong indicator of malicious activity and tool staging. This event is likely related to the subsequent suspicious network share access and DNS exfiltration activity observed on the same host. The host was confirmed to be isolated from the network, the file hash was recommended for blocklisting (if available via EDR), and the incident was escalated to Tier 2 for further investigation into the initial execution vector.