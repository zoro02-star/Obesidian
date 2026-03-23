# What is SMTP? 📧

**Simple Mail Transfer Protocol (SMTP)** is the protocol used to **send emails across networks**.

It is responsible for **transferring email messages from a sender to a mail server and between mail servers**.

Example flow:

User → Mail Server → Internet → Recipient Mail Server → Recipient

SMTP handles **sending**, while receiving is usually done by:

- **POP3**
    
- **IMAP**
    

---

# How SMTP Works

When you send an email:

1. Email client connects to SMTP server.
    
2. Server verifies sender.
    
3. Email is transferred to recipient mail server.
    
4. Recipient retrieves the email.
    

Typical ports used:

|Port|Purpose|
|---|---|
|25|Default SMTP communication|
|465|SMTP over SSL|
|587|Secure email submission|

---

# Basic SMTP Commands

SMTP works using commands between client and server.

Example session:

HELO client.example.com  
MAIL FROM:<harsh@example.com>  
RCPT TO:<user@example.com>  
DATA  
Subject: Hello  
  
This is a test email  
.  
QUIT

Explanation:

|Command|Meaning|
|---|---|
|HELO / EHLO|Start connection|
|MAIL FROM|Sender email|
|RCPT TO|Recipient email|
|DATA|Email body starts|
|QUIT|End connection|

---

# What are SMTP Logs?

**SMTP logs** record **all email sending activity on a mail server**.

These logs help monitor:

- Who sent an email
    
- Recipient address
    
- Sending IP
    
- Message status
    
- Delivery success or failure
    

They are often analyzed in SIEM tools like **Splunk Enterprise**.

---

# Typical SMTP Log Fields

SMTP logs usually contain:

|Field|Description|
|---|---|
|Timestamp|When email was sent|
|Sender|Email sender|
|Recipient|Email receiver|
|Source IP|Sender IP address|
|Destination server|Mail server|
|Message ID|Unique email identifier|
|Status|Delivered / Failed|
|Size|Email size|

---

# Example SMTP Log

Example log entry:

2026-03-06 10:42:18  
SourceIP=192.168.1.15  
Sender=harsh@example.com  
Recipient=admin@company.com  
MessageID=89234  
Protocol=SMTP  
Status=Delivered  
Size=45KB

Meaning:

- Email sent from **harsh@example.com**
    
- To **admin@company.com**
    
- Message delivered successfully.
    

---

# Why SMTP Logs Are Important in Cybersecurity 🔐

SMTP logs help detect:

### 1. Spam Campaigns

If a system sends **thousands of emails quickly**, it may be compromised.

Example:

5000 emails sent in 10 minutes

Possible:

- Spam bot
    
- Malware infection
    

---

### 2. Phishing Attacks

Attackers may send phishing emails pretending to be trusted domains.

Example:

support@bank-secure-login.com

SMTP logs help identify:

- Suspicious domains
    
- Unknown senders
    

---

### 3. Email Account Compromise

If a user suddenly sends emails from **different IP or country**, it might indicate a hacked account.

---

### 4. Malware Communication

Some malware sends **stolen data through email attachments**.

SMTP logs help detect unusual attachments or traffic.

---

# SMTP Log Analysis in Splunk

### Search SMTP Logs

index=email_logs protocol=smtp

---

### Top Email Senders

index=email_logs  
| stats count by sender  
| sort -count

This shows **users sending the most emails**.

---

### Detect Possible Spam

index=email_logs  
| stats count by src_ip  
| where count > 100

Meaning:

IPs sending **more than 100 emails**.

---

### Check Failed Emails

index=email_logs status=failed

This can reveal:

- Misconfigured servers
    
- Attack attempts
    

---

# Example Suspicious SMTP Activity

Normal user:

10 emails/day

Suspicious activity:

2000 emails in 5 minutes

Possible causes:

- Spam bot
    
- Compromised account
    
- Malware
    

---

# SMTP in SOC Investigations

SOC analysts check:

⚠️ Email flooding  
⚠️ Unknown domains  
⚠️ Suspicious attachments  
⚠️ High email volume from single IP  
⚠️ Emails sent outside business hours