```
Client: 192.168.202.94
Server: 192.168.25.101

User: anonymous
Command: RETR (download) 

File downloaded:
registration.pyc

Transfer mode: binary
Size: 2745 bytes

Status: Transfer successful
```
---
```js
1️⃣ FTP activity
index=main sourcetype=ftp

2️⃣ Top IPs
| stats count by src

3️⃣ Failed logins
reply_code=530

4️⃣ File types
| stats count by filetype

5️⃣ Transfer spikes
| timechart count

find specific file 
index=main sourcetype=ftp filenames="*.php" (webshell)
index=main sourcetype=ftp filenames="*.exe" (program)
index=main sourcetype=ftp filenames="*.py" (python file)
```