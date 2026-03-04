# 🛡️ Comprehensive Infrastructure & Network Penetration Testing Checklist  
*Designed for real-world engagements across Windows/Linux environments. Covers SMB, SSH, FTP, DNS, DHCP, LDAP, Kerberos, RDP, VNC, HTTP/HTTPS, databases, wireless, and social engineering.*

---

## 1. ✅ PRE-ENGAGEMENT ACTIVITIES

### Scope Definition
- [ ] Obtain written scope document including:
  - IP ranges, domains, hostnames, cloud assets
  - In-scope vs. out-of-scope systems/services
  - Authorized testing windows (timeframes)
  - Excluded tests (e.g., DoS, physical intrusion)
- [ ] Define target environment types: on-prem, cloud (AWS/Azure/GCP), hybrid
- [ ] Identify critical systems to avoid (e.g., production DBs, SCADA)

### Authorization & Legal
- [ ] Secure signed Rules of Engagement (RoE) and Statement of Work (SoW)
- [ ] Obtain explicit written authorization from asset owner(s)
- [ ] Verify legal compliance: GDPR, HIPAA, PCI-DSS if applicable
- [ ] Establish emergency contact for incident response

### Rules of Engagement (RoE)
- [ ] Define allowed techniques: black-box, gray-box, white-box
- [ ] Specify credential usage (if any): provided accounts, password spraying limits
- [ ] Set rate limits/throttling to avoid service disruption
- [ ] Define data handling procedures: encryption, retention, destruction
- [ ] Agree on communication protocol: encrypted channels, daily status updates

### Tools & Environment Setup
- [ ] Prepare isolated testing VM (Kali Linux, Parrot OS, or custom)
- [ ] Install/update core tools: Nmap, Metasploit, Burp Suite, CrackMapExec, Impacket, Responder, etc.
- [ ] Configure logging: record all commands, screenshots, tool outputs
- [ ] Set up secure storage for findings (encrypted drive or vault)

> **Best Practice**: Always validate scope with client before launching active scans. Never assume.

---

## 2. 🔍 INFORMATION GATHERING (RECON & ENUMERATION)

### Passive Reconnaissance
- [ ] Harvest domain info via WHOIS, DNSdumpster, SecurityTrails, Censys, Shodan
- [ ] Enumerate subdomains: Sublist3r, Amass, AssetFinder, Knockpy
- [ ] Scrape LinkedIn, GitHub, job postings for employee names, tech stack, internal tools
- [ ] Check certificate transparency logs (crt.sh) for hidden subdomains

### Active Reconnaissance
- [ ] Ping sweep: `fping -g 192.168.1.0/24`, `nmap -sn`
- [ ] Port scanning:
  - TCP SYN scan: `nmap -sS -p- -T4 -oA tcp_full`
  - UDP scan: `nmap -sU --top-ports 100 -oA udp_top`
  - Service/version detection: `nmap -sV -sC -O`
- [ ] Banner grabbing: Netcat, Telnet, `curl -v`, `openssl s_client`

### Protocol-Specific Enumeration

#### SMB (Ports 139/445)
- [ ] Enum shares: `smbclient -L //<IP> -N`, `crackmapexec smb <IP> --shares`
- [ ] Null session check: `rpcclient -U "" -N <IP>`
- [ ] User enum: `enum4linux-ng`, `GetNPUsers.py` (Impacket)
- [ ] OS/arch detection via SMB signing, dialects

> **Tools**: CrackMapExec, enum4linux-ng, smbmap, Impacket scripts  
> **Vulns**: Null sessions, anonymous share access, outdated SMB versions (SMBv1), MS17-010

#### SSH (Port 22)
- [ ] Banner grab for version: `ssh -v user@host`
- [ ] Check for weak ciphers/MACs: `nmap --script ssh2-enum-algos`
- [ ] Test for default/weak creds: Hydra, Medusa
- [ ] Enum users via timing attacks (if permitted): `python ssh_enum.py`

> **Vulns**: Weak algorithms (CBC mode, diffie-hellman-group1-sha1), default creds, exposed keys

