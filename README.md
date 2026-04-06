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
Focused on the deployment of advanced endpoint telemetry by installing Sysmon on Windows VM and the configuration of the data ingestion pipeline to achieve deep visibility into system-level activity.  

**Step 2.1: Install Sysmon -**  
Sysmon (System Monitor) is a Microsoft tool that provides deep-level telemetry (Process Creation, File Creation time changes, Network Connection). I downloaded it from [Microsoft Sysinternals](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon).  

<img src="screenshots/sysmon_download.png" width="600">  

And then downloaded [SwiftOnSecurity's Config](https://github.com/SwiftOnSecurity/sysmon-config/blob/master/sysmonconfig-export.xml) as filter so I don't overwhelm my Windows VM.  

<img src="screenshots/swiftonsecurity_config.png" width="600">  

Expanded the downloaded Sysmon zip file in a new directory and ran `.\Sysmon64.exe -i ..\sysmonconfig-export.xml -accepteula` to configure Sysmon with the downloaded configuration file.  

<img src="screenshots/running_sysmon_with_config.png" width="600">

**Step 2.2: Data Pipeline Configuration -**  
Now that Sysmon is recording data, I opened Notepad as Administrator and created `inputs.conf` file so that it can pick up the data from Splunk Universal Forwarder and send it to Splunk Cloud.  

<img src="screenshots/inputs.conf_file.png" width="600">  

After copying `inputs.conf` file to `C:\Program Files\SplunkUniversalForwarder\etc\system\local`. I restarted the Splunk Service.  

<img src="screenshots/restarting_uf.png" width="600">

**Step 2.3: - Visibility Verification**  
Verified the visibility of logs by using filter `index="main" | stats count by sourcetype` in Search & Reporting of Splunk Cloud.  

<img src="screenshots/endpoint_visibility_splunk_cloud.png" width="600">  

---
Phase 3: Detection Engineering & Data Visualization  
---
Transformed raw telemetry from the Windows Endpoint into actionable security intelligence by creating specific Detection Logic (Alerts) and High-Level Visibility (Dashboards).  

**Step 3.1: -**  
input  

<img src="screenshots/alert1.png" width="600">  

input  

<img src="screenshots/alert1_saveas_1.png" width="600">  
<img src="screenshots/alert1_saveas_2.png" width="600">  

input  

<img src="screenshots/alert2.png" width="600">  

input  

<img src="screenshots/alert3.png" width="600">  

input  

<img src="screenshots/alert4.png" width="600">  

input  

<img src="screenshots/alert5.png" width="600">  

input  

<img src="screenshots/all_alerts.png" width="600">  

**Step 3.2: -**  
input  

<img src="screenshots/create_dashboard.png" width="600">  

input  

<img src="screenshots/v1_chart.png" width="600">  

input  

<img src="screenshots/v1_source.png" width="600">  

input  

<img src="screenshots/v1_coloring.png" width="600">  

input  

<img src="screenshots/v2_source.png" width="600">  

input  

<img src="screenshots/v3_source.png" width="600">  

input  

<img src="screenshots/final_dashboard.png" width="600">  

---
Phase 4: Attack Simulation  
---

