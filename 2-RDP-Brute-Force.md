# ⚔️ Project 2: Threat Emulation (RDP Brute Force)

## 📝 Project Description
In this module, I shifted roles to "Red Teaming" to simulate a Credential Access attack against the Windows 11 Endpoint. The objective was to validate that the SIEM (Wazuh) could ingest, parse, and visualize an active brute-force attempt via Remote Desktop Protocol (RDP).

## 🔧 Technologies & Utilities
* **Attacker:** Kali Linux (Hydra)
* **Victim:** Windows 11 Enterprise (RDP Enabled)
* **SIEM:** Wazuh Manager (Log Analysis)
* **Protocol:** RDP (Port 3389)

## 🗺️ MITRE ATT&CK Mapping
This project simulates the following adversary tactics:
* **Technique:** [T1110.001](https://attack.mitre.org/techniques/T1110/001/) (Brute Force: Password Guessing)
* **Tactic:** TA0006 (Credential Access)

## 🚀 Execution Steps

### 1. Environment Staging
I prepared the attack surface by enabling RDP on the Windows victim and disabling Network Level Authentication (NLA) to allow Hydra to interact with the login prompt directly.
* **Attacker IP:** `192.168.50.131`
* **Victim IP:** `192.168.50.129`

### 2. Weaponization (Custom Wordlist)
To simulate a targeted attack, I created a custom password list (`passlist.txt`) containing common weak passwords and the actual correct password to verify the success trigger.

### 3. The Attack (Hydra)
I utilized **Hydra**, a parallelized login cracker, to launch the attack against the Victim's RDP service.

```bash
# Syntax: hydra -l [User] -P [Wordlist] [Protocol]://[Target_IP]
hydra -l Victim -P passlist.txt rdp://192.168.50.129 -V
```
## Observations:

Hydra attempted multiple logins rapidly.

Successful cracked credential returned: login: Victim password: Password123

### 4. Detection & Analysis (Wazuh)
##Immediately following the attack, I pivoted to the Wazuh Dashboard to verify telemetry.

Event ID 4625: Multiple failures detected (Reason: "Unknown user name or bad password").



Event ID 4624: One successful login detected immediately following the failures.

### 📸 Proof of Concept
Evidence A: The Attack (Kali Terminal)
(Screenshot showing Hydra successfully finding the password)
<img width="862" height="694" alt="Screenshot 2025-11-22 135419" src="https://github.com/user-attachments/assets/3eb19876-2250-4489-b3a5-609d59112414" />

Evidence B: The Detection (Wazuh Dashboard)
(Screenshot showing the spike in Authentication Failures)
<img width="1900" height="978" alt="Screenshot 2025-11-22 135600" src="https://github.com/user-attachments/assets/de50d69c-cf2b-4a10-92a6-ffbbf317f79f" />

### 🧠 Lessons Learned
Noise vs. Signal: A real brute force attack generates massive amounts of logs (Event ID 4625) in seconds. A single "4624" (Success) after a string of "4625" (Failures) is a high-fidelity Indicator of Compromise (IoC).

RDP Risks: Exposing RDP directly without VPNs or rate-limiting makes endpoints highly vulnerable to automated dictionary attacks.
