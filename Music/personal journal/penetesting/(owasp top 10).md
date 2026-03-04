# A01 -> Broken Access Control
Broken Access Control happens when users can **act outside of their intended permissions.**
### 1. Vertical Privileges Escalation : A low-privileged user gaining admin or **higher-level access.** 
	- Normal user accessing `/admin`
	- User changing role parameter: `role=user → role=admin`
### 2. Horizontal Privilege Escalation (IDOR) : Accessing **other users' resources** with the same role 
		- `/api/users/123` → `/api/users/124`
		- Viewing or editing another user’s data
### 3. Missing access control on Backend APIs : Frontend hides features, but backend **does not enforce authorization.**
		- Admin-only API accessible to normal users
		- Mobile API endpoints unprotected
### 4. Force Browsing / unauthorized URL Access : Directly accessing **restricted pages or endpoints.**
			- `/admin/dashboard`
			- `/manage/users`
			- `/export/all-users`
### 5. Parameter Tampering : modifying parameters that **controls access.**
		- `isAdmin=false → true`
		- `user_id=10 → 1`
		- `account_type=premium`
### 6. Mass Assignment : updating **more fields than intended** via APIs.
		Sending extra JSON fields:
					
	{
		  "username": "harsh",
		  "email": "a@b.com",
		  "role": "admin"
	}
					

### 7. JWT / Token Misuse : Access **control failures** related to tokens.
		- Missing role validation in JWT
		- Accepting expired or tampered tokens
		- Trusting client-side role claims
### 8. CORS Misconfiguration (Access Control Aspect) : Improper cross-origin permissions **allowing unauthorized access.**
		- `Access-Control-Allow-Origin: *` with credentials
		- Sensitive APIs accessible cross-origin
### 9. Bypassing Access checks via HTTP Methods : Access allowed by **changing request method.**
		- `POST` blocked, but `GET` allowed
		- `DELETE` allowed without auth
### 10. Business Logic Authorization Failures : Logical flaws allowing **forbidden** actions.
		- User canceling someone else’s order
		- Reusing coupons infinitely
		- Skipping payment steps
# A02 ->Cryptographic Failures 
**Cryptographic Failures** occur when **sensitive data is not properly protected** due to weak, missing, or incorrect use of cryptography.
### 1. Sensitive Data Transmitted in Plaintext : Data sent without encryption.
	- HTTP instead of HTTPS
	- Credentials sent over plaintext
	- APIs not using TLS
### 2. Weak or Broken Encryption Algorithms : Using outdated or insecure cryptography.
	- MD5, SHA-1
	- DES, 3DES
	- RC4
### 3. Improper Password Storage : Passwords not securely hashed.
	- Plaintext passwords
	- Reversible encryption
	- Fast hashes without salt (MD5/SHA)
	**Correct**
	- bcrypt, scrypt, Argon2, PBKDF2
### 4. Missing Encryption for Sensitive Data at Rest : Sensitive data stored unencrypted.
	- Database storing PII in plaintext
	- Unencrypted backups
	- Logs containing secrets
### 5. Hardcoded or Weak Cryptographic Keys : Poor key management.
	- Keys in source code
	- Weak or short keys
	- Reused keys across environments
### 6. Improper Certificate validation : TLS used incorrectly.
	- Accepting self-signed certs
	- No certificate validation in mobile apps
	- Disabled SSL verification
### 7. Weak Randomness : Predictable random values.
	Insecure RNG for:
	- Password reset tokens
	- Session IDs
	- OTPs

### 8. Missing or Incorrect Use of HTTPS Security Headers : failure to enforce secure transport.
	- No HSTS header
	- Mixed content (HTTP + HTTPS)
### 9. Cryptographic Misuse : Using crypto correctly but in the wrong way.
	- Encrypting passwords instead of hashing
	- Reusing IVs
	- Using ECB mode
	- Rolling your own crypto
### 10. Client-Side Cryptography Trust : Relying on client-side encryption for security.
	- Encrypting data in JS and trusting it
	- Keys exposed in frontend code

