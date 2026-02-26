INCIDENT TICKET: Inbound Spam / Advance-Fee Scam Attempt

Date: February 18, 2026
Analyst: [Your Name]
Verdict: True Positive – Spam (Low Severity)


![Inbound Spam Email](Images/6-splunk-log.png)

An inbound email alert was triggered for a message originating from a public Gmail address (josephine@gmail.com). Review of the Splunk email logs confirmed the message was a generic advance-fee scam attempt, requesting payment (“500 per ticket”) for a fictitious service. The content was clearly unsolicited and not related to business operations. No attachments, malicious URLs, or embedded payloads were identified, indicating there was no direct technical threat to the environment. The overall risk was assessed as low, limited to potential social engineering exposure if a recipient engaged with the sender. The sender address was added to the email security blocklist, the message was removed from the contact@tryhatme.com inbox, and the alert was closed as a True Positive (Spam) with no escalation required.