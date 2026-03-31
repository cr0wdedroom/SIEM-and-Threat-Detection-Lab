# SIEM-and-Threat-Detection-Lab

This lab demonstrates end-to-end proficiency in threat detection and SOC analyst workflows. The core objective was to build a functional SIEM-based detection environment and validate it against real attack simulations. This involved:  

1. Setting up a two-VM isolated network environment (Kali Linux and Windows 11 ARM) with Sysmon capturing deep host telemetry across all critical event categories.  
2. Configuring Splunk Enterprise to ingest 6+ Windows log sources and deploying custom correlation rules mapped to the MITRE ATT&CK framework.  
3. Simulating targeted attack chains (e.g., credential dumping, persistence via scheduled tasks, PowerShell execution) using Atomic Red Team to generate real security alerts.  
4. Performing SOC triage by analyzing Splunk detections against raw Sysmon and Windows Event Log evidence, confirming True Positives and documenting findings in a formal Incident Report.


---
Phase 1: Environment Setup & Network Preparation  
---
Established a secure, isolated virtual network where the Kali Linux (Attacker) can interact with the Windows 11 (Victim/SIEM) host using static configurations to ensure reliable log indexing.  

**Step1.1: VM IP Address Confirmation -**  
To ensure both Kali and Windows VM were on the same subnet, I switched the Network Adapter Settings to Host-Only. Then executed command `ifconfig` on Kali and `ipconfig` on Windows to confirm the same.  

<img src="screenshots/kali_ifconfig.png" width="600">
<img src="screenshots/windows_ipconfig.png" width="600">
<img src="screenshots/connectivity_windows.png" width="600">
<img src="screenshots/connectivity_kali.png" width="600">
<img src="screenshots/splunk_cloud.png" width="600">
<img src="screenshots/uf_download.png" width="600">
<img src="screenshots/uf_credentials.png" width="600">
<img src="screenshots/linking_splunk_cloud_to_windows.png" width="600">

