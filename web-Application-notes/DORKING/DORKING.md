## 🔍 **Reconnaissance**

### Subdomain Enumeration

Bash

```bash
# Subfinder
subfinder -d target.com -o subdomains.txt

# Amass
amass enum -d target.com -o amass_results.txt

# Assetfinder
assetfinder --subs-only target.com | tee assetfinder.txt

# Findomain
findomain -t target.com -o
```

### DNS Enumeration

Bash

```bash
# DNSRecon
dnsrecon -d target.com

# Fierce
fierce --domain target.com

# dig commands
dig target.com ANY
dig +short target.com
dig target.com MX
dig target.com TXT
```

---

## 🌐 **HTTP Probing & Live Host Detection**

Bash

```bash
# httpx
cat subdomains.txt | httpx -o live_hosts.txt

# httprobe
cat subdomains.txt | httprobe | tee alive.txt

# With status codes
cat subdomains.txt | httpx -status-code -title -tech-detect
```

---

## 📁 **Directory & File Discovery**

Bash

```bash
# Dirsearch
dirsearch -u https://target.com -w /path/to/wordlist.txt

# Gobuster
gobuster dir -u https://target.com -w /usr/share/wordlists/dirb/common.txt

# Feroxbuster
feroxbuster -u https://target.com -w wordlist.txt

# ffuf
ffuf -u https://target.com/FUZZ -w wordlist.txt
```

---

## 🔗 **URL & Endpoint Discovery**

Bash

```bash
# Waybackurls
echo target.com | waybackurls | tee wayback.txt

# Gau (GetAllUrls)
gau target.com | tee gau_urls.txt

# Katana (crawling)
katana -u https://target.com -o katana_output.txt

# Extract JS files
cat urls.txt | grep "\.js$" | tee js_files.txt
```

---

## 🔑 **API & Token Testing**

Bash

```bash
# API endpoint fuzzing
ffuf -u https://api.target.com/v1/FUZZ -w api_wordlist.txt -H "Authorization: Bearer TOKEN"

# JWT testing with jwt_tool
jwt_tool <token> -a

# Check for exposed tokens in JS
cat js_files.txt | xargs -I{} curl -s {} | grep -Ei "(api_key|token|secret|password)"
```

---

## 💉 **Vulnerability Scanning**

### XSS Testing

Bash

```bash
# Dalfox
dalfox url https://target.com/page?param=test

# With pipe
cat urls.txt | dalfox pipe

# XSStrike
python3 xsstrike.py -u "https://target.com/search?q=test"
```

### SQL Injection

Bash

```bash
# SQLMap
sqlmap -u "https://target.com/page?id=1" --dbs
sqlmap -u "https://target.com/page?id=1" --batch --risk=3 --level=5
sqlmap -r request.txt --dbs
```

### SSRF Testing

Bash

```bash
# With Burp Collaborator or interactsh
interactsh-client

# Test SSRF
curl "https://target.com/fetch?url=http://YOUR_COLLAB_SERVER"
```

---

## 🛡️ **Nuclei (Template-Based Scanning)**

Bash

```bash
# Basic scan
nuclei -u https://target.com

# With templates
nuclei -u https://target.com -t cves/
nuclei -u https://target.com -t exposures/
nuclei -u https://target.com -t vulnerabilities/

# Multiple targets
nuclei -l live_hosts.txt -t nuclei-templates/ -o nuclei_results.txt

# Severity filter
nuclei -u https://target.com -s critical,high
```

---

## 🔐 **Authentication & Session Testing**

Bash

```bash 
# Check for default credentials
hydra -l admin -P /usr/share/wordlists/rockyou.txt target.com http-post-form "/login:user=^USER^&pass=^PASS^:Invalid"

# Token expiration check
curl -H "Authorization: Bearer EXPIRED_TOKEN" https://api.target.com/user
```

---

## 📧 **Email & CORS Testing**

Bash

```bash
# Email header injection test
curl -X POST -d "email=test@test.com%0ACc:attacker@evil.com" https://target.com/contact

# CORS misconfiguration
curl -H "Origin: https://evil.com" -I https://target.com/api/data
```

---

## 🧰 **Parameter Discovery**

Bash

```bash
# Arjun
arjun -u https://target.com/endpoint

# ParamSpider
paramspider -d target.com

# x8
x8 -u "https://target.com" -w params.txt
```

```bash
# Directory brute-force
ffuf -u https://target.com/FUZZ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 50 -mc 200,204,301,302,403

# With extensions
ffuf -u https://target.com/FUZZ -w wordlist.txt -e .php,.json,.bak,.env -mc 200,403

# Parameter fuzzing / hidden params
arjun -u https://target.com/endpoint -m GET,POST
ffuf -u "https://target.com/endpoint?FUZZ=test" -w param-wordlist.txt
```

---

## 📋 **GitHub Recon**

Bash

```bash
# GitDorker
python3 GitDorker.py -t YOUR_GITHUB_TOKEN -d dorks.txt -q target.com

# truffleHog (secret scanning)
trufflehog git https://github.com/target/repo

# gitleaks
gitleaks detect -s /path/to/repo
```

---

## 🔧 **One-Liner Combos**

Bash

```bash
# Full recon pipeline
subfinder -d target.com -silent | httpx -silent | nuclei -t nuclei-templates/

# Find sensitive files
gau target.com | grep -E "\.(json|xml|config|env|sql|log)$"

# Extract parameters from URLs
cat urls.txt | grep "?" | cut -d"?" -f2 | tr "&" "\n" | cut -d"=" -f1 | sort -u

# Screenshot all live hosts
cat live_hosts.txt | aquatone
```

---

## 📦 **Useful Wordlists**

Bash

```bash
# SecLists location
/usr/share/seclists/

# Common paths
/usr/share/seclists/Discovery/Web-Content/common.txt
/usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
/usr/share/seclists/Fuzzing/LFI/LFI-Jhaddix.txt
```