#### FTP (Port 21)
- [ ] Anonymous login test: `ftp <IP>`, user: anonymous, pass: anything
- [ ] Directory traversal checks
- [ ] File upload/download permissions abuse
- [ ] Banner/version fingerprinting

> **Tools**: ftp, lftp, nmap scripts (`ftp-anon`, `ftp-brute`)  
> **Vulns**: Anonymous write access, cleartext credentials, directory listing enabled

#### DNS (Port 53)
- [ ] Zone transfer attempt: `dig axfr @<DNS_SERVER> <DOMAIN>`
- [ ] Reverse lookup sweeps: `dnsrecon -r 192.168.1.0/24 -n <DNS>`
- [ ] Subdomain brute-force: `gobuster dns -d domain.com -w wordlist.txt`
- [ ] Cache snooping, DNS poisoning prep

> **Tools**: dig, dnsrecon, dnsenum, fierce  
> **Vulns**: Zone transfers allowed, dynamic updates enabled, cache poisoning vectors

#### DHCP (Port 67/68)
- [ ] Spoof DHCP server to capture client requests (if in scope): `yersinia -G`
- [ ] Analyze lease times, gateway/DNS assignments for MITM potential
- [ ] MAC address cloning for impersonation

> **Caution**: DHCP attacks can disrupt network — only with explicit permission.

#### LDAP (Port 389/636)
- [ ] Anonymous bind test: `ldapsearch -x -h <IP> -b "" -s base`
- [ ] Enum users/groups: `ldapsearch -x -h <IP> -b "dc=domain,dc=com"`
- [ ] Extract password policy, lockout thresholds
- [ ] Check for LDAPS misconfigurations

> **Tools**: ldapsearch, BloodHound (for AD), WindapSearch  
> **Vulns**: Anonymous binds, excessive permissions, cleartext binds over 389

#### Kerberos (Port 88)
- [ ] AS-REP Roasting: `GetNPUsers.py domain/ -usersfile users.txt -format hashcat`
- [ ] SPN scanning for Kerberoasting: `GetUserSPNs.py domain/user:pass -request`
- [ ] Golden/Silver Ticket prep: extract krbtgt hash, service account hashes

> **Tools**: Impacket suite, Rubeus (Windows), BloodHound  
> **Vulns**: Pre-auth not required, weak service account passwords, unconstrained delegation

#### RDP (Port 3389)
- [ ] Check NLA requirement: `nmap -p 3389 --script rdp-ntlm-info`
- [ ] Brute-force with throttling: Hydra, Crowbar
- [ ] BlueKeep check (CVE-2019-0708): `nmap -p 3389 --script vulners --script-args mincvss=7.0`

> **Tools**: xfreerdp, rdesktop, NLA brute tools  
> **Vulns**: Weak creds, NLA disabled, CVE-2019-0708, CVE-2023-28275

#### VNC (Port 5900+)
- [ ] Banner grab + version check
- [ ] Default password attempts: “password”, “admin”, “vnc”
- [ ] Check for unauthenticated access

> **Tools**: vncviewer, metasploit auxiliary modules  
> **Vulns**: No auth, weak/default passwords, buffer overflows (UltraVNC, RealVNC)

#### HTTP/HTTPS (Ports 80/443/8080/etc.)
- [ ] Web server fingerprinting: Wappalyzer, WhatWeb, `nmap -sV --script=http-server-header`
- [ ] Directory brute-forcing: `gobuster dir -u https://target -w /wordlist -x php,html,txt`
- [ ] Virtual host discovery: `ffuf -w vhosts.txt -H "Host: FUZZ.domain.com" -u http://target`
- [ ] SSL/TLS config audit: `testssl.sh`, `sslyze`, `nmap --script ssl-enum-ciphers`
- [ ] CORS, HSTS, CSP header checks
- [ ] Spider entire site: Burp Suite, OWASP ZAP, wget recursive

> **Tools**: Burp Suite, Nikto, DirBuster, ffuf, curl, testssl.sh  
> **Vulns**: Directory listing, default pages, weak TLS, missing security headers, exposed .git/.svn

