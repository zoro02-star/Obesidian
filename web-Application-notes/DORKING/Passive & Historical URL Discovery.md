## 📜 **Wayback Machine (Archive.org CDX API)**

Bash

```bash
# Basic URL extraction
curl -s "https://web.archive.org/cdx/search/cdx?url=*.target.com/*&output=text&fl=original&collapse=urlkey" | sort -u | tee wayback_urls.txt

# With timestamps and status codes
curl -s "https://web.archive.org/cdx/search/cdx?url=*.target.com/*&output=text&fl=timestamp,original,statuscode&collapse=urlkey" | tee wayback_full.txt

# Filter by mimetype (JS files only)
curl -s "https://web.archive.org/cdx/search/cdx?url=*.target.com/*&output=text&fl=original&collapse=urlkey&filter=mimetype:application/javascript" | sort -u

# Filter by status code (200 only)
curl -s "https://web.archive.org/cdx/search/cdx?url=*.target.com/*&output=text&fl=original&collapse=urlkey&filter=statuscode:200" | sort -u

# Get unique digests (find different versions)
curl -s "https://web.archive.org/cdx/search/cdx?url=target.com/config.js&output=text&fl=timestamp,digest" | sort -u

# Extract endpoints with parameters
curl -s "https://web.archive.org/cdx/search/cdx?url=*.target.com/*&output=text&fl=original&collapse=urlkey" | grep "?" | sort -u | tee params_urls.txt
```

---

## 🌐 **Common Crawl**

Bash

```bash
# Get available indexes
curl -s "https://index.commoncrawl.org/collinfo.json" | jq -r '.[].cdx-api'

# Query specific index (replace CC-MAIN-2024-10 with latest)
curl -s "https://index.commoncrawl.org/CC-MAIN-2024-10-index?url=*.target.com&output=json" | jq -r '.url' | sort -u

# Loop through multiple indexes
for index in $(curl -s "https://index.commoncrawl.org/collinfo.json" | jq -r '.[0:5] | .[].id'); do
  curl -s "https://index.commoncrawl.org/${index}-index?url=*.target.com&output=json" | jq -r '.url' 2>/dev/null
done | sort -u | tee commoncrawl_urls.txt

# Get with all fields
curl -s "https://index.commoncrawl.org/CC-MAIN-2024-10-index?url=*.target.com&output=json" | jq '.'
```

---

## 👽 **AlienVault OTX**

Bash

```bash
# Get URLs for domain
curl -s "https://otx.alienvault.com/api/v1/indicators/domain/target.com/url_list?limit=500" | jq -r '.url_list[].url' | sort -u | tee alienvault_urls.txt

# Get passive DNS
curl -s "https://otx.alienvault.com/api/v1/indicators/domain/target.com/passive_dns" | jq -r '.passive_dns[].hostname' | sort -u

# Get associated malware hashes
curl -s "https://otx.alienvault.com/api/v1/indicators/domain/target.com/malware" | jq '.'

# Get general info
curl -s "https://otx.alienvault.com/api/v1/indicators/domain/target.com/general" | jq '.'

# For hostname type
curl -s "https://otx.alienvault.com/api/v1/indicators/hostname/www.target.com/url_list?limit=500" | jq -r '.url_list[].url'

# Paginate through results
for page in {1..5}; do
  curl -s "https://otx.alienvault.com/api/v1/indicators/domain/target.com/url_list?limit=500&page=$page" | jq -r '.url_list[].url'
done | sort -u
```

---

## 🔍 **URLScan.io**

Bash

```bash
# Basic domain search
curl -s "https://urlscan.io/api/v1/search/?q=domain:target.com&size=10000" | jq -r '.results[].page.url' | sort -u | tee urlscan_urls.txt

# With date range (last 30 days)
curl -s "https://urlscan.io/api/v1/search/?q=domain:target.com%20AND%20date:%3Enow-30d&size=10000" | jq -r '.results[].page.url'

# Get scan UUIDs for DOM retrieval
curl -s "https://urlscan.io/api/v1/search/?q=domain:target.com&size=100" | jq -r '.results[].task.uuid' | tee scan_uuids.txt

# Fetch DOM content from scans
while read uuid; do
  curl -s "https://urlscan.io/dom/$uuid/" >> dom_content.txt
done < scan_uuids.txt

# Get all linked resources
curl -s "https://urlscan.io/api/v1/search/?q=domain:target.com&size=100" | jq -r '.results[].task.uuid' | while read uuid; do
  curl -s "https://urlscan.io/api/v1/result/$uuid/" | jq -r '.data.requests[].request.request.url' 2>/dev/null
done | sort -u

# Search by IP
curl -s "https://urlscan.io/api/v1/search/?q=ip:1.2.3.4&size=1000" | jq -r '.results[].page.url'

# Search for specific file types
curl -s "https://urlscan.io/api/v1/search/?q=domain:target.com%20AND%20filename:*.js&size=1000" | jq '.'
```

---

## 🛡️ **VirusTotal**

Bash

