# SOC-Analyst-Lab
A documentation of my home lab simulating a Security Operations Center (SOC) environment using Wazuh, Kali Linux, and Windows.
# 🛡️ SOC Analyst Home Lab

## 📌 Executive Summary
This repository documents the construction and operation of a virtual Security Operations Center (SOC). The goal is to simulate a corporate environment to simulate attacks, analyze logs, and develop incident response playbooks. 

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
1. **[COMPLETED] SIEM Deployment:** Automated deployment of Wazuh Manager & Agent configuration.
2. **[PLANNED] Attack Simulation:** SSH Brute Force & Detection.
3. **[PLANNED] Telemetry:** Network traffic analysis with Wireshark.
4. **[PLANNED] AI Integration:** Python-based log anomaly detection.

---
*Disclaimer: All attacks were simulated in a controlled, isolated environment.*
