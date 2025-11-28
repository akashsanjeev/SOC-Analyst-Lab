# 📧 Project 5: Phishing Campaign Analysis (Email Forensics)

## 📝 Project Description
In this module, I analyzed a suspicious email artifact reported by a user. The goal was to inspect the email headers to identify spoofing attempts, malicious "Reply-To" addresses, and the true geographical origin of the sender.

## 🔧 Tools & Techniques
* **Artifact:** Raw Email Headers (RFC 5322)
* **OS:** Kali Linux
* **Tools:** `whois`, `curl ipinfo.io`, Manual Header Parsing
* **Technique:** Typosquatting detection & Source IP Geolocation

## 🚀 Investigation Steps

### 1. Header Extraction
I isolated the raw headers from the suspicious email to identify the traversal path.
* **Subject:** ACTION REQUIRED: Your account has been compromised
* **Claimed Sender:** Microsoft Security Team

### 2. Artifact Analysis (The Red Flags)
Upon reviewing the headers, I identified three critical Indicators of Compromise (IoC):

* **IoC 1 (Spoofing):**
    * `From:` support@microsoft.com
    * `Return-Path:` admin@vps-3392.cloud-hosting.net
    * **Analysis:** The Return-Path (technical sender) does not match the From address. Microsoft does not use generic "cloud-hosting" domains.

* **IoC 2 (Typosquatting):**
    * `Reply-To:` urgent-help@micro-soft-security-update.com
    * **Analysis:** The domain contains a hyphen (`micro-soft`) intended to trick the user. This is a classic typosquatting attack.

* **IoC 3 (Source IP Reputation):**
    * `X-Originating-IP:` 89.116.27.189
    * **Analysis:** I performed a WHOIS lookup on the Source IP.

### 3. Geolocation & Attribution
I utilized the Kali Linux terminal to query the IP registration data.
```bash
curl ipinfo.io/89.116.27.189
```
### 📸 Forensic Evidence
(Screenshot showing the WHOIS/IP lookup confirming the non-Microsoft origin)
<img width="1019" height="718" alt="Screenshot 2025-11-28 181625" src="https://github.com/user-attachments/assets/4aa26bed-94c5-4885-93b4-9ada62542b9a" />

### 🧠 Lessons Learned
Trust but Verify: The "From" address is essentially a sticker that anyone can fake. The "Return-Path" and "Received" headers tell the true story.

Infrastructure Analysis: Phishing campaigns often utilize cheap VPS infrastructure (DigitalOcean, Hostinger, etc.) rather than compromised corporate servers.
