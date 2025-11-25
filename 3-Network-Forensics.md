# 🔍 Project 3: Network Traffic Analysis (Forensics)

## 📝 Project Description
Following the RDP Brute Force attack in Project 2, I performed network forensics to capture and analyze the traffic artifacts. The goal was to identify the attack signature at the packet level and understand the limitations of network monitoring regarding encrypted protocols.

## 🔧 Tools Used
* **Packet Sniffer:** Wireshark v4.x
* **Protocol:** RDP (Remote Desktop Protocol) / TCP Port 3389
* **Filter Logic:** Source IP targeting and port specific filtering.

## 🚀 Investigation Steps

### 1. Traffic Capture
I initiated a live packet capture on the `Ethernet0` interface of the Windows 11 endpoint immediately prior to launching the Hydra attack from the Kali Linux machine.

### 2. Noise Filtering
To isolate the attack traffic from background system noise, I applied the following Wireshark display filter:
```wireshark
tcp.port == 3389 && ip.addr == 192.168.50.131
```
Logic: Display only packets traveling over Port 3389 (RDP) involving the Attacker IP (.131).

### 3. Pattern Analysis
Observation: The capture file showed a high-velocity pattern of TCP Handshakes (SYN, SYN-ACK, ACK) followed immediately by RST (Reset) or FIN (Finish) packets.

Significance: This "Machine Speed" traffic volume (10+ connections per second) confirms an automated dictionary attack rather than a human error.

### 4. Payload Inspection (The Encryption Limitation)
I attempted to view the payload content by using "Follow TCP Stream".

Result: The payload data was unreadable (garbled text).

Conclusion: RDP uses TLS encryption by default. While I could not see the password Password123 in cleartext on the wire, the volume and source of the traffic provided sufficient evidence of malicious intent.
### 📸 Forensic Evidence
(Screenshot of Wireshark showing the filtered RDP attack traffic from the Kali Source IP) 
<img width="1718" height="892" alt="Network_Traffic_Analysis" src="https://github.com/user-attachments/assets/fea5b9dd-c083-42b0-897d-867890243c59" />
### 🧠 Lessons Learned
Network Signature: Automated attacks have distinct "heartbeats" or volume signatures that differ from human behavior.

Encryption Blind Spots: Network tools (NIDS/Sniffers) cannot read encrypted payloads (RDP, HTTPS, SSH). This reinforces the need for Endpoint Logs (Project 2) to see what happened inside the session, while Network tools prove who connected and when.