# A03 -> injections
The injection category covers vulnerabilities where untrusted input is sent to **an interpreter and executed** as part of a command or query.
1. `SQL injection (SQLI) `: injecting malicious SQL into database queries.
2. `NoSQL injection` : injection in NOSQL databases like mongoDB, CouchDB.
3. `OS command injection` : Executing system-level commands through user input.
4. `LDAP injection `: manipulating LDAP queries used for authentication or directory lookups.
5. `XPath injection `: injecting into XPath queries used for XML data access.
6. `Expression Language injection (EL injection) `: injecting expressions in languages like java EL, OGNL, SpEL.
7. `ORM injection `: Abusing object-relational mapping query builders.
8. `Header injection `: injecting malicious data into HTTP headers.
9. `Template injection` : (SSTI) : injecting malicious template expressions.
10. `Code injection` : Direct Execution of attacker-supplied code.

# A04 -> Insecure Design
**Insecure Design** is about missing or flawed security controls at the design/architecture level
### 1.  Missing Threat Modeling : No structured thinking about threats during design. 
		- No abuse-case analysis
		- No STRIDE / threat modeling
		- Security added “later”
### 2. Missing or Weak Business Logic Controls : System logic allows abuse by design.
		- Unlimited password reset attempts
		- Unlimited coupon reuse
		- No rate limit on critical actions
### 3. Absence of Defense-in-Depth : single control protecting critical functionality.
		- Only frontend validation
		- Only role check, no ownership check
		- No secondary verification
### 4. Missing Rate Limiting : Abuse possible because limits were never designed.
		- OTP brute force
		- Login flooding
		- Enumeration endpoints
### 5. Insecure Workflow Design : Flows can be skipped or reordered.
		- Checkout without payment
		- Verify email skipped
		- Direct API call completes action
### 6. Trusting the Client by Design : System assumes client input is honest.
		- Price sent from frontend
		- Role or permission from client
		- Discount calculated client-side
### 7. Lack of Abuse Case Handling : only "happy path" considered.
		No handling of:
		- Replay attacks
		- Automation
		- Race conditions
### 8. Insecure Default Design Choices : Dangerous defaults.
		- Public APIs by default
		- Weak password policy
		- Features enabled without auth
### 9. Overly Complex Design : Complexity introduces unavoidable flaws.
		- Multiple auth systems   
		- Overloaded permissions
		- Legacy + modern mixed logic
### 
10. Missing Security Requirements : Security never defined as a requirement.
		No requirement for:
		- Logging
		- Rate limiting
		- MFA
		- Monitoring
## Key Difference (Very Important)

| Category | What’s wrong                               |
| -------- | ------------------------------------------ |
| **A01**  | Access control implemented incorrectly     |
| **A04**  | Access control **never designed properly** |
| **A07**  | Authentication implemented incorrectly     |

# A05 -> Security Misconfiguration
Security Misconfiguration happens when security controls exist but are configured incorrectly, incompletely, or left insecure by default.
`The feature is there.`
`The protection is wrong.`

### 1. Default Credentials & Configurations

Leaving vendor defaults unchanged.

**Examples**

- `admin:admin`
    
- Default database users
    
- Default cloud service configs
    

---

### 2. Unnecessary Features Enabled

Attack surface increased by unused components.

**Examples**

- Admin panels exposed
    
- Debug endpoints enabled
    
- Sample apps left deployed
    

---

### 3. Improper HTTP Security Headers

Missing or weak browser protections.

**Examples**

- Missing:
    
    - `Content-Security-Policy`
        
    - `X-Frame-Options`
        
    - `X-Content-Type-Options`
        
- Weak CSP rules
    

---

### 4. Verbose Error Messages

Sensitive details leaked via errors.

**Examples**

- Stack traces
    
- SQL errors
    
- Framework debug output
    

---

### 5. Directory Listing Enabled

Server exposes file structure.

**Examples**

- `/uploads/`
    
- `/backup/`
    

---

### 6. Misconfigured CORS

Improper cross-origin access rules.

**Examples**

- `Access-Control-Allow-Origin: *` with credentials
    
- Reflecting origin header
    

---

### 7. Open Cloud Storage & Services

Cloud resources exposed publicly.

**Examples**

- Public S3 buckets
    
- Open Firebase databases
    
- Open Elasticsearch
    

---

### 8. Outdated or Vulnerable Software

Known-vulnerable components due to config neglect.

**Examples**

- Old Apache/Nginx configs
    
