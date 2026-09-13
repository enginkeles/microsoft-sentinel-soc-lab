# Microsoft Sentinel SOC Lab

Built a Microsoft Sentinel SOC lab using an internet-exposed Azure Windows VM. Collected Windows Security Events, Sysmon, and PowerShell logs in Log Analytics using Azure Monitor Agent and Data Collection Rules. Used KQL to investigate the collected telemetry, developed and tuned detection rules, and built a Sentinel Workbook to visualise authentication attempts against the VM using IP geolocation data.

## RDP Honeypot

- Created an Azure Windows VM and exposed RDP to the internet.
  - Temporarily configured the Network Security Group (NSG) to allow inbound traffic.
    ![allow_all_traffic](images/allow_all_traffic.png)
    ![allow_all_traffic2](images/allow_all_traffic2.png)
    
  - Temporarily disabled Windows Firewall profiles.

    ![windows_firewall_off](images/windows_firewall_off.png)

    
## Log Collection

- Created a Log Analytics Workspace (LAW) and installed Azure Monitor Agent (AMA) on the Windows VM.

  ![ama_installed](images/ama_installed.png)

- Created a Data Collection Rule (DCR) to collect Windows Security Events and send them to the LAW.

  ![connecting_logs_dcr](images/connecting_logs_dcr.png)

- Generated failed login attempts to verify log ingestion.

  ![failed_login_attempt_logs](images/failed_login_attempt_logs.png)

- Observed failed login attempts from unknown external IP addresses and used KQL against the `SecurityEvent` table to investigate the events and filter by fields including Event ID and source IP.

  ![failed_login_attempts_filtered](images/failed_login_attempts_filtered.png)

### Sysmon

- Installed and configured Sysmon on the Windows VM.

  ![whoami-ran-log](images/whoami-ran-log.png)

- Created a separate DCR for Sysmon events and verified successful ingestion into Log Analytics.
- Used KQL to query and analyse the collected Sysmon events.

  ![filtered-event-query](images/filtered-event-query.png)

### PowerShell

- Collected PowerShell Script Block Logging events (Event ID 4104).
- Used KQL to query and analyse PowerShell execution data.

  ![v2-powershell-query](images/v2-powershell-query.png)


## Microsoft Sentinel

- Enabled Microsoft Sentinel on the Log Analytics Workspace.
- Used Sentinel to investigate the collected security telemetry.

  ![v1-incident-investigation](images/v1-incident-investigation.png)


## Detection Rules

- Created a KQL detection for PowerShell reconnaissance using Event ID 4104.
- The initial detection searched PowerShell event data for reconnaissance commands but generated false positives from PowerShell module and framework code.

  ![v1-powershell-query](images/v1-powershell-query.png)

- Created a second version that parsed the `EventData` XML and extracted `ScriptBlockText` to target the executed script content and reduce noise.

  ![v2-powershell-query-2](images/v2-powershell-query-2.png)

- Deployed both versions as Microsoft Sentinel Analytics Rules.

  ![scheduled-rule](images/scheduled-rule.png)

- Generated controlled PowerShell reconnaissance activity to test both rules and verify alert and incident generation.

  ![v1-rule-incidents](images/v1-rule-incidents.png)

  ![v2-rule-incidents](images/v2-rule-incidents.png)


## Authentication Attempt Visualisation

- Created a Sentinel Watchlist containing IP geolocation data.
- Used the watchlist to enrich source IP addresses with geographic information.

  ![query_bruteforce_geolocation](images/query_bruteforce_geolocation.png)

- Created a Sentinel Workbook to visualise the volume and geographic origin of authentication attempts against the VM.

  ![bruteforce_attempts_geolocation_map](images/bruteforce_attempts_geolocation_map.png)
