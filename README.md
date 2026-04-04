# SIEM-and-Threat-Detection-Lab

This lab demonstrates end-to-end proficiency in threat detection and SOC analyst workflows. The core objective was to build a functional SIEM-based detection environment and validate it against real attack simulations. This involved:  

1. Setting up a two-VM isolated network environment (Kali Linux and Windows 11 ARM) with Sysmon capturing deep host telemetry across all critical event categories.  
2. Configuring Splunk Cloud to ingest Windows log sources and deploying custom correlation rules mapped to the MITRE ATT&CK framework.  
3. Simulating targeted attack chains (e.g., credential dumping, persistence via scheduled tasks, PowerShell execution) using Atomic Red Team to generate real security alerts.  
4. Performing SOC triage by analyzing Splunk detections against raw Sysmon and Windows Event Log evidence, confirming True Positives and documenting findings in a formal Incident Report.


---
Phase 1: Environment Setup & Network Preparation  
---
Established a secure, isolated virtual network where the Kali Linux (Attacker) can interact with the Windows 11 (Victim/SIEM) host using static configurations to ensure reliable log indexing.  

**Step 1.1: VM IP Address Confirmation -**  
To ensure both Kali and Windows VM were on the same subnet, I switched the Network Adapter Settings to Host-Only. Then executed command `ifconfig` on Kali and `ipconfig` on Windows to confirm the same.  

<img src="screenshots/kali_ifconfig.png" width="600">
<img src="screenshots/windows_ipconfig.png" width="600">

**Step 1.2: Connectivity & ICMP Handshake -**  
By default, Windows 11 ARM blocks pings. So had to open a specific hole in firewall using command `New-NetFirewallRule -DisplayName "Allow ICMPv4-In" -Direction Inbound -Protocol ICMPv4 -IcmpType 8 -Action Allow` to allow Kali VM to see Windows VM, which was later verified by running `ping 172.16.36.130 -c 4`

<img src="screenshots/connectivity_windows.png" width="600">
<img src="screenshots/connectivity_kali.png" width="600">

**Step 1.3.1: Splunk Cloud SIEM Provisioning -**  
Signed up for Splunk Cloud Free Trial and got an assigned Splunk Cloud Admin account with credentials on email.  

<img src="screenshots/splunk_cloud.png" width="600">  

**Step 1.3.2: Universal Forwarder for Windows -**  
Downloaded Universal Forwarder (UF) on Windows VM that speaks its language (ARM).  

<img src="screenshots/uf_download.png" width="600">  

**Step 1.3.3: Credentials for Cloud-Windows connection -**  
Downloaded Universal Forwarder Credentials so the Cloud knows to trust my Windows VM.  

<img src="screenshots/uf_credentials.png" width="600">  

**Step 1.3.4: Linking Windows to Splunk Cloud -**  
Opened Powershell as Administrator in Windows and ran command `cd C:\Program Files\SplunkUniversalForwarder\bin` to navigate to the Splunk bin folder, then ran `.\splunk install app C:\Users\YourName\Desktop\splunkclouduf.spl`  to install the cloud credentials and finally `.\splunk restart` to restart the forwarder.  

<img src="screenshots/linking_splunk_cloud_to_windows.png" width="600">  

---
Phase 2: Data Ingestion & Endpoint Visibility  
---
Installed Sysmon on Windows and made a custom configuration file that connects back to Splunk Cloud with the help of credentials.  

**Step 2.1: Sysmon Installation -**  
input