- Deprecated framework settings
    

---

### 9. Missing or Weak TLS Configuration

TLS present but insecurely configured.

**Examples**

- TLS 1.0 / 1.1 enabled
    
- Weak cipher suites
    
- No HSTS
    

---

### 10. Lack of Environment Separation

Production running in insecure mode.

**Examples**

- Debug enabled in prod
    
- Test keys in prod
    
- Dev configs deployed live

### 11. Insecure File & Folder Permissions

OS / container level misconfigs.

**Examples**

- World-writable directories
    
- Sensitive files readable by anyone
    
- `.env` accessible via web root
    

---

### 12. Exposed Admin / Management Interfaces

Management tools reachable publicly.

**Examples**

- `/phpmyadmin`
    
- `/actuator`
    
- `/adminer`
    
- Kubernetes dashboard exposed
    

---

### 13. Missing Security Hardening

Platform is installed but never hardened.

**Examples**

- No WAF rules
    
- No request size limits
    
- No upload restrictions
    

---

### 14. Improper Container / Docker Configuration

Container-level security gaps.

**Examples**

- Running as root
    
- Privileged containers
    
- Exposed Docker socket
    

---

### 15. Insecure Defaults in Frameworks

Framework protections disabled or misused.

**Examples**

- CSRF disabled globally
    
- CORS allow-all
    
- Deserialization protections off
    

---

### 16. Logging & Monitoring Misconfiguration

Security events not captured.

**Examples**

- No auth failure logs
    
- No alerting
    
- Logs accessible publicly
    

---

### 17. Backup & Artifact Exposure

Sensitive leftovers exposed.

**Examples**

- `.bak`, `.old`, `.zip`
    
- Source code backups
    
- Database dumps accessible
    

---

### 18. Misconfigured API Gateways / Reverse Proxies

Edge security broken.

**Examples**

- Auth bypass due to proxy rules
    
- Header trust misconfig
    
- Path-based auth gaps

## One Rule That Never Fails

```
> If security exists but is set up wrong → A05 
> If security never existed → A04
```

# A06 -> Vulnerable and Outdated Components
This category covers risks caused by **using components with known vulnerabilities** or **components that are no longer maintained**.

> The app is secure.  
> The stuff it’s built on… isn’t.

### 1. Using Components with Known CVEs

Libraries, frameworks, or platforms with publicly known vulnerabilities.

**Examples**

- Vulnerable npm / pip / Maven packages
    
- Old OpenSSL, Log4j (Log4Shell)
    
- Vulnerable CMS plugins
    

---

### 2. Outdated Libraries & Frameworks

No longer receiving security patches.

**Examples**

- Old React / Angular / jQuery
    
- Unsupported Java, PHP, Python versions
    
- Deprecated APIs
    

---

### 3. Unpatched Operating Systems

Infrastructure-level outdated components.

**Examples**

- Old Linux kernel
    
- Unpatched Windows Server
    
- EOL server images
    

---

### 4. Vulnerable Containers & Images

Base images with known flaws.

**Examples**

- Old Docker base images
    
- Images from untrusted sources
    
- No image scanning
    

---

### 5. Vulnerable Third-Party Services

External components integrated into the app.

**Examples**

- Vulnerable payment SDKs
    
- Analytics libraries with XSS
    
- Auth providers with known issues
    

---

### 6. Unknown Component Versions

No inventory or tracking of dependencies.

**Examples**

- No SBOM
    
- Can’t tell which version is deployed
    
- “Works on my machine” deployments
    

---

### 7. Unused or Transitive Dependencies

Hidden risks from dependencies you didn’t directly install.

**Examples**

- Deep npm dependency chains
    
- Unused packages left installed
    

---

### 8. Insecure Update & Patch Process

Updates exist but aren’t applied safely.

**Examples**

- Manual patching
    
- No dependency monitoring
    
- Ignoring security advisories
    

---

### 9. Vulnerable Client-Side Components

Frontend dependencies with known issues.

**Examples**

- Old jQuery with XSS
    
- Vulnerable charting libraries
    
- CDN-loaded libraries without integrity checks
    

---

### 10. No Security Testing for Components

Components never assessed for risk.

**Examples**

- No dependency scanning
    