#### Database Services (MSSQL 1433, MySQL 3306, PostgreSQL 5432, Oracle 1521)
- [ ] Default/weak credential testing: `hydra -L users.txt -P passwords.txt mssql://<IP>`
- [ ] Version/banner enumeration
- [ ] MSSQL: `sqsh -S <IP> -U sa -P ''` → check xp_cmdshell
- [ ] MySQL: `mysql -h <IP> -u root -p` → check file_priv, INTO OUTFILE
- [ ] PostgreSQL: `\conninfo`, check pg_read_file(), COPY FROM PROGRAM

> **Tools**: sqsh, mysql-client, psql, Metasploit db modules, Nmap scripts  
> **Vulns**: Default creds, excessive privileges, command execution via SQLi, cleartext protocols

---

## 3. 🧨 VULNERABILITY ASSESSMENT & ANALYSIS

### Automated Scanning
- [ ] Run credentialed/uncredentialed Nessus/OpenVAS scans
- [ ] Web app scanner: Burp Pro Active Scan, Acunetix, Nikto
- [ ] Container/cloud misconfig: Trivy, ScoutSuite, Prowler (AWS), Azucar (Azure)

### Manual Validation
- [ ] Validate all high/critical automated findings manually
- [ ] Prioritize by exploitability, business impact, ease of remediation
- [ ] Map vulnerabilities to MITRE ATT&CK TTPs

### Common Critical Vulnerabilities to Hunt For
- [ ] Unpatched services: EternalBlue (MS17-010), ProxyShell, Log4j (CVE-2021-44228)
- [ ] Misconfigured services: anonymous SMB/LDAP/FTP, unrestricted NFS exports
- [ ] Weak crypto: SSLv3, TLS 1.0, RC4, DES, NULL ciphers
- [ ] Default credentials on routers, cameras, printers, IoT
- [ ] Exposed admin interfaces: Jenkins, Tomcat, Docker API, Kubernetes dashboard
- [ ] Path traversal, SSRF, XXE, SSTI in web apps

### OS-Specific Checks

#### Windows
- [ ] Missing patches: `wmic qfe list`
- [ ] Local Admin Password Solution (LAPS) misconfig
- [ ] Unquoted service paths: `wmic service get name,displayname,pathname,startmode`
- [ ] AlwaysInstallElevated registry keys
- [ ] Stored credentials in Credential Manager, GPP files

#### Linux
- [ ] World-writable configs/scripts: `find / -writable ! -user $(whoami) 2>/dev/null`
- [ ] SUID/SGID binaries: `find / -perm -4000 -o -perm -2000 2>/dev/null`
- [ ] Cron jobs writable by low-priv users
- [ ] Kernel exploits: `uname -a`, searchsploit, linux-exploit-suggester
- [ ] Docker socket exposed: `/var/run/docker.sock`

> **Tools**: WinPEAS/LinPEAS, Seatbelt, PowerUp.ps1, Sherlock.ps1, linux-smart-enumeration  
> **Best Practice**: Correlate vulns with asset criticality — prioritize domain controllers, database servers, jump boxes.

---

## 4. 💥 EXPLOITATION TECHNIQUES

### Service Exploitation

#### SMB
- [ ] EternalBlue (MS17-010): `exploit/windows/smb/ms17_010_eternalblue`
- [ ] Pass-the-Hash: `crackmapexec smb <IP> -u user -H <NTLM_HASH> -x 'whoami'`
- [ ] PSExec/WMI lateral movement: `psexec.py`, `wmiexec.py`

#### SSH
- [ ] Private key reuse: `ssh -i id_rsa user@host`
- [ ] Agent hijacking (if agent-forwarding enabled)
- [ ] Port forwarding abuse for pivoting

#### RDP
- [ ] Pass-the-Hash with xfreerdp: `xfreerdp /v:IP /u:DOMAIN\user /pth:HASH`
- [ ] BlueKeep RCE: `exploit/windows/rdp/cve_2019_0708_bluekeep_rce`

