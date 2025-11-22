# 🏗️ Project 1: SIEM Architecture & Deployment

## 📝 Project Description
In this project, I architected and deployed a localized Security Operations Center (SOC) using **Wazuh**, an open-source XDR/SIEM solution. The objective was to establish centralized logging and monitoring for a heterogeneous environment (Linux Servers and Windows Endpoints).

## 🔧 Technologies Used
* **SIEM Core:** Wazuh Manager v4.9.2 (running on Ubuntu 22.04 LTS)
* **Endpoint:** Windows 11 Enterprise (simulated corporate workstation)
* **Virtualization:** VMware Workstation Pro (NAT Configuration)

## 🚀 Implementation Steps

### 1. Infrastructure Design
I configured a **NAT Network (Subnet 192.168.50.0/24)** to isolate the lab environment while allowing internet access for package updates.
* **SIEM Server IP:** `192.168.50.130` (Static Reserved)
* **Endpoint IP:** `DHCP`

### 2. Automated Deployment (Infrastructure as Code)
Utilized the Wazuh installation script for a streamlined deployment of the Manager, Indexer, and Dashboard components.

```bash
curl -sO [https://packages.wazuh.com/4.9/wazuh-install.sh](https://packages.wazuh.com/4.9/wazuh-install.sh) && sudo bash ./wazuh-install.sh -a
```
### 3. Troubleshooting & Optimization
During deployment, I encountered resource contention issues ("No space left on device") caused by RAM exhaustion during the Indexer installation.

Resolution: Increased VM RAM allocation to 8GB and performed a clean re-installation of the stack.

Authentication: Resolved a password synchronization issue between the Indexer and Dashboard by using the wazuh-passwords-tool.sh utility to force a credential reset.

### 4. Agent Configuration (Windows)
Deployed the Wazuh Agent to the Windows 11 endpoint using PowerShell for silent installation and immediate registration.
```
.\wazuh-agent.msi /q WAZUH_MANAGER='192.168.50.130'
NET START WazuhSvc
```
### 📸 Proof of Concept
##(See image below verifying Active Agent connectivity)
<img width="1910" height="1002" alt="wazuh-dashboard-proof" src="https://github.com/user-attachments/assets/46a7b2e1-c997-4ea3-81c9-c606e168b34f" />

### 🧠 Lessons Learned
Version Compatibility: Learned the strict requirement that Agent versions must match or be older than the Manager version (downgraded Agent from 4.14 to 4.9.2).

Service Management: Gained proficiency in Linux systemd commands (systemctl status/restart) to diagnose backend service failures.
