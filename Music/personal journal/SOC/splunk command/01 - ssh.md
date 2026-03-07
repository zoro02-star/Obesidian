```
index=linux_logs sourcetype=sshd
("Failed password" OR "Accepted password")
| stats count by src_ip action
```
