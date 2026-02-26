INCIDENT TICKET: Suspicious File Copy Activity (Robocopy Execution)

Date: February 18, 2026
Analyst: Alperen Metin
Verdict: True Positive – Escalated to Tier 2

Time of Activity: 02/18/2026 12:24:45.731



![PowerView File Creation](Images/10-splunk-log.png)

This alert was triggered by a Sysmon Event ID 1 (Process Create) on host win-3450 at 12:24:45.731. The affected entity is the host win-3450 under the user context michael.ascot. The log shows that Robocopy.exe was executed from C:\Windows\system32\Robocopy.exe with the command line copying data to C:\Users\michael.ascot\downloads\exfiltration\ using the /E parameter. The parent process was powershell.exe (PID: 3728), which has already been linked to previous malicious activity on the same host. The working directory was Z:\, which indicates the mapped network drive previously associated with the sensitive financial share. This activity was classified as a True Positive because Robocopy is commonly abused by attackers to stage or collect large amounts of data before exfiltration, and the execution context strongly connects it to the ongoing attack chain. The alert was escalated because it indicates possible data staging from a network share to a local exfiltration folder, increasing the risk of data loss. Recommended remediation actions include maintaining host isolation, preserving forensic evidence, reviewing file access logs for the Z:\ drive, and validating whether sensitive data was copied. Attack indicators identified include execution of Robocopy.exe from a PowerShell parent process (PID 3728), working directory Z:\, and the suspicious destination folder C:\Users\michael.ascot\downloads\exfiltration\.

Status: Closed by Tier 1 – Escalated to Tier 2 for further investigation**