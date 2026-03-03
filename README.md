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
yo