- No CVE monitoring
    
- No version pinning
    


---
## Common Confusions (Important)

|Issue|Correct Category|
|---|---|
|Default creds|**A05**|
|Weak crypto algorithm|**A02**|
|Logic flaw|**A04**|
|Library CVE|**A06**|
|Misconfigured library|**A05**|

---

## Bug Bounty Mindset

> **“What outdated thing are they still trusting?”**

---

### Key Real-World Examples

- **Log4Shell**
    
- **Heartbleed**
    
- **Shellshock**
    
- **Old WordPress plugins**
    

---

### Verdict

- ❌ Not a coding mistake
    
- ✅ Fixed by updating, removing, or replacing components
    

---

### 11. End-of-Life (EOL) Software

Still running but **no longer supported**.

**Examples**

- PHP 7.2
    
- Python 2.x
    
- Old Node.js LTS
    
- Unsupported Linux distros
    

---

### 12. Components Without Security Advisories

Blind spots in dependency tracking.

**Examples**

- Private/internal libraries
    
- Forked open-source projects
    
- No CVE monitoring
    

---

### 13. Insecure Dependency Sources

Where components come from.

**Examples**

- Untrusted package registries
    
- Typosquatting packages
    
- No checksum or signature verification
    

---

### 14. Missing Integrity Verification

Updates can be tampered with.

**Examples**

- CDN JS without SRI
    
- No hash validation
    
- Auto-updates without verification
    

---

### 15. Abandoned or Unmaintained Libraries

No updates even if no CVE exists yet.

**Examples**

- Last commit years ago
    
- No maintainer response
    
- Security issues unfixed
    

---

### 16. Shadow Dependencies

Dependencies introduced indirectly by tools.

**Examples**

- Build tools
    
- CI/CD plugins
    
- Monitoring agents
    

---

### 17. Vendor Components With Hidden Vulnerabilities

Closed-source but vulnerable.

**Examples**

- Proprietary CMS plugins
    
- Vendor SDKs
    
- Appliance firmware

# A07 -> Identification & Authentication Failures
Alright — let’s lock in **A07: Identification & Authentication Failures** only.  
Clean, complete, and OWASP-accurate.  
(OWASP Top 10: **2021**)

---

## A07: Identification & Authentication Failures

This category covers failures in **confirming who a user is** and **properly managing authentication credentials, sessions, and identity**.

> The system can’t reliably tell **who you are**.

---

## What comes under A07

### 1. Weak Password Policies

Passwords allowed to be easily guessed.

**Examples**

- No minimum length
    
- No complexity rules
    
- Allowing common passwords
    

---

### 2. Credential Stuffing & Brute Force Protection Missing

No safeguards against automated attacks.

**Examples**

- No rate limiting on login
    
- No CAPTCHA or lockout
    
- Unlimited OTP attempts
    

---

### 3. Default or Hardcoded Credentials

Credentials shipped or embedded in code.

**Examples**

- `admin/admin`
    
- API keys in source code
    
- Test credentials in production
    

---

### 4. Authentication Bypass

Login can be skipped or tricked.

**Examples**

- Direct API access without auth
    
- Missing auth checks on endpoints
    
- Logic flaws allowing login bypass
    

---

### 5. Weak or Broken Session Management

Sessions not handled securely.

**Examples**

- Predictable session IDs
    
- Session fixation
    
- Session not invalidated on logout
    

---

### 6. Improper MFA Implementation

Multi-factor auth present but flawed.

**Examples**

- MFA optional for admins
    
- MFA bypass via API
    
- MFA not enforced after login
    

---

### 7. Insecure Password Recovery / Reset

Reset flows that can be abused.

**Examples**

- Guessable reset tokens
    
- No expiration
    
- Account enumeration via reset responses
    

---

### 8. Improper Token Handling

Auth tokens used insecurely.

**Examples**

- JWTs not validated properly
    
- Accepting unsigned or weakly signed tokens
    
- Tokens stored in localStorage (XSS risk)
    

---

### 9. Account Enumeration

System leaks whether an account exists.

**Examples**

- Different error messages for user vs password
    
- Reset emails only for valid users
    

---

### 10. Re-authentication Missing for Sensitive Actions

Critical actions don’t require re-verification.

**Examples**

