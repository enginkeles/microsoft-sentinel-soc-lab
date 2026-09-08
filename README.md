# Microsoft Sentinel SOC Lab

- Exposed an Azure VM to the internet as an RDP honeypot.
  - Temporarily configured the NSG to allow inbound traffic.
  - Temporarily disabled Windows Firewall profiles.
 
- Configured Windows Security Events to be forwarded into a Log Analytics Workspace (LAW).
  - Created a Log Analytics Workspace.
  - Installed the Azure Monitor Agent (AMA) on the Windows VM.
  - Created a Data Collection Rule (DCR) to collect Windows Security Events from the VM and send them to the LAW.
  - Generated failed login attempts to confirm logs were being ingested.
  - Received a large number of failed login attempts from unknown external IPs.
  - Used KQL against the SecurityEvent table to view and analyse the logs.
  - Filtered events by fields such as EventID and source IP.
 
- Enabled Sentinel on the Log Analytics Workspace.
  - Added Sentinel to the LAW.
  - Analysed the security logs stored in the workspace.

- Visualised the failed login attempts.
  - Created a Sentinel Watchlist containing IP geolocation data.
  - Used the watchlist to add geographic information to the source IPs.
  - Created a Sentinel Workbook to visualise the volume and geographic origin of failed login attempts.

 ---------------------------------------------------------------------------------------------------------------------
- Extend the lab by collating other logs like more Windows Security Events, Sysmon, Powershell, and Entra ID logs.
  - Installed Sysmon but having issues with getting Logs to show up in the LAW.
    - Created new DCR for Sysmon events but KQL query not showing the events. 
- Write KQL detections for these logs and create Sentinel Analytics Rules to automate detection and trigger alerts.
- Generate attacks to test the detections.
- Identify false positives/negatives and possible gaps in logic.
- Document the detection logic, methodology, and findings.
