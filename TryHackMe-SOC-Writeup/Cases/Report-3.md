# Case Study: Suspicious Network Share Mapping (Alert ID 1003)

**Date:** Feb 18, 2026
**Analyst:** Alperen Metin
**Alert:** Suspicious process execution mapping a network drive
**Verdict:** True Positive (Highly Suspicious Activity / Potential Ransomware or Exfiltration)
**Status:** Escalated to Tier 2 / Incident Response

---

## 🧐 What Happened?
I was reviewing Sysmon Event Code 1 (Process Creation) logs and noticed a highly suspicious command executed on the host `win-3450`. A process was attempting to map a network drive to a sensitive financial folder.

## 🕵️‍♂️ My Investigation Steps

### 1. Breaking Down the Log
Looking at the Splunk log, I identified three major red flags:

* **The User & Location:** The process was executed from `C:\Users\michael.ascot\downloads\`. The Downloads folder is highly untrusted as it's the landing zone for internet files.
* **The Parent Process:** The command was initiated by `powershell.exe`. Attackers frequently use PowerShell for "Living off the Land" attacks.
* **The Command Executed:** `"C:\Windows\system32\net.exe" use Z: \\FILESRV-01\SSF-FinancialRecords`

*(Splunk Evidence)*
`![Sysmon Log Details](../Images/3-splunk-log.png)`

### 2. Connecting the Dots (The Threat)
While `net.exe` is a legitimate Windows tool, the context makes it dangerous. A PowerShell script running from the Downloads folder should *never* automatically attempt to access the company's "Financial Records" share (`\\FILESRV-01\SSF-FinancialRecords`). 

This behavior strongly suggests that the user `michael.ascot` downloaded a malicious payload (like a script or macro), which is now trying to move laterally to access, steal, or encrypt sensitive financial data.

## 📝 Conclusion & Escalation
Due to the high risk of data exfiltration or a ransomware infection targeting financial records, I immediately categorized this as a **True Positive**. 

**Actions I Took:**
1.  **Isolated** the infected host (`win-3450`) from the network to prevent the malware from reaching the financial server.
2.  **Escalated** the ticket to Tier 2 / Incident Response team for deeper malware analysis on the downloaded file.

**My Key Takeaway:**
Context is everything. A legitimate tool like `net.exe` can be used maliciously. Always look at the *Parent Process* and the *Working Directory* to understand the true intent behind a command.