- Changing email/password without password
    
- Disabling MFA without confirmation

---
### 11. Session Timeout Issues

Sessions live too long or never expire.

**Examples**

- “Remember me” forever
    
- No idle timeout
    
- Tokens valid for weeks
    

---

### 12. Insecure Cookie Attributes

Cookies exist but aren’t protected.

**Examples**

- Missing `HttpOnly`
    
- Missing `Secure`
    
- Missing `SameSite`
    

---

### 13. Concurrent Session Abuse

System allows unlimited active sessions.

**Examples**

- Same account logged in everywhere
    
- No session revocation
    

---

### 14. Weak OTP / MFA Token Handling (Auth Side)

MFA exists but can be abused.

**Examples**

- OTP reuse
    
- OTP not bound to session
    
- No attempt limits
    

> (If OTP is predictable → **A02**  
> If OTP logic is broken → **A07**)

---

### 15. Improper Logout Handling

Logout doesn’t actually end the session.

**Examples**

- Token still valid after logout
    
- Back button still works
    

---

### 16. Trusting Client-Side Auth State

Client controls authentication state.

**Examples**

- `isLoggedIn=true` in frontend
    
- Role stored only in JS
    

---

### 17. Identity Verification Gaps

Identity not strongly verified.

**Examples**

- Email change without verification
    
- Phone number change without OTP
    

---

## One-Line OWASP Definition

> _Failures related to identifying users, authenticating them, and managing their sessions._

---

## Very Common Confusions (Important)

|Issue|Correct Category|
|---|---|
|IDOR|**A01**|
|Weak password hashing|**A02**|
|Login flow badly designed|**A04**|
|Default creds|**A07**|
|Old auth library CVE|**A06**|

---

## Bug Bounty Mindset

> **“Can I log in as someone I shouldn’t — or stay logged in when I shouldn’t?”**

---

### Quick Verdict

- ✅ About identity, login, tokens, sessions
    
- ❌ Not about permissions (A01)


# A08 -> Software and Data Integrity Failures
This category covers failures related to **code, updates, libraries, pipelines, and data that are trusted without integrity verification**.

> You trusted it.  
> You shouldn’t have.

---

## What comes under A08

### 1. Untrusted Software Updates

Updates applied without integrity checks.

**Examples**

- No digital signatures
    
- No checksum verification
    
- Auto-updates from untrusted sources
    

---

### 2. Insecure CI/CD Pipelines

Build and deploy process can be tampered with.

**Examples**

- No access control on pipeline
    
- Secrets exposed in CI logs
    
- Unsigned build artifacts
    

---

### 3. Dependency Confusion / Supply-Chain Attacks

Malicious packages injected into dependency resolution.

**Examples**

- Public package overriding private one
    
- Typosquatting dependencies
    
- Malicious npm / PyPI packages
    

---

### 4. Missing Integrity Checks for Critical Data

Data trusted without verification.

**Examples**

- Config files without checksum
    
- Serialized objects trusted blindly
    
- Webhooks without signature validation
    

---

### 5. Insecure Deserialization

Untrusted serialized data leading to tampering or RCE.

**Examples**

- Java deserialization
    
- PHP object injection
    
- Pickle deserialization
    

> (In 2017 this was its own category — now folded into **A08**)

---

### 6. Unsigned or Unverified Code Execution

Running code without validating its origin.

**Examples**

- Plugins without signature checks
    
- Scripts fetched and executed directly
    
- Browser extensions without verification
    

---

### 7. Client-Side Integrity Trust

Trusting client-side code or data integrity.

**Examples**

- Trusting hidden form fields
    
- Trusting client-generated JSON
    
- No server-side validation
    

---

### 8. Lack of Rollback & Update Validation

Bad updates can’t be safely reversed.

**Examples**

- No version pinning
    
- No rollback strategy
    
- Hotfixes applied blindly
    

---

### 9. Compromised Third-Party Assets

External assets altered upstream.

**Examples**

- Compromised CDN scripts
    
- Malicious analytics code
    
- Altered SDKs
    

---

### 10. Blind Trust in External Systems

No verification of external inputs.

**Examples**

- Webhooks without HMAC verification
    
- Partner APIs trusted blindly
    
- No origin validation
    