#### Web Apps
- [ ] SQL Injection → OS shell via xp_cmdshell, INTO OUTFILE, COPY TO PROGRAM
- [ ] File upload → webshell (PHP, ASPX, JSP)
- [ ] Deserialization (Java, .NET, Python Pickle) → RCE
- [ ] SSRF → AWS Metadata, internal port scanning, Redis RCE

#### Databases
- [ ] MSSQL xp_cmdshell enablement: `EXEC sp_configure 'show advanced options',1; RECONFIGURE; EXEC sp_configure 'xp_cmdshell',1; RECONFIGURE;`
- [ ] MySQL UDF injection → sys_exec()
- [ ] PostgreSQL COPY TO PROGRAM → command execution

#### Kerberos
- [ ] AS-REP Roast → crack hash → gain initial access
- [ ] Kerberoast → crack service account hash → escalate privileges
- [ ] Silver Ticket → forge service ticket for specific SPN
- [ ] Golden Ticket → forge TGT using krbtgt hash → domain persistence

> **Tools**: Metasploit, CrackMapExec, Impacket, sqlmap, ysoserial, Rubeus, Mimikatz  
> **Best Practice**: Use staged payloads (meterpreter) for stability. Avoid noisy mass exploitation.

---

## 5. 🚀 POST-EXPLOITATION

### Privilege Escalation

#### Windows
- [ ] Token impersonation: Incognito module, PrintSpoofer, RoguePotato
- [ ] UAC bypass: fodhelper, cmstp, eventvwr
- [ ] DLL hijacking, service executable replacement
- [ ] Extract plaintext creds: Mimikatz `sekurlsa::logonpasswords`, LSASS dump
- [ ] Bypass AMSI/ETW for PowerShell/C# payloads

#### Linux
- [ ] Sudo misconfig: `sudo -l`, GTFOBins
- [ ] PATH hijacking, cron script overwrite
- [ ] Docker breakout: `docker run -v /:/host alpine chroot /host`
- [ ] Kernel exploits: Dirty Pipe, Dirty Cow, OverlayFS
- [ ] Reuse SSH agent socket or authorized_keys

> **Tools**: WinPEAS, LinPEAS, Sherlock, linux-exploit-suggester, gtfobins, lolbas

### Lateral Movement
- [ ] Pass-the-Hash/Ticket across domain
- [ ] Remote service execution: WMI, PSExec, Scheduled Tasks, WinRM
- [ ] SSH key reuse across Linux hosts
- [ ] RDP session hijacking (tscon.exe)
- [ ] Exploit trust relationships: constrained/unconstrained delegation, resource-based constrained delegation (RBCD)

> **Tools**: BloodHound (analyze attack paths), CrackMapExec, Impacket, Rubeus, Evil-WinRM

### Persistence
- [ ] Windows: Registry run keys, scheduled tasks, services, WMI event subscriptions, AppCert DLLs
- [ ] Linux: cron jobs, systemd services, .bashrc, SSH authorized_keys, LD_PRELOAD
- [ ] Web: backdoor in index.php, .htaccess, WordPress plugin
- [ ] Domain: Golden Ticket, DCSync rights, malicious GPO, SID History abuse

> **Best Practice**: Use multiple low-noise persistence mechanisms. Clean up after engagement.

---

## 6. 📶 WIRELESS NETWORK TESTING

### Recon & Discovery
- [ ] Identify SSIDs, channels, encryption types: `airodump-ng wlan0mon`
- [ ] Client association tracking
- [ ] Hidden SSID detection

### Attacks (If In-Scope & Physically Present)
- [ ] WPA/WPA2 PSK cracking:
  - Capture handshake: `airodump-ng -c <ch> --bssid <AP> -w capture wlan0mon`
  - Crack: `hashcat -m 2500 capture.hccapx rockyou.txt`
- [ ] WPS PIN brute-force: `reaver -i wlan0mon -b <BSSID> -vv`
- [ ] Evil Twin AP: `airbase-ng`, captive portal phishing
- [ ] Karma attack (rogue AP responding to ANY probe)
- [ ] Deauth flood (only if explicitly permitted): `aireplay-ng -0 5 -a <AP> -c <CLIENT>`

