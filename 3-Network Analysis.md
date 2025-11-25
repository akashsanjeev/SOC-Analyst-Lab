---

## 🦈 Project 3: Network Forensics (Wireshark Analysis)

### 📖 Objective
While the SIEM (Project 2) provided application-level logs, I needed to validate the attack at the **network packet level**. The goal was to perform a packet capture (PCAP) to identify the specific traffic patterns of a Brute Force attack "on the wire."

### 🛠️ Tools & Configuration
- **Tool:** Wireshark (v4.x) with Npcap driver.
- **Probe Point:** Windows 11 Victim Interface (`Ethernet0`).
- **Filter Applied:** `tcp.port == 3389` (Remote Desktop Protocol).

### 🧪 Execution Steps
1.  **Traffic Baseline:** Initiated a packet capture on the Victim machine to record ingress traffic.
2.  **Attack Simulation:** Re-executed the Hydra Brute Force script from the Kali Attacker (`192.168.50.131`).
3.  **Capture & Filter:** Stopped the capture immediately after the password was cracked and filtered specifically for RDP traffic to remove background noise.

### 🕵️‍♂️ Analysis & Findings
The packet capture revealed distinct anomalies confirming the attack:
- **Source Identification:** A high volume of TCP packets originating from `192.168.50.131` (Attacker) targeting `192.168.50.129` (Victim).
- **Protocol Abuse:** The capture showed a rapid succession of **SYN > SYN-ACK > ACK** (TCP Handshakes) followed immediately by **RST** (Reset) or session termination packets.
- **Pattern Recognition:** Unlike a human user who establishes one persistent RDP session, the capture showed **8+ separate connection attempts in under 10 seconds**, mathematically confirming an automated script was in use.

**(Wireshark Packet Capture Evidence)**
![Wireshark Traffic Analysis]<img width="1718" height="892" alt="Network_Traffic_Analysis" src="https://github.com/user-attachments/assets/63f2b0df-87a5-4e2d-b469-1554925f8ac4" />