---
### 11. Missing Signature Verification for Webhooks

Integrity of incoming events not verified.

**Examples**

- No HMAC validation
    
- Trusting `X-Signature` blindly
    

---

### 12. Tampering with Configuration Artifacts

Config treated as trusted data.

**Examples**

- `.yaml`, `.json` configs modified
    
- Environment files altered
    
- Feature flags changed without checks
    

---

### 13. Build Artifact Poisoning

Build outputs modified post-build.

**Examples**

- Unsigned binaries
    
- Artifacts stored in public buckets
    
- No checksum validation before deploy
    

---

### 14. Script Injection via Update Mechanisms

Update channels abused to deliver malicious code.

**Examples**

- App downloads scripts on startup
    
- No integrity checks on fetched code
    

---

### 15. Weak Trust Boundaries in Microservices

Services trust each other blindly.

**Examples**

- No request signing between services
    
- Internal APIs assumed safe
    

---

### 16. Cache Poisoning of Trusted Data

Trusted data source poisoned.

**Examples**

- CDN cache poisoning
    
- Config cache poisoning
    
- Serialized object cache poisoning
    

---

### 17. Model / AI Artifact Integrity Failures

(Modern but still valid A08)

**Examples**

- Loading unverified ML models
    
- Tampered model weights
    
- No hash/signature validation

---
## One-Line OWASP Definition

> _Failures related to software updates, critical data, and CI/CD pipelines without integrity checks._

---

## Common Confusions (Important)

|Issue|Correct Category|
|---|---|
|Library CVE|**A06**|
|Weak crypto|**A02**|
|Logic flaw|**A04**|
|Insecure deserialization|**A08**|
|Missing auth|**A07**|

---

## Bug Bounty Mindset

> **“What am I trusting that I shouldn’t?”**
# A09 -> Security Logging and Monitoring Failures
This category covers failures in **confirming who a user is** and **properly managing authentication credentials, sessions, and identity**.

> The system can’t reliably tell **who you are**.

---

## What comes under A07

### 1. Weak Password Policies

Passwords allowed to be easily guessed.

**Examples**

- No minimum length
    
- No complexity rules
    
- Allowing common passwords
    

---

### 2. Credential Stuffing & Brute Force Protection Missing

No safeguards against automated attacks.

**Examples**

- No rate limiting on login
    
- No CAPTCHA or lockout
    
- Unlimited OTP attempts
    

---

### 3. Default or Hardcoded Credentials

Credentials shipped or embedded in code.

**Examples**

- `admin/admin`
    
- API keys in source code
    
- Test credentials in production
    

---

### 4. Authentication Bypass

Login can be skipped or tricked.

**Examples**

- Direct API access without auth
    
- Missing auth checks on endpoints
    
- Logic flaws allowing login bypass
    

---

### 5. Weak or Broken Session Management

Sessions not handled securely.

**Examples**

- Predictable session IDs
    
- Session fixation
    
- Session not invalidated on logout
    

---

### 6. Improper MFA Implementation

Multi-factor auth present but flawed.

**Examples**

- MFA optional for admins
    
- MFA bypass via API
    
- MFA not enforced after login
    

---

### 7. Insecure Password Recovery / Reset

Reset flows that can be abused.

**Examples**

- Guessable reset tokens
    
- No expiration
    
- Account enumeration via reset responses
    

---

### 8. Improper Token Handling

Auth tokens used insecurely.

**Examples**

- JWTs not validated properly
    
- Accepting unsigned or weakly signed tokens
    
- Tokens stored in localStorage (XSS risk)
    

---

### 9. Account Enumeration

System leaks whether an account exists.

**Examples**

- Different error messages for user vs password
    
- Reset emails only for valid users
    

---

### 10. Re-authentication Missing for Sensitive Actions

Critical actions don’t require re-verification.

**Examples**

- Changing email/password without password
    
- Disabling MFA without confirmation
    

---
### 11. No Log Correlation

Events logged but never connected.

**Examples**

- Login + data access not linked
    
- No user/session correlation
    
- No timeline reconstruction
    

---

### 12. No Time Synchronization

Logs exist but timestamps are unreliable.

**Examples**

- Servers with different times
    
- No NTP
    
- Impossible forensic analysis
    

---

### 13. Alert Fatigue by Design

