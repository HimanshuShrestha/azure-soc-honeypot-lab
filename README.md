
# Cloud-Native SIEM & Honeypot Deployment (Azure Sentinel)

## Overview
In this project, I deployed a cloud-based Security Operations Center (SOC) and honeypot in Microsoft Azure to detect, monitor, and analyze real-world attack traffic. I configured Microsoft Sentinel as the SIEM to aggregate logs and trigger alerts from an intentionally exposed virtual machine. 
Over 24 hours, the environment captured over 74k automated brute force attacks, analyzed threat actor behaviors, and successfully created a localized incident response and remediation protocol. 

## Tools & technologies used
- Microsoft Azure (Virtual Machine, Log Analytics Workspace, Network Security Groups)
- Microsoft Sentinel (SIEM)
- Windows Event Viewer
- KQL (Kusto Query Language) for Log queries
- Remote Desktop Protocol (RDP-port 3389)

## Architecture
<img width="916" height="501" alt="Lab Architecture" src="https://github.com/user-attachments/assets/6b8ecc3b-f201-4115-b37e-fb6ac75513cc" />

## What I did
1. Asset Provisioning & Vulnerability Introduction
   - Deployed an enterprise-grade Windows Server virtual machine instance
   - Intentionally introduced vulnerabilities by establishing an inbound security rule on Azure NSG, allowing any traffic on port 3389
   - Disabled Windows security firewall
2. Setting up Data Pipeline
   - Configured Log Analytics to ingest Windows Security Event Logs
   - Configured Azure Monitor Agent to stream Windows Security logs from inside the VM straight to our workspace database.
   - Connected Sentinel to the workspace and enabled data connectors
3. Adding the Global Map (Threat Intel)
   - Imported a custom GeoIP watchlist spreadsheet into Sentinel. This spreadsheet matches IP addresses to real-world countries and coordinates.
   - Used a KQL query inside Azure Workbooks to look at the failed login IPs, match them with our location spreadsheet, and draw visual dots on a world map.
   - Observed real attack traffic within 24 hours of deployment
4. Creating the Alert Rule
   - Built an automation rule in Sentinel that triggers an alert if any single IP address fails to log in more than 10 times within 5 minutes.
   - Monitored the incidents dashboard to ensure that when a bot hit our threshold, Sentinel automatically generated a ticket and put it into our queue to review.
 
## Key findings & Attack Analysis
After letting the honeypot run for 24 hours, the data showed exactly how aggressive automated internet bots are:
  - received 74k failed RDP login attempts within 24 hours
  - Top attacker countries: Poland 
  - Most targeted username: Administrator
  - Configured alert rules to trigger on 10+ failed logins in 5 minutes
  - While looking at the incident found, I caught an interesting case where a bot tried to log in exactly 9 times using broken data that Windows recorded as NOUSER. Because my automation alert rule needed more than 10 failures to fire, this script successfully avoided creating an automatic incident.

## Locking Down the System 
  - Closed the Firewall from the Azure Network Security Group (NSG) and changed the RDP port 3389 setting. I switched it from Any (the whole internet) to My IP only. 
  - Ran a KQL check 15 minutes after the lockdown, and the attack count dropped to 0, proving the fix worked perfectly.
  - Turned off the VM and deallocated the VM in Azure so it would stop running and save my remaining student credits.

## What I learned
  - Defense in Depth: Learned that you should never trust just one firewall. Perimeter firewalls (Azure NSG) and host firewalls (Windows) need to work together to keep a system fully secure.
  - How to Read Logs: Got hands-on experience looking at raw Windows events, tracking down specific event numbers (4625), and figuring out rows like NOUSER.
  - SIEM & KQL Basics: Learned how to stop just passively looking at data and instead write custom KQL code to build real alert rules that catch bad actors automatically.
  - Connecting Data Streams: Learned how to take standard computer logs and combine them with an outside location spreadsheet to turn raw text into a helpful visual dashboard.


## Screenshots of Steps
**SIEM Incident Investigation Dashboard**<br>
This screenshot shows my custom alert rule firing on the left, and the raw log query window open on the right where I investigated the NOUSER attack that hit exactly 9 times.
<img width="955" height="454" alt="Sentinel_Incident_investigation" src="https://github.com/user-attachments/assets/b6c4d560-db81-4752-b195-21379d13d7c8" />

**Global Attack Map**<br>
The visual map workbook showing the huge red circles over Eastern Europe where the automated botnets were blasting our open RDP port.
<img width="614" height="341" alt="Geolocation of Attackers" src="https://github.com/user-attachments/assets/dc2acb9e-ca3b-4840-9fba-c13ed77023b4" />

**Post-Remediation Check**<br>
Proof that our lockdown worked, showing that the failed login attempts completely flatlined to zero once the firewall rule was limited to my personal IP.
<img width="953" height="455" alt="Post-Remediation" src="https://github.com/user-attachments/assets/42a736ae-2de8-4c2f-9985-4978107530db" />

## Scope & Notes

This project was performed in a personal Microsoft Azure lab for learning purposes. The environment was intentionally exposed for a limited period to observe and analyze real-world automated attack traffic. It represents hands-on practice with SIEM monitoring, log analysis, KQL, detection engineering, and basic incident investigation—not production or enterprise SOC experience.

