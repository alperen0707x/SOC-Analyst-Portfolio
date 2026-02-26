# Case Study: Suspicious Process Analysis (Alert ID 1002)

**Date:** Feb 18, 2026
**Analyst:** Alperen Metin
**Alert:** A suspicious process with an uncommon parent-child relationship was detected
**Verdict:** False Positive (Normal System Behavior)

---

## 🧐 What Happened?
I received an alert stating that a suspicious process was detected on the host `win-3451`. The alert mentioned an "uncommon parent-child relationship." My task was to investigate if this was a real malware infection or just normal Windows activity.

## 🕵️‍♂️ My Investigation Steps

### 1. Checking the Sysmon Logs
I searched the logs in Splunk and found the exact Event Code 1 (Process Creation) that triggered the alert. 

Here is what I found:
* **Host:** `win-3451`
* **Parent Process:** `svchost.exe`
* **Child Process:** `taskhostw.exe`
* **Command Line:** `taskhostw.exe KEYROAMING`
* **Folder Path:** `C:\Windows\system32\`

*(Splunk Evidence)*
`![Sysmon Log Details](../Images/2-sysmon-log.png)`

### 2. Analyzing the Behavior (Why it's not suspicious)
To determine if this is malicious, I asked myself a few questions:
* *Are these normal Windows files?* Yes. `svchost.exe` (Service Host) and `taskhostw.exe` (Host Process for Windows Tasks) are legitimate Microsoft built-in programs.
* *Are they in the right folder?* Yes, they are running from `C:\Windows\system32\`, which is the official Windows system folder. If they were running from a temporary folder, I would be worried.
* *Is the command normal?* Yes, `KEYROAMING` is a standard scheduled task in Windows used for syncing settings.

## 📝 Conclusion & What I Learned
This alert is a **False Positive**. There is no malware. The security rule is simply too sensitive and flagged a normal background task that Windows performs every day.

**Actions I Took:**
* Closed the alert as a False Positive.
* Recommended the SOC Lead to tune (adjust) the SIEM rule so it ignores `taskhostw.exe KEYROAMING` in the future. We don't want analysts wasting time on normal Windows operations.

**My Key Takeaway:**
I learned that not every SIEM alert means we are under attack. Knowing how normal Windows processes behave (like `svchost.exe` spawning other tasks) is just as important as knowing how malware behaves.