So many alerts that real ones are ignored.

**Examples**

- No alert prioritization
    
- No severity levels
    
- No suppression rules
    

---

### 14. No Forensic Readiness

System not designed for investigations.

**Examples**

- Logs overwritten too fast
    
- No immutable storage
    
- No centralized logging
    

---

### 15. No Logging for Abuse Patterns

System logs events but not **abuse behavior**.

**Examples**

- No tracking of failed OTP attempts
    
- No tracking of enumeration
    
- No tracking of scraping
    

---

### 16. Logging Disabled or Inconsistent Across Services

Partial visibility.

**Examples**

- One microservice logs, others don’t
    
- Logging disabled for performance
    
- Different formats everywhere
---
## One-Line OWASP Definition

> _Failures related to identifying users, authenticating them, and managing their sessions._

---

## Very Common Confusions (Important)

|Issue|Correct Category|
|---|---|
|IDOR|**A01**|
|Weak password hashing|**A02**|
|Login flow badly designed|**A04**|
|Default creds|**A07**|
|Old auth library CVE|**A06**|

---

## Bug Bounty Mindset

> **“Can I log in as someone I shouldn’t — or stay logged in when I shouldn’t?”**

---

### Quick Verdict

- ✅ About identity, login, tokens, sessions
    
- ❌ Not about permissions (A01)

# A10: Server-Side Request Forgery (SSRF)

**SSRF** occurs when an application **fetches a remote resource based on user input** and attackers can **force the server to make unintended requests**.

> You control the URL.  
> The server makes the request.

---

## What comes under A10

### 1. Basic SSRF

User controls a URL that the server fetches.

**Examples**

- Image fetch
    
- PDF generator
    
- Webhook tester
    
- URL preview
    

---

### 2. Internal Network Access

Accessing internal services not exposed publicly.

**Examples**

- `http://localhost`
    
- `http://127.0.0.1`
    
- `http://internal-api`
    

---

### 3. Cloud Metadata SSRF

Accessing cloud metadata services.

**Examples**

- `http://169.254.169.254`
    
- Stealing IAM credentials
    
- Cloud instance info leakage
    

---

### 4. Blind SSRF

No direct response, but request is made.

**Examples**

- Timing-based detection
    
- Out-of-band (Burp Collaborator)
    
- DNS-based callbacks
    

---

### 5. Filter Bypass Techniques

Evading SSRF protections.

**Examples**

- IP encoding (decimal, hex, octal)
    
- DNS rebinding
    
- URL parsing tricks
    
- `@`, `#`, `?` abuse
    

---

### 6. Protocol Smuggling

Using non-HTTP protocols.

**Examples**

- `file://`
    
- `gopher://`
    
- `ftp://`
    
- `dict://`
    

---

### 7. Redirect-Based SSRF

Following redirects to internal targets.

**Examples**

- External URL → internal redirect
    
- Open redirect abuse
    

---

### 8. SSRF via Headers

Server trusts request headers.

**Examples**

- `Host`
    
- `X-Forwarded-For`
    
- `X-Forwarded-Host`
    

---

### 9. SSRF Leading to RCE or Data Exfiltration

SSRF chained with other issues.

**Examples**

- Redis → RCE
    
- Internal admin panels
    
- Kubernetes API abuse
    

---

### 10. Partial SSRF / Limited SSRF

SSRF with constraints.

**Examples**

- Only DNS requests allowed
    
- Only specific ports
    
- Only internal hostnames
    

---
### 11. DNS-Based SSRF

Server resolves attacker-controlled domain.

**Examples**

- DNS rebinding
    
- Internal IP resolution via DNS
    
- Split-horizon DNS abuse
    

---

### 12. IPv6 SSRF

Filters only block IPv4.

**Examples**

- `http://[::1]`
    
- `http://[::ffff:127.0.0.1]`
    

---

### 13. Port Scanning via SSRF

Using SSRF to map internal services.

**Examples**

- Timing differences
    
- Connection errors
    
- Response size changes
    

---

### 14. Authentication-Bypassing SSRF

SSRF reaches services that trust internal traffic.

**Examples**

- Internal admin panels
    
- Monitoring dashboards
    
- Private APIs without auth
    

---

