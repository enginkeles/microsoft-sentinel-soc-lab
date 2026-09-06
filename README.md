# Microsoft Sentinel SOC Lab

- Expose an Azure VM to the internet, allowing failed RDP attempts.
- Configure log forwarding to a LAW.
- Connect the LAW to Sentinel.
- Use KQL to query the SIEM and collate and visualise logs of failed RDP attempts.
- Extend the lab by collating other logs like more Windows Security Events, Sysmon, Powershell, and Entra ID logs.
- Write KQL detections for these logs and create Sentinel Analytics Rules to automate detection and trigger alerts.
- Generate attacks to test the detections.
- Identify false positives/negatives and possible gaps in logic.
- Document the detection logic, methodology, and findings.
