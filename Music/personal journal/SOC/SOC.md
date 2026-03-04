# What Exactly Is a Log?

A **log** is a file (or database entry) that stores information about:
- Who did something
- What they did
- When they did it
- From where they did it
- Whether it succeeded or failed

Example:
```
[2026-03-05 14:22:01] User: harsh    
IP: 192.168.1.10    
Action: Login attempt    
Status: Failed  
```

---

## 🛡️ Why Logs Matter in Cybersecurity

Logs are EVERYTHING in security. Without logs, you’re blind.

They help with:

### 1️⃣ Detecting Attacks

If someone is brute-forcing passwords, logs will show:

- Multiple failed login attempts
    
- Same IP trying many times
    

### 2️⃣ Investigating Incidents

After a breach, security teams analyze logs to answer:

- How did the attacker get in?
    
- What data was accessed?
    
- Did they escalate privileges?
    

### 3️⃣ Monitoring Suspicious Behavior

Example:

- Admin account logging in at 3AM from another country 👀
    
- A normal user suddenly accessing sensitive files
    

---

## 📂 Types of Logs
Here are common ones you’ll see in bug bounty and real-world security:
### 🔹 System Logs
OS-level events (Linux, Windows)
Example: `/var/log/auth.log` in Linux
### 🔹 Application Logs
Logs from web apps (PHP, Node.js, etc.)
Since you test web apps, these are important.
### 🔹 Web Server Logs
From servers like:
- Apache HTTP Server 
- Nginx
They record:
- IP
- URL accessed
- Status code (200, 404, 500)
- User agent
Super useful for:
- Detecting scanning
- Finding LFI/RFI attempts
- Spotting weird payloads
### 🔹 Firewall Logs
Show blocked/allowed traffic.
### 🔹 Authentication Logs
Login attempts, privilege changes, token generation.

<mark style="background: #FF5582A6;">read windows event logs</mark> <mark style="background: #FFB8EBA6;">- [[windows-event-log-analyst-reference.pdf]]</mark>

# Logs (2)
## Firewall Logs (Palo Alto Firewall)
- Logs analyzed are from **Palo Alto Next-Generation Firewall** which detects **threat logs** and **traffic logs**.
- **Threat logs** capture malicious activity such as URL threats, viruses, flood attacks, vulnerability scans, etc.
- Log fields include:
    - Received time
    - Serial number
    - Threat type (e.g., URL, file)
    - Source IP (private IPs: 10.x.x.x, 192.168.x.x)
    - Destination IP (public IPs)
    - Source and destination ports
    - Action taken (Alert, Block, Allow)
- Example:
    - Traffic from a private IP to a public IP is **outbound** traffic; NAT (Network Address Translation) converts private IPs and ports to public ones.
    - Ports involved can indicate type of traffic, e.g., 443 for HTTPS.
- **Traffic logs** differ from threat logs by showing allowed or denied connections rather than detected threats.
- **Understanding inbound vs outbound traffic** by analyzing IP ranges and ports is crucial.

## Antivirus (AV) Logs (Symantec Endpoint Protection)
- AV logs indicate detection of malware or suspicious files.
- Detection methods:
    - **Signature-based**: matching hash values of known malware.
    - **Behavior-based (heuristics)**: analyzing suspicious behavior of files without known signatures.
- Important log fields:
    - Hostname
    - Username
    - File hash
    - Malware type (Trojan, rootkit, etc.)
    - Action taken (Quarantine, Delete, Leave Alone)
- Analysts investigate alerts further if no immediate action is taken by AV, involving IT teams for remediation.

#### **Log Sources and SIEM Overview

- Logs come from various **log sources** such as Windows Event Viewer, firewalls, antivirus, web servers, and DNS servers.
- SIEM tools aggregate, parse, and analyze these logs to generate alerts.
- Understanding log structure and contents is essential for:
    - Writing effective alert queries (e.g., alert on outbound RDP traffic over port 3389).
    - Performing incident investigations.
- Logs are retained for audit and forensic purposes, commonly for a year or more, to comply with organizational and regulatory requirements.