### 15. Chained SSRF (Second-Order SSRF)

Payload stored first, triggered later.

**Examples**

- Webhooks saved in DB
    
- Feed URLs processed later
    
- Background jobs fetching URLs
    

---

### 16. File-Based SSRF (Local File Read)

SSRF abused to read files.

**Examples**

- `file:///etc/passwd`
    
- Local config files
    
- Cloud credential files
    

---

### 17. SSRF via Parsing Confusion

Abusing URL parsing differences.

**Examples**

- `http://127.0.0.1#evil.com`
    
- `http://evil.com@127.0.0.1`
    
- Double encoding
    

---

### 18. SSRF via PDF / Image / Media Processors

Non-obvious fetch points.

**Examples**

- PDF generators
    
- Image resizers
    
- Video thumbnailers
---
## One-Line OWASP Definition

> _SSRF flaws occur when an application fetches a remote resource without validating the user-supplied URL._

---

## Common Confusions (Important)

|Issue|Correct Category|
|---|---|
|Client-side fetch|❌ Not SSRF|
|Open redirect|❌ Not SSRF|
|XXE|❌ A05/A03|
|Internal API abuse via SSRF|✅ A10|

---

## Bug Bounty Mindset

> **“Where does the server fetch something on my behalf?”**

---

### Quick Verdict

- ✅ Server makes the request
    
- ❌ User should not control destination

# Beyond OWASP Top-10 
## 1. Business Logic & Abuse

- Workflow bypass
    
- Race conditions / double spend
    
- Coupon & reward abuse
    
- Subscription abuse
    
- Price manipulation
    
- State machine bypass
    

👉 _Not fully covered by OWASP Top 10_

---

## 2. Authentication & Identity Edge Cases

- Account takeover chains
    
- Password reset poisoning
    
- MFA fatigue attacks
    
- Session riding
    
- OAuth misbinding
    
- SSO misconfigurations
    

---

## 3. Authorization Edge Cases

- Permission inheritance flaws
    
- Role confusion
    
- Object-level vs function-level auth
    
- Multi-tenant isolation failures
    

---

## 4. Injection (Beyond SQL/XSS)

- SSTI
    
- LDAP / XPath injection
    
- Expression language injection
    
- Command injection variants
    
- Header injection
    

---

## 5. Client-Side & Browser Attacks

- DOM Clobbering
    
- Mutation XSS
    
- Clickjacking (UI redress)
    
- Service Worker abuse
    
- Web Storage abuse
    

---

## 6. API & Protocol Attacks

- API parameter pollution
    
- GraphQL introspection abuse
    
- WebSocket hijacking
    
- HTTP request smuggling
    
- HTTP desync attacks
    

---

## 7. File Handling Attacks

- Unrestricted file upload
    
- Polyglot files
    
- MIME-type confusion
    
- Path traversal
    
- ZIP Slip
    

---

## 8. Serialization & Data Handling

- Insecure deserialization
    
- Prototype pollution
    
- JSON injection
    
- Object reference confusion
    

---

## 9. Infrastructure & Network

- DNS rebinding
    
- Subdomain takeover
    
- Open ports & admin services
    
- Firewall bypass via SSRF
    

---

## 10. Cloud / Container / DevOps

- IAM privilege escalation
    
- Serverless abuse
    
- Kubernetes API attacks
    
- Docker socket exposure
    
- CI/CD secret leaks
    

---

## 11. Cryptography & Secrets

- Hardcoded secrets
    
- JWT algorithm confusion
    
- Token replay
    
- Key reuse
    
- Side-channel crypto leaks
    

---

## 12. Supply Chain & Third Party

- Typosquatting
    
- Dependency confusion
    
- Compromised SDKs
    
- Malicious browser extensions
    

---

## 13. Availability & Resilience

- Application-layer DoS
    
- Regex DoS (ReDoS)
    
- Resource exhaustion
    
- Logic-based DoS
    

---

## 14. Data Privacy & Compliance

- Excessive data exposure
    
- PII leakage
    
- GDPR/consent violations
    
- Over-logging sensitive data
    

---

## 15. Emerging & Advanced Attacks

- AI / ML model poisoning
    
- Prompt injection
    
- LLM data leakage
    
- Cache poisoning
    
- Speculative execution attacks