### Enterprise Wireless (802.1X)
- [ ] EAP downgrade attacks
- [ ] PEAP/MSCHAPv2 relay with eaphammer or hostapd-wpe
- [ ] Certificate validation bypass checks

> **Tools**: Aircrack-ng suite, Kismet, Wireshark, eaphammer, hostapd-wpe  
> **Best Practice**: Always obtain explicit written permission. Wireless attacks are highly visible and disruptive.

---

## 7. 🎭 SOCIAL ENGINEERING (IF APPLICABLE & AUTHORIZED)

### Phishing Campaigns
- [ ] Clone OWA, VPN, HR portals with GoPhish, SET
- [ ] Weaponized attachments: macro-enabled docs, ISO, LNK, HTML smuggling
- [ ] Credential harvesting + MFA bypass (token theft, reverse proxy like Evilginx2)

### Physical & Phone-Based
- [ ] USB drop attack (if permitted): preloaded Rubber Ducky, Bash Bunny
- [ ] Vishing: impersonate IT/helpdesk to extract creds or trigger actions
- [ ] Tailgating into facility (requires separate physical pen test agreement)

### Defense Evasion
- [ ] Obfuscate payloads: base64, XOR, AES, process hollowing
- [ ] Living-off-the-Land (LOLBAS/LOLScripts): certutil, bitsadmin, regsvr32, mshta
- [ ] Disable logging where possible (Clear Event Logs, disable PowerShell transcription)

> **Tools**: GoPhish, SET, Evilginx2, Modlishka, Canarytokens, MacroPack  
> **Best Practice**: Simulate real attacker TTPs. Report click rates, credential submissions, payload executions.

---

## 8. 📄 REPORTING & DOCUMENTATION

### Executive Summary
- [ ] Business risk overview (non-technical)
- [ ] Critical findings with business impact
- [ ] Remediation priority matrix (High/Medium/Low)

### Technical Findings (Per Vulnerability)
- [ ] Title, CVE/CWE ID (if applicable)
- [ ] Affected host(s)/service(s)
- [ ] Proof of Concept (screenshots, command output, video)
- [ ] Steps to reproduce
- [ ] Risk rating (CVSS 3.1 score + context)
- [ ] Detailed remediation steps (config snippets, patch links, hardening guides)

### Attack Narrative / Kill Chain
- [ ] Timeline of compromise: Initial Access → Execution → Persistence → PrivEsc → Lateral → Exfil
- [ ] MITRE ATT&CK mapping for each phase
- [ ] Indicators of Compromise (IOCs) for blue team

### Appendices
- [ ] Tool commands used (with parameters)
- [ ] Raw scan outputs (Nmap, Nessus, etc.)
- [ ] Credentials tested (redacted)
- [ ] Scope document & RoE (attached)

### Delivery & Presentation
- [ ] Deliver encrypted PDF + password-protected archive of raw data
- [ ] Schedule debrief meeting with technical + management teams
- [ ] Offer retesting window (e.g., 30 days post-remediation)

> **Best Practices**:
> - Never include live credentials or sensitive data in report.
> - Use consistent severity ratings (e.g., DREAD, CVSS).
> - Recommend compensating controls if patching isn’t immediate.
> - Include positive findings (“secure configurations observed”).

---

## 🔄 FINAL CHECKS BEFORE SIGN-OFF

- [ ] All tools/logs securely wiped from tester machine
- [ ] Temporary accounts/persistence mechanisms removed
- [ ] Client confirms receipt of report and understands findings
- [ ] Lessons learned documented internally for team improvement
- [ ] Retest plan agreed upon (if applicable)

---

✅ **You now have a battle-tested, comprehensive infrastructure & network penetration testing checklist. Adapt per engagement, stay ethical, document everything.**

> *“The only secure system is one that’s powered off, cast in a block of concrete, and sealed in a lead-lined room... but even then, I have my doubts.” — Gene Spafford*

🔐 Happy Hacking — Responsibly.