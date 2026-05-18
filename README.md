# azure-soc-honeypot-lab

##Overview
In this project, I deployed a cloud-based Security Operations Center (SOC) and honeypot in Microsoft Azure to detect, monitor, and analyze real-world attack traffic. I configured Microsoft Sentinel as the SIEM to aggregate logs and trigger alerts from an intentionally exposed virtual machine.  

## Tools & technologies used
- Microsoft Azure (Virtual Machine, Log Analytics Workspace)
- Microsoft Sentinel (SIEM)
- Windows Event Viewer
- KQL (Kusto Query Language) for Log queries
- Remote Desktop Protocol (RDP)

## Architecture


## What I did
  1. Created an Azure VM(Student Account) and intentionally disabled firewall rules
  2. Configured Log Analytics to ingest Windows Security Event Logs
  3. Connected Sentinel to the workspace and enabled data connectors
  4. Wrote custom KQL queries to  detect brute force login attempts
  5. Observed real attack traffic within 24 hours of deployment
  6. Documented attacker IPs, geolocations, and plugged in Sentinel to show a global map for attacks.
 
## Key findings

- received X failed RDP login attempts within 24 hours
- Top attacker countries:
- Most targated username: ....
- Configured alret rules to trigger on 5+ failed logins in 5 minutes

## What I learned


## Screenshots of Steps
