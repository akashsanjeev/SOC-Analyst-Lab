# 🕵️ Project 4: Endpoint Forensics (Fileless Malware Detection)

## 📝 Project Description
In this module, I simulated a "Living off the Land" (LotL) attack using PowerShell to execute a Fileless Malware technique. The goal was to bypass standard signature-based detection and validate that **Sysmon** (System Monitor) combined with **Wazuh** could detect the malicious behavior (Process Creation and Obfuscation).

## 🔧 Tools & Configuration
* **Endpoint:** Windows 11 Enterprise
* **Telemetry Source:** Microsoft Sysmon (Event ID 1: Process Creation)
* **SIEM:** Wazuh Manager (Log Analysis)
* **Attack Vector:** Obfuscated PowerShell (Base64 Encoded)

## 🗺️ MITRE ATT&CK Mapping
* **Technique:** [T1059.001](https://attack.mitre.org/techniques/T1059/001/) (Command and Scripting Interpreter: PowerShell)
* **Technique:** [T1027](https://attack.mitre.org/techniques/T1027/) (Obfuscated Files or Information)

## 🚀 Execution Steps

### 1. Telemetry Pipeline
I installed **Sysmon** on the victim endpoint to expand logging visibility beyond standard Windows Event Logs. I then configured the Wazuh Agent (`ossec.conf`) to ingest the `Microsoft-Windows-Sysmon/Operational` channel.
```xml
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```
### 2. Attack Emulation
I executed a Base64 encoded PowerShell command to simulate a hidden, fileless payload execution. This technique is commonly used to hide malicious scripts from human review.
```# Decoded intent: Start-Process calc.exe (Simulating a payload launch)
powershell.exe -NoP -NonI -W Hidden -Exec Bypass -Enc UwB0AGEAcgB0AC0AUAByAG8AYwBlAHMAcwAgAGMAdQBsAGMALgBlAHgAZQA=
```
### 3. Forensic Analysis
I utilized the Wazuh Dashboard to hunt for the specific execution.

Query: sysmon filtering for Event ID 1.

Detection: Located the powershell.exe process.

Indicators of Compromise (IoC):

-W Hidden: Attempt to hide the window from the user.

-Exec Bypass: Bypassing the local script execution policy.

-Enc: The presence of a long Base64 string indicates obfuscation.

## 📸 Forensic Evidence
(Screenshot of Wazuh detecting the obfuscated command line arguments)
<img width="1911" height="889" alt="Screenshot 2025-11-25 071145" src="https://github.com/user-attachments/assets/2a93ec36-f856-4e31-9441-fbb0040ef51b" />

### 🧠 Lessons Learned
Signature vs. Behavior: Traditional antivirus might miss this because powershell.exe is a legitimate tool. Only behavioral logging (Sysmon) capturing the arguments (-Enc ...) revealed the malicious intent.

Decoding is Mandatory: As an analyst, spotting Base64 (-Enc) is an immediate trigger to decode the string to understand the adversary's intent.
