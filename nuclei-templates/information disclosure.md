```bash
https://web.archive.org/cdx/search/cdx?url =*. example.com/*&collapse=urlkey&output=text&fl=original

https://www.virustotal.com/vtapi/v2/domain/report?apikey=
982680b1787fa59701919aa22515a025e00df1e3bb2bc4f186b8e919558d576c&domain=example.com

https://otx.alienvault.com/api/v1/indicators/hostname/domain.com/url_list?limit=500&page=1

curl -G "https://web.archive.org/cdx/search/cdx" \
  --data-urlencode "url=*.indykite.id/*" \
  --data-urlencode "collapse=urlkey" \
  --data-urlencode "output=text" \
  --data-urlencode "fl=original" > out.txt

cat out.txt | uro | grep -Ei '\.(xls|xml|xlsx|json|pdf|sql|doc|docx|pptx|txt|zip|tar\.gz|tgz|bak|7z|rar|log|cache|secret|db|backup|yml|gz|config|csv|yaml|md|md5|exe|dll|bin|ini|bat|sh|tar|deb|rpm|iso|img|apk|msi|dmg|tmp|crt|pem|key|pub|asc)$'
```


```bash
JsRecon methodology by coffinxp:

katana -u samsung.com -d 5 -jc | grep '\.js$' | tee alljs.txt
echo www.samsung.com | gau | grep '\.js$' | anew alljs.txt
cat alljs.txt | httpx-toolkit -mc 200 -o samsung.txt
cat samsung.txt |jsleaks -s -1 -k
cat samsung.txt | nuclei -t prsnl/credentials-disclosure-all.yaml -c 30
cat samsung.txt | nuclei -t /home/coffinxp/nuclei-templates/http/exposures -c 30

cat samsung. txt | xargs -I{} bash -c 'echo -e "\ntarget : {}\n" && python lazyegg. py
"{}" -- js_urls -- domains -- ips -- leaked_creds -- local_storage'


```

finding origin ip or ip withiout cdn or waf
```bash
curl -s "https://www.virustotal.com/vtapi/v2/domain/report?domain=rapfame.app&apikey=982680b1787fa59701919aa22515a025e00
df1e3bb2bc4f186b8e919558d576c" \
| jq -r '.. | .ip_address? // empty' \
| grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' | sort -u

curl -s "https://urlscan.io/api/v1/search/?q=domain:dell.com&size=10000" \
| jq -r '.results[]?.page?.ip // empty' \
| grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' | sort -u

curl -s "https://otx.alienvault.com/api/v1/indicators/hostname/dell.com/url_list?limit=500&page=1" \
| jq -r '.url_list[]?.result?.urlworker?.ip // empty' \
| grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' | sort -u
```