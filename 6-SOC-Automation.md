# 🤖 Project 6: SOC Automation (Python & API Integration)

## 📝 Project Description
In the final phase of the lab, I developed a custom Python script to interact with the Wazuh SIEM API. The goal was to simulate **SOAR (Security Orchestration, Automation, and Response)** capabilities by programmatically fetching alerts and applying logic to detect anomalies (Brute Force Thresholds) without human intervention.

## 🔧 Technologies Used
* **Language:** Python 3.x
* **Library:** `requests` (REST API Communication)
* **Target:** Wazuh Manager API (Port 55000)
* **Concept:** Automated Threat Hunting

## 🚀 Script Logic

### 1. API Authentication
The script authenticates to the Wazuh Manager using a `GET` request to `/security/user/authenticate` to retrieve a temporary JWT (JSON Web Token) for session access.

### 2. Data Ingestion
It queries the `/security/events` endpoint to pull the latest JSON logs from the SIEM database.

### 3. Anomaly Logic (The "Brain")
The script iterates through the logs looking for specific Indicators of Compromise (IoC), specifically Windows Event ID 4625 (Logon Failures).
* **Threshold:** If > 5 failures are detected in the batch.
* **Action:** Trigger a "CRITICAL ANOMALY" alert in the console.

## 💻 The Code Snippet

```python
def anomaly_detection(alerts):
    fail_count = 0
    for event in alerts:
        # Check for Logon Failure description
        if "Logon failure" in event.get('rule', {}).get('description', ''):
            fail_count += 1
    
    # Threshold Logic
    if fail_count >= 5:
        print("🚨 CRITICAL ANOMALY DETECTED: Potential Brute Force Attack!")
```
📸 Execution Evidence
(Screenshot showing the Python script connecting to the SIEM and flagging the Brute Force attack)
<img width="1192" height="767" alt="Screenshot 2025-11-30 143555" src="https://github.com/user-attachments/assets/9516a4b8-f3da-4e10-8df3-ffa6fc93f02d" />
### 🧠 Lessons Learned
API Power: Accessing the SIEM via API allows for custom reporting and automation that goes beyond the standard Dashboard GUI.

Automation Value: By scripting the detection logic, I reduced the "Time to Detect" (TTD) for high-volume attacks, freeing up analyst time for complex investigations.
