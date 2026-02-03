## **🔐 SOC Automated Detection Lab**

**SIEM-Based Endpoint Threat Detection using Splunk & Sysmon**

**📌 Project Overview**

This project demonstrates the design and implementation of a Security Operations Center (SOC) detection lab using Splunk Enterprise as a SIEM and Sysmon for high-fidelity Windows endpoint telemetry.

The lab focuses on log ingestion, threat detection, investigation workflows, and SOC dashboards, following real-world SOC practices.

⚠️ Alert automation and SOAR are intentionally excluded due to Splunk Free license limitations. Detection logic and investigation workflows are emphasized instead.

**🧱 Architecture**
``
Windows Endpoint
 └── Sysmon (Endpoint Telemetry)
       └── Splunk Universal Forwarder
             └── Splunk Enterprise (SIEM)
                   ├── Detection Searches
                   ├── Investigation Queries
                   └── SOC Dashboards
 ``


**🛠 Tools & Technologies**

Splunk Enterprise (Free)

Splunk Universal Forwarder

Sysmon (System Monitor)

Windows 10 / 11

PowerShell

SPL (Search Processing Language)

**🎯 Objectives**

Collect detailed Windows endpoint telemetry

Detect suspicious behavior using Sysmon

Perform SOC-style threat investigations

Build dashboards for continuous monitoring

Simulate real SOC analyst workflows

**⚙️ Lab Setup**

1️⃣ Splunk Enterprise (SIEM)

Installed on a Linux VM

Configured to receive logs on port 9997

Web interface accessed via http://<splunk-ip>:8000

2️⃣ Windows Endpoint Configuration
Sysmon Installation
Sysmon64.exe -accepteula -i sysmonconfig.xml


Sysmon Operational logs enabled

Verified via Event Viewer:

Applications and Services Logs
 └─ Microsoft
    └─ Windows
       └─ Sysmon
          └─ Operational

Splunk Universal Forwarder

Installed on Windows endpoint

Configured to forward logs to Splunk indexer

Service runs as LocalSystem (required for Sysmon access)

sc.exe config SplunkForwarder obj= LocalSystem
Restart-Service SplunkForwarder

**📥 Data Sources Ingested**

Sysmon Operational Logs

Windows Security Logs

Verified using:

index=main source="WinEventLog:Microsoft-Windows-Sysmon/Operational"

**🔍 Detection Use Cases**

🟥 Detection 1 — Suspicious PowerShell Execution
index=main EventCode=1 Image="*powershell.exe*"
| table _time host User Image CommandLine ParentImage


Detects:

Living-off-the-land attacks

Script-based execution

Fileless malware activity

🟥 Detection 2 — Credential Dumping (LSASS Access)
index=main EventCode=10 TargetImage="*lsass.exe*"
| table _time host SourceImage TargetImage GrantedAccess


Detects:

Credential dumping attempts

Mimikatz-style behavior

🟥 Detection 3 — Persistence via Registry Run Keys
index=main EventCode=13 TargetObject="*Run*"
| table _time host Image TargetObject Details


Detects:

Startup persistence techniques

**🧪 Investigation Workflow**

SOC-style investigation steps followed:

Identify detection hit

Expand timeline for the host

Analyze process lineage

Check network connections

Assess persistence mechanisms

Classify incident severity

**📊 SOC Dashboard***

A custom dashboard was created containing:

PowerShell abuse detections

LSASS access attempts

Registry persistence events

Sysmon event volume overview

This dashboard acts as a SOC analyst monitoring console.

**🚫 Alerting & SOAR**

Splunk Free license does not support alerts

Detection logic is validated via:

Manual threat hunting

Dashboards

Controlled attack simulations

This mirrors SOC environments where alert automation is limited or handled externally.

**🧠 Skills Demonstrated**

SIEM configuration & log ingestion

Endpoint telemetry analysis

Detection engineering (SPL)

SOC investigation workflows

Threat hunting

Security monitoring architecture

**📌 Future Enhancements**

Add Linux endpoint telemetry

Integrate SOAR for automated response

Map detections to MITRE ATT&CK

Add attack simulations (Atomic Red Team)

**👤 Author**

**Muhammed Shifan P N**
Master’s Student — Cybersecurity
SOC | SIEM | Threat Detection
