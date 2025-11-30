# 🛡️ SOC Analyst Home Lab Portfolio

## 📌 Executive Summary
This repository documents the construction and operation of a virtual Security Operations Center (SOC). The goal was to simulate a corporate environment to execute attacks, analyze logs, and develop incident response playbooks. 

As a transitioning professional with a background in Operations Management, this lab serves as a practical application of Incident Response (IR) lifecycle management, system hardening, and telemetry analysis.

## 🛠️ Technical Architecture
**Host System:** Ryzen 7 5800H | 22GB RAM | Windows 11 Pro
**Hypervisor:** VMware Workstation Pro (Network: NAT Subnet `192.168.50.0/24`)

### Infrastructure Components
| Role | OS | IP Address | Function |
|------|----|------------|----------|
| **SIEM Server** | Ubuntu Server 22.04 | `192.168.50.130` | Hosts Wazuh Manager, Indexer & Dashboard |
| **Endpoint** | Windows 11 Ent. | `DHCP` | Victim workstation with Sysmon & Wazuh Agent |
| **Attacker** | Kali Linux | `DHCP` | Red Team operations (Hydra, Nmap) |

## 📂 Project Modules

### [1. SIEM Architecture & Deployment](1-SIEM-Implementation.md)
* **Objective:** Deployed Wazuh (XDR/SIEM) and onboarded a Windows 11 endpoint.
* **Skills:** Linux Administration, Agent Deployment, Configuration Management.

### [2. Threat Emulation (RDP Brute Force)](2-RDP-Brute-Force.md)
* **Objective:** Simulated a credential access attack using Hydra (Kali Linux).
* **Skills:** Red Teaming, MITRE ATT&CK Mapping (T1110), Log Analysis.

### [3. Network Forensics (Wireshark)](3-Network-Forensics.md)
* **Objective:** Captured and analyzed live attack traffic to identify patterns.
* **Skills:** Packet Analysis (PCAP), Protocol Filtering, Traffic Signature ID.

### [4. Endpoint Forensics (Fileless Malware)](4-Endpoint-Forensics.md)
* **Objective:** Detected obfuscated PowerShell scripts using Sysmon & Wazuh.
* **Skills:** Threat Hunting, Base64 Decoding, Living-off-the-Land (LotL) detection.

### [5. Phishing Analysis](5-Phishing-Analysis.md)
* **Objective:** Analyzed email headers to identify Typosquatting and Source IP reputation.
* **Skills:** OSINT, Email Forensics, Whois/GeoIP lookup.

### [6. SOC Automation (Python)](6-SOC-Automation.md)
* **Objective:** Scripted a custom tool to interact with the SIEM API and auto-flag anomalies.
* **Skills:** Python Scripting, API Integration, SOAR concepts.

---
*Disclaimer: All attacks were simulated in a controlled, isolated environment.*