```bash
# Domain report (requires API key)
curl -s "https://www.virustotal.com/vtapi/v2/domain/report?apikey=YOUR_API_KEY&domain=target.com" | jq '.'

# Extract subdomains
curl -s "https://www.virustotal.com/vtapi/v2/domain/report?apikey=YOUR_API_KEY&domain=target.com" | jq -r '.subdomains[]' | sort -u | tee vt_subdomains.txt

# Extract detected URLs
curl -s "https://www.virustotal.com/vtapi/v2/domain/report?apikey=YOUR_API_KEY&domain=target.com" | jq -r '.detected_urls[].url' | sort -u

# V3 API (newer)
curl -s -H "x-apikey: YOUR_API_KEY" "https://www.virustotal.com/api/v3/domains/target.com/subdomains?limit=100" | jq -r '.data[].id'

# Get resolutions (historical IPs)
curl -s "https://www.virustotal.com/vtapi/v2/domain/report?apikey=YOUR_API_KEY&domain=target.com" | jq -r '.resolutions[].ip_address' | sort -u
```

---

## 🕵️ **IntelX (Intelligence X)**

Bash

```bash
# Search (free tier)
curl -s -X POST "https://free.intelx.io/intelligent/search" \
  -H "x-key: YOUR_INTELX_KEY" \
  -H "Content-Type: application/json" \
  -d '{"term":"target.com","maxresults":100,"media":0,"terminate":[]}'

# Get results with search ID
curl -s "https://free.intelx.io/intelligent/search/result?id=SEARCH_ID" \
  -H "x-key: YOUR_INTELX_KEY" | jq '.'

# Paid endpoint
curl -s -X POST "https://2.intelx.io/intelligent/search" \
  -H "x-key: YOUR_INTELX_KEY" \
  -H "Content-Type: application/json" \
  -d '{"term":"target.com","maxresults":1000,"media":0}'

# Phonebook (email/domain search)
curl -s -X POST "https://free.intelx.io/phonebook/search" \
  -H "x-key: YOUR_INTELX_KEY" \
  -H "Content-Type: application/json" \
  -d '{"term":"@target.com","maxresults":100,"target":1}'
```

---

## 👻 **GhostArchive**

Bash

```bash
# Search for domain
curl -s "https://ghostarchive.org/search?term=target.com&page=1" | grep -oP 'href="/archive/[^"]+' | cut -d'"' -f2 | tee ghost_archives.txt

# Paginate through results
for page in {1..10}; do
  curl -s "https://ghostarchive.org/search?term=target.com&page=$page" | grep -oP 'href="/archive/[^"]+'
done | cut -d'"' -f2 | sort -u

# Fetch archived DOM content
while read path; do
  curl -s "https://ghostarchive.org$path" >> ghost_dom_content.txt
done < ghost_archives.txt
```

---

## 🔧 **Combined One-Liners**

Bash

```bash
# All sources combined URL extraction
{
  curl -s "https://web.archive.org/cdx/search/cdx?url=*.target.com/*&output=text&fl=original&collapse=urlkey"
  curl -s "https://otx.alienvault.com/api/v1/indicators/domain/target.com/url_list?limit=500" | jq -r '.url_list[].url' 2>/dev/null
  curl -s "https://urlscan.io/api/v1/search/?q=domain:target.com&size=10000" | jq -r '.results[].page.url' 2>/dev/null
} | sort -u | tee all_urls.txt

# Extract unique parameters from all URLs
cat all_urls.txt | grep "?" | cut -d"?" -f2 | tr "&" "\n" | cut -d"=" -f1 | sort -u | tee unique_params.txt

# Find sensitive file extensions
cat all_urls.txt | grep -E "\.(json|xml|config|env|sql|log|bak|backup|old|txt|yml|yaml|ini|conf)$" | sort -u | tee sensitive_files.txt

# Find API endpoints
cat all_urls.txt | grep -E "(api|v1|v2|v3|graphql|rest|endpoint)" | sort -u | tee api_endpoints.txt

# Extract JS files for secret hunting
cat all_urls.txt | grep -E "\.js(\?|$)" | sort -u | while read url; do
  curl -s "$url" | grep -oE "(api_key|apikey|secret|token|password|aws_access|private_key)['\"]?\s*[:=]\s*['\"][^'\"]{5,}['\"]"
done
```

---

## 📝 **Bash Function (Add to .bashrc)**

Bash

```bash
# Full recon function
recon_urls() {
  domain=$1
  echo "[*] Fetching from Wayback..."
  curl -s "https://web.archive.org/cdx/search/cdx?url=*.$domain/*&output=text&fl=original&collapse=urlkey" > /tmp/wayback_$domain.txt
  
  echo "[*] Fetching from AlienVault..."
  curl -s "https://otx.alienvault.com/api/v1/indicators/domain/$domain/url_list?limit=500" | jq -r '.url_list[].url' 2>/dev/null > /tmp/alien_$domain.txt
  
  echo "[*] Fetching from URLScan..."
  curl -s "https://urlscan.io/api/v1/search/?q=domain:$domain&size=10000" | jq -r '.results[].page.url' 2>/dev/null > /tmp/urlscan_$domain.txt
  
  cat /tmp/wayback_$domain.txt /tmp/alien_$domain.txt /tmp/urlscan_$domain.txt | sort -u > ${domain}_all_urls.txt
  echo "[+] Saved to ${domain}_all_urls.txt ($(wc -l < ${domain}_all_urls.txt) unique URLs)"
}

# Usage: recon_urls target.com
```