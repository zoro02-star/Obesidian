Spoofing, Tempering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege.
# Spoofing (Authentication Threats)
**Definition:** Illegally posing as a legitimate user, system, or service to gain access.
**Examples:**
- Login using stolen credentials
- Session hijacking (reusing session tokens)
- Fake services posing as trusted endpoints (e.g., phishing domains)
**Typical Targets:**
- Authentication services
- External interfaces
- Any identity-related processes
**Common Mitigations:**
- Strong multi-factor authentication (MFA)
- Cryptographically verifiable identities (certificates, keys)
- Anti-spoofing controls at network boundary and API levels
## 1️⃣ User Identity Spoofing (Account Takeover / Impersonation)

### ✅ Checks to Perform

- Can I log in **without valid credentials**?
    
- Can I brute-force usernames/passwords?
    
- Is username enumeration possible?
    
- Can I log in as another user via parameter manipulation?
    
- Can I bypass login via:
    
    - Missing auth checks
        
    - Default credentials
        
    - Weak password policy
        
- Can I reuse leaked credentials (credential stuffing)?
    
- Can I log in using **password reset abuse**?
    

### 🎯 Common Attacks

- Brute force
    
- Credential stuffing
    
- Default credentials
    
- Logic flaws in login
    
- Weak reset tokens
    

### 🧪 Test Examples

`POST /login username=admin&password=admin`

`POST /reset-password email=victim@example.com`

---

## 2️⃣ Session Spoofing (Session Hijacking / Fixation)

### ✅ Checks

- Can I reuse someone else’s session ID?
    
- Is session ID predictable?
    
- Does session change after login?
    
- Can I set my own session ID before login? (fixation)
    
- Are cookies missing:
    
    - HttpOnly
        
    - Secure
        
    - SameSite
        

### 🎯 Attacks

- Session hijacking
    
- Session fixation
    
- Replay attacks
    

### 🧪 Test

`Cookie: sessionid=12345`

---

## 3️⃣ Token Spoofing (JWT / OAuth / API Tokens)

### ✅ Checks

- Can I:
    
    - Modify JWT payload?
        
    - Use `alg=none`?
        
    - Replace user ID (`sub`, `uid`)?
        
- Is token signature validated?
    
- Are refresh tokens reusable?
    
- Can I use expired tokens?
    
- Can I swap access tokens between users?
    

### 🎯 Attacks

- JWT tampering
    
- Token replay
    
- OAuth misbinding
    

### 🧪 JWT Tests

`{   "sub": "admin",   "role": "admin" }`

---

## 4️⃣ API Identity Spoofing

### ✅ Checks

- Can I call APIs **without authentication**?
    
- Can I spoof:
    
    - API key
        
    - Authorization header
        
- Can I impersonate another user via:
    
    - userId parameter
        
    - accountId parameter
        
- Is auth checked on **every endpoint**?
    

### 🎯 Attacks

- Broken authentication
    
- Broken object level auth (BOLA)
    
- IDOR with identity takeover
    

### 🧪 Test

`GET /api/users/12345 Authorization: Bearer attacker_token`

---

## 5️⃣ Service-to-Service Spoofing (Microservices / Cloud)

### ✅ Checks

- Can one service impersonate another?
    
- Is mutual TLS enforced?
    
- Are service tokens shared?
    
- Are internal APIs exposed externally?
    
- Can I fake internal headers?
    

### 🎯 Attacks

- Trust boundary abuse
    
- Internal service impersonation
    

### 🧪 Header Spoofing

`X-Internal-Service: billing`

---

## 6️⃣ Network Identity Spoofing

### ✅ Checks

- Can I spoof IP-based trust?
    
- Does the app trust:
    
    - `X-Forwarded-For`
        
    - `X-Real-IP`
        
- Can I bypass IP allowlists?
    
- Is there trust on localhost / private IPs?
    

### 🎯 Attacks

- IP spoofing
    
- Proxy header injection
    

### 🧪 Test

`X-Forwarded-For: 127.0.0.1`

---

## 7️⃣ Email & Communication Spoofing

### ✅ Checks

- Can I send password reset emails pretending to be system?
    
- Is SPF / DKIM / DMARC enforced?
    
- Can I spoof “From” address?
    
- Can I intercept verification links?
    

### 🎯 Attacks

- Email spoofing
    
- Phishing
    
- Reset-link hijacking
    

---

## 8️⃣ Device / Client Spoofing

### ✅ Checks

- Does app trust:
    
    - User-Agent
        
    - Device ID
        
    - Client version
        
- Can I bypass device binding?
    
- Can I replay mobile tokens from another device?
    

### 🎯 Attacks

- Client impersonation
    
- Emulator bypass
    

---

## 9️⃣ Role / Privilege Spoofing

### ✅ Checks

- Can I:
    
    - Change role parameter?
        
    - Modify hidden fields?
        
- Is role derived from:
    
    - Client input ❌
        
    - Server session ✅
        
- Can I claim admin privileges?
    

### 🎯 Attacks

- Privilege spoofing
    
- AuthZ confusion
    

### 🧪 Test

`{   "role": "admin" }`

---

## 🔟 DNS / Hostname Spoofing

### ✅ Checks

- Can I:
    
    - Poison DNS cache?
        
    - Redirect traffic to fake server?
        
- Does TLS properly validate certs?
    
- Are cert pinning controls present (mobile)?
    

---

## 11️⃣ Replay Attacks (Identity Reuse)

### ✅ Checks

- Can I reuse:
    
    - OTPs
        
    - Password reset tokens
        
    - Magic login links
        
- Are tokens one-time and short-lived?
    

---

## 12️⃣ Trust Boundary Spoofing (STRIDE-Specific)

### ✅ Checks

- Can external user appear as:
    
    - Internal admin?
        
    - Trusted service?
        
- Is authentication missing at trust boundaries?
    
- Are assumptions documented or implicit?

# Tempering (Integrity Threats)

**Definition:** Unauthorized modification of data, code, or system artifacts.
**Examples:**

- Altering database records
    
- Manipulating API parameters in transit
    
- Modified binaries deployed to production
    

**Impact:**

- Compromised business logic
    
- Data corruption without detection
    

**Typical Mitigations:**

- Integrity verification (hashes, digital signatures)
    
- Secure transport (TLS)
    
- Change control, access restrictions

## 1️⃣ Parameter Tampering (MOST COMMON)

### ✅ What to Check

- Can I modify:
    
    - `price`
        
    - `quantity`
        
    - `role`
        
    - `userId`
        
    - `accountId`
        
    - `discount`
        
    - `status`
        
- Are client-side values trusted?
    
- Are hidden fields validated server-side?
    

### 🧪 Tests

`POST /checkout price=1`

`role=admin`

### 🎯 Real Impact

- Free purchases
    
- Account takeover
    
- Privilege escalation
    

---

## 2️⃣ URL / Query String Tampering

### ✅ Checks

- Modify URL parameters:
    
    - IDs
        
    - pagination
        
    - filters
        
- Negative numbers
    
- Large numbers
    
- Strings instead of integers
    

### 🧪 Tests

`GET /order?orderId=9999 GET /user?id=1 OR 1=1`

---

## 3️⃣ Cookie Tampering

### ✅ Checks

- Modify cookies:
    
    - session data
        
    - roles
        
    - flags
        
- Is cookie signed or encrypted?
    
- Can cookies be replayed?
    

### 🧪 Tests

`Cookie: role=user`

→ change to:

`Cookie: role=admin`

---

## 4️⃣ Hidden Field Tampering (HTML / JS)

### ✅ Checks

- Inspect hidden inputs
    
- Modify via devtools
    
- Remove disabled fields
    

### 🧪 Tests

`<input type="hidden" name="isAdmin" value="false">`

---

## 5️⃣ Client-Side Validation Bypass

### ✅ Checks

- Disable JavaScript validation
    
- Send invalid data directly to server
    
- Test:
    
    - Length
        
    - Format
        
    - Range
        
    - Required fields
        

### 🧪 Tests

`age=999 email=notanemail`

---

## 6️⃣ HTTP Method Tampering

### ✅ Checks

- Change method:
    
    - GET ↔ POST
        
    - PUT ↔ DELETE
        
- Check if access control differs
    

### 🧪 Tests

`GET /deleteUser?id=5`

---

## 7️⃣ JSON Body Tampering (APIs)

### ✅ Checks

- Add extra fields
    
- Remove required fields
    
- Modify nested objects
    
- Change boolean flags
    

### 🧪 Tests

`{   "userId": 10,   "isAdmin": true }`

---

## 8️⃣ Mass Assignment / Overposting

### ✅ Checks

- Can you set fields not intended by UI?
    
- Check API schema vs actual behavior
    

### 🧪 Tests

`{   "username": "harsh",   "role": "admin",   "balance": 1000000 }`

---

## 9️⃣ Price / Business Logic Tampering

### ✅ Checks

- Modify:
    
    - cart totals
        
    - coupon codes
        
    - payment status
        
- Double-spend
    
- Skip payment steps
    

### 🧪 Tests

`total=0 payment_status=paid`

---

## 🔟 File Upload Tampering

### ✅ Checks

- Modify:
    
    - file extension
        
    - MIME type
        
    - file content
        
- Upload scripts disguised as images
    
- Modify metadata
    

### 🧪 Tests

`Content-Type: image/jpeg shell.php`

---

## 1️⃣1️⃣ Header Tampering

### ✅ Checks

- Modify:
    
    - `Content-Type`
        
    - `Host`
        
    - `X-Forwarded-For`
        
    - `Referer`
        
- Does app trust headers?
    

### 🧪 Tests

`X-Forwarded-For: 127.0.0.1`

---

## 1️⃣2️⃣ State / Workflow Tampering

### ✅ Checks

- Skip steps in multi-step flows
    
- Replay requests out of order
    
- Modify state transitions
    

### 🧪 Example

- Submit order without payment
    
- Approve before review
    

---

## 1️⃣3️⃣ Token / Signature Tampering

### ✅ Checks

- Modify JWT payload
    
- Check if signature validated
    
- Use old tokens
    
- Change claims
    

### 🧪 Tests

`{   "role": "admin" }`

---

## 1️⃣4️⃣ Cache & Race Condition Tampering

### ✅ Checks

- Simultaneous requests
    
- Modify same resource concurrently
    
- Double-spend issues
    

---

## 1️⃣5️⃣ Config / Feature Flag Tampering

### ✅ Checks

- Enable disabled features
    
- Toggle beta flags
    
- Modify environment variables via input
    

---

## 1️⃣6️⃣ Logging & Audit Tampering

### ✅ Checks

- Can attacker modify:
    
    - logs
        
    - timestamps
        
    - user IDs in logs
        
- Can logs be deleted or overwritten?
    

---

## 1️⃣7️⃣ Third-Party / Integration Tampering

### ✅ Checks

- Modify:
    
    - webhook payloads
        
    - callback URLs
        
    - signed requests
        
- Are signatures verified?

## 1️⃣ Cryptographic Integrity Tampering (Very High Impact)

### What Most Miss

- Data is “encrypted” but **not authenticated**
    
- MAC not verified
    
- Same key used for encryption + signing
    

### Checks

- Can I modify encrypted blobs and still get valid responses?
    
- Is AES used without HMAC / AEAD?
    
- Are signatures verified on **every** request?
    

### Example

`data=ENCRYPTED_BLOB`

→ flip bits → server accepts

---

## 2️⃣ Webhook & Callback Tampering (Bug Bounty Gold)

### Checks

- Modify webhook payloads
    
- Replay old webhook events
    
- Remove signature headers
    
- Change order ID / amount
    

### Real Impact

- Fake “payment successful”
    
- Trigger refunds
    
- Free subscriptions
    

---

## 3️⃣ Client-State Tampering (SPA / React / Vue)

### Checks

- Modify:
    
    - Redux / Zustand store
        
    - localStorage
        
    - sessionStorage
        
- Does server trust client state?
    

### Example

`localStorage.setItem("isPremium", true)`

---

## 4️⃣ GraphQL-Specific Tampering

### Checks

- Modify query structure
    
- Add unauthorized fields
    
- Remove limits
    
- Change variables
    

### Example

`mutation {   updateUser(id: 1, role: ADMIN) }`

---

## 5️⃣ Deserialization Tampering

### Checks

- Modify serialized objects
    
- Type confusion
    
- Object injection
    

### Impact

- Auth bypass
    
- RCE (in some stacks)
    

---

## 6️⃣ HTTP/2 & Protocol-Level Tampering

### Checks

- Duplicate headers
    
- Smuggling-like behavior
    
- Inconsistent parsing
    

### Example

`Content-Length: 0 Transfer-Encoding: chunked`

---

## 7️⃣ Cache Poisoning & Cache Key Tampering

### Checks

- Modify headers that affect cache:
    
    - Host
        
    - X-Forwarded-Host
        
- Can attacker poison shared cache?
    

### Impact

- Serve malicious data to users
    

---

## 8️⃣ Authorization Logic Tampering (Subtle)

### Checks

- Authorization check only on UI, not API
    
- Authorization check only on first request
    
- Role checked but **resource ownership not**
    

---

## 9️⃣ ML / AI Feature Tampering (Modern Apps)

### Checks

- Modify model inputs
    
- Poison feedback loops
    
- Change confidence thresholds
    

---

## 🔟 Supply Chain & Frontend Asset Tampering

### Checks

- Modify JS files in transit
    
- Third-party CDN trust
    
- Subresource Integrity (SRI) missing
    

---

## 1️⃣1️⃣ Time-Based Tampering

### Checks

- Expiry timestamps modifiable
    
- Race between validation & use
    
- Token still valid after logout
    

---

## 1️⃣2️⃣ Trust Boundary Misclassification (STRIDE Core)

### The Deep STRIDE Question

> “Did the developer _assume_ this data is safe because it crossed a boundary?”

Examples:

- Internal API assumed trusted
    
- Admin panel assumed safe
    
- Backend assumed frontend validated

# REPUDIATION 
Repudiation breaks **ACCOUNTABILITY** and **NON-REPUDIATION**.  
If an action **cannot be reliably tied to an identity**, it’s a repudiation risk.
## 1️⃣ Authentication Event Logging

### ✅ Checks

- Are **login attempts** logged?
    
- Are **failed logins** logged?
    
- Are log entries tied to:
    
    - User ID
        
    - Session ID
        
    - Timestamp
        
    - Source IP / device
        
- Are logs **tamper-resistant**?
    

### ❌ Red Flags

- Only success logged
    
- No timestamp
    
- Logs editable by app user
    

---

## 2️⃣ Critical Action Logging

### ✅ Actions That MUST Be Logged

- Password change
    
- Email change
    
- MFA enable/disable
    
- Role / permission change
    
- Money transfer
    
- Order placement
    
- Account deletion
    
- Admin actions
    

### 🔍 Checks

- Can these actions happen **without a log entry**?
    
- Is “who did it” clearly recorded?
    
- Is “what changed” recorded?
    

---

## 3️⃣ Log Integrity (VERY IMPORTANT)

### ✅ Checks

- Can logs be:
    
    - Modified?
        
    - Deleted?
        
    - Overwritten?
        
- Are logs protected by:
    
    - Append-only storage
        
    - Hash chaining
        
    - Centralized logging
        

### 🎯 Attack

- Attacker performs action → deletes logs → denies action
    

---

## 4️⃣ Session Attribution Failures

### ✅ Checks

- Is every action tied to a session?
    
- Are anonymous sessions allowed to perform actions?
    
- Can session IDs be reused across users?
    
- Are background jobs attributed to a user?
    

---

## 5️⃣ Weak or Missing User Attribution

### ✅ Checks

- Logs record only:
    
    - “Action happened”
        
    - but not **who** did it
        
- Usernames logged instead of immutable user IDs
    
- Logs rely on client-supplied identity
    

---

## 6️⃣ IP / Device Evidence Gaps

### ✅ Checks

- Is IP logged?
    
- Is proxy header trusted blindly?
    
- Is device fingerprinting used (when appropriate)?
    
- Are geo/location signals logged?
    

> ⚠️ IP alone is **not proof**, but lack of it is worse.

---

## 7️⃣ Timestamp & Time Sync Issues

### ✅ Checks

- Are timestamps:
    
    - Missing?
        
    - Client-controlled?
        
    - Inconsistent across services?
        
- Are clocks synchronized (NTP)?
    

### 🎯 Attack

- Attacker disputes time of action
    

---

## 8️⃣ Request & Parameter Logging

### ✅ Checks

- Are **parameters** logged for sensitive actions?
    
- Is before/after state logged?
    
- Are logs verbose enough to reconstruct event?
    

### Example

❌ `User updated profile`  
✅ `UserID=123 changed email from a@x.com → b@y.com`

---

## 9️⃣ Audit Trail Completeness

### ✅ Checks

- Can you trace:
    
    - Request → API → DB → Response?
        
- Is correlation ID used?
    
- Are logs searchable and centralized?
    

---

## 🔟 Client-Side–Only Evidence (Common Mistake)

### ✅ Checks

- Are important actions only recorded:
    
    - In frontend logs?
        
    - In browser storage?
        
- Is server authoritative?
    

> **Client-side logs = zero legal value**

---

## 1️⃣1️⃣ Replay & Reordering Issues

### ✅ Checks

- Can requests be replayed without detection?
    
- Are idempotency keys used?
    
- Can attacker replay old actions and deny original intent?
    

---

## 1️⃣2️⃣ Email / Notification Gaps

### ✅ Checks

- Are confirmation emails sent?
    
- Are they logged?
    
- Can attacker disable notifications and deny action?
    

---

## 1️⃣3️⃣ Admin & Support Repudiation

### ✅ Checks

- Are admin actions logged separately?
    
- Are support staff actions tracked?
    
- Is “acting on behalf of user” recorded?
    

---

## 1️⃣4️⃣ Third-Party Action Attribution

### ✅ Checks

- Are webhook-triggered actions logged?
    
- Is third-party identity recorded?
    
- Can attacker trigger external actions without trace?
    

---

## 1️⃣5️⃣ Log Injection & Confusion Attacks

### ✅ Checks

- Can attacker inject:
    
    - Newlines
        
    - Fake log entries
        
- Can attacker manipulate log format?
    

### 🎯 Example

`username=admin\nUser deleted account`

---

## 1️⃣6️⃣ Privacy vs Repudiation Balance

### ✅ Checks

- Are logs overly redacted?
    
- Is sensitive info removed **without losing attribution**?
    
- Is there a balance between GDPR & security?
    

---

## 1️⃣7️⃣ Log Retention & Availability

### ✅ Checks

- How long are logs kept?
    
- Can logs be purged early?
    
- Are logs available during investigations?

# Information Disclosure
Information Disclosure breaks **CONFIDENTIALITY**.  
If data is **accessible, inferable, or leaked**, it counts.
## 1️⃣ Sensitive Data in Responses

### ✅ Checks

- API responses include:
    
    - Password hashes
        
    - Tokens / API keys
        
    - Internal IDs
        
    - Internal flags (`isAdmin`, `isVerified`)
        
- Excessive fields returned
    

### 🧪 Tests

`GET /api/user/123`

Look for:

`"passwordHash": "...", "role": "admin"`

---

## 2️⃣ Broken Access Control (Read-Based)

### ✅ Checks

- Can I read:
    
    - Other users’ profiles?
        
    - Orders?
        
    - Messages?
        
    - Admin data?
        
- Change only **ID** → data leaks
    

### 🧪 Tests

`GET /api/orders/456`

---

## 3️⃣ IDOR (Insecure Direct Object Reference)

### ✅ Checks

- Predictable object IDs
    
- No ownership checks
    
- Read-only endpoints forgotten
    

### 🎯 Impact

- PII exposure
    
- Business data leaks
    

---

## 4️⃣ Error Message Disclosure

### ✅ Checks

- Stack traces
    
- SQL errors
    
- File paths
    
- Framework versions
    

### 🧪 Trigger Errors

- Invalid input
    
- Broken JSON
    
- Missing parameters
    

---

## 5️⃣ Debug / Test Endpoints

### ✅ Checks

- `/debug`
    
- `/test`
    
- `/admin`
    
- `/actuator`
    
- `/metrics`
    
- `/health`
    

---

## 6️⃣ Source Code & Configuration Leaks

### ✅ Checks

- `.env`
    
- `.git/`
    
- `config.yml`
    
- `web.config`
    
- Backup files
    

### 🧪 Examples

`GET /.env GET /.git/config`

---

## 7️⃣ Authentication Bypass = Info Disclosure

### ✅ Checks

- Data accessible without login
    
- Partial auth bypass leaks limited data
    

---

## 8️⃣ Token & Credential Leakage

### ✅ Checks

- Tokens in:
    
    - URLs
        
    - Referer headers
        
    - Logs
        
- JWTs readable by client
    
- Long-lived tokens
    

---

## 9️⃣ Transport Layer Leakage (TLS Issues)

### ✅ Checks

- HTTP instead of HTTPS
    
- Mixed content
    
- No HSTS
    
- TLS downgrade
    

---

## 🔟 Browser Storage Leaks

### ✅ Checks

- Sensitive data in:
    
    - localStorage
        
    - sessionStorage
        
    - IndexedDB
        
- XSS → instant data theft
    

---

## 1️⃣1️⃣ Caching & CDN Leaks

### ✅ Checks

- Authenticated pages cached
    
- Missing `Cache-Control: private`
    
- Shared caches leaking user data
    

---

## 1️⃣2️⃣ File Download & Directory Listing

### ✅ Checks

- Unrestricted file access
    
- Path traversal
    
- Predictable filenames
    

---

## 1️⃣3️⃣ Backup & Log File Exposure

### ✅ Checks

- `.log`
    
- `.bak`
    
- `.old`
    
- `~`
    

---

## 1️⃣4️⃣ GraphQL Overexposure

### ✅ Checks

- Introspection enabled
    
- Excessive fields
    
- Nested queries leaking data
    

---

## 1️⃣5️⃣ Search & Enumeration Leaks

### ✅ Checks

- User enumeration
    
- Order enumeration
    
- Search results revealing private info
    

---

## 1️⃣6️⃣ Metadata & Side-Channel Leaks

### ✅ Checks

- Response timing
    
- Content length differences
    
- HTTP status differences
    

---

## 1️⃣7️⃣ Third-Party Integration Leaks

### ✅ Checks

- Webhook data exposure
    
- Analytics leaks
    
- Third-party scripts exfiltrating data
    

---

## 1️⃣8️⃣ Mobile / API-Specific Leaks

### ✅ Checks

- Hidden API endpoints
    
- Versioned APIs exposing old data
    
- Mobile apps trusting client
    

---

## 1️⃣9️⃣ Infrastructure & Cloud Leaks

### ✅ Checks

- AWS metadata (`169.254.169.254`)
    
- Internal IPs in responses
    
- Cloud storage buckets public
    

---

## 2️⃣0️⃣ Logging as Information Disclosure

### ✅ Checks

- Sensitive data in logs
    
- Logs accessible via web
    
- Log aggregation dashboards exposed
## 1️⃣ Inference Attacks (Data You Never See Directly)

### Checks

- Can I infer:
    
    - If a user exists?
        
    - If an email is registered?
        
    - If a transaction happened?
        
- Compare:
    
    - Status codes
        
    - Response time
        
    - Message wording
        

### Example

`"User not found" vs "Invalid password"`

> Even **yes/no knowledge** is disclosure.

---

## 2️⃣ Timing & Side-Channel Leaks

### Checks

- Login takes longer for valid usernames?
    
- Password reset responses differ?
    
- Encryption/decryption timing differences?
    

> Timing = data.

---

## 3️⃣ HTTP Status Code Disclosure

### Checks

- 401 vs 403 reveals auth state
    
- 404 vs 403 reveals resource existence
    

---

## 4️⃣ Content-Length & Compression Leaks

### Checks

- Response size changes with secret data?
    
- BREACH-style leaks (compressed responses + secrets)
    

---

## 5️⃣ GraphQL Introspection Abuse (Deeper)

### Checks

- Schema reveals:
    
    - Internal models
        
    - Hidden fields
        
    - Admin-only mutations
        
- Error messages expose resolver logic
    

---

## 6️⃣ Feature Flag & A/B Testing Leaks

### Checks

- Flags reveal:
    
    - Unreleased features
        
    - Internal roadmap
        
    - Admin-only flows
        
- Can flags be enumerated?
    

---

## 7️⃣ Password Reset & Recovery Leaks

### Checks

- Reset endpoints reveal:
    
    - Account existence
        
    - MFA status
        
    - Linked auth providers
        

---

## 8️⃣ Search Engine & Indexing Leaks

### Checks

- Sensitive pages indexed?
    
- `robots.txt` revealing admin paths
    
- Cached versions of private pages
    

---

## 9️⃣ Object Relationship Leaks

### Checks

- From one object, can you discover:
    
    - Other users’ IDs?
        
    - Internal relationships?
        
- “Pivoting” from allowed data to hidden data
    

---

## 🔟 Data Residue & “Forgotten” Data

### Checks

- Deleted accounts still accessible?
    
- Old exports still downloadable?
    
- Archived data reachable via old endpoints?
    

---

## 1️⃣1️⃣ Mobile + Web Parity Leaks

### Checks

- Mobile API returns more fields than web
    
- Old mobile versions expose deprecated fields
    

---

## 1️⃣2️⃣ Observability Tool Exposure

### Checks

- Prometheus
    
- Grafana
    
- Kibana
    
- Sentry
    
- New Relic
    

> These often expose **stack traces + secrets**.

---

## 1️⃣3️⃣ CDN & Edge Case Leaks

### Checks

- Cached error pages
    
- Vary header misconfiguration
    
- Host header cache poisoning
    

---

## 1️⃣4️⃣ Third-Party Script Exfiltration

### Checks

- Analytics capturing PII
    
- Session replay tools
    
- Tag managers leaking secrets
    

---

## 1️⃣5️⃣ Legal / Compliance Disclosure

### Checks

- GDPR exports reveal internal data
    
- Audit exports expose more than requested
# Denial of Service 
### **“Make the service unavailable or unreliable for legitimate users”**

> DoS breaks **AVAILABILITY**.  
> The goal is **resource exhaustion, state corruption, or forced crashes**.

⚠️ **Ethical note**: These checks are for **authorized testing only**.

## 1️⃣ Application-Layer DoS (L7 – The Dangerous One)

### Checks

- Endpoints with:
    
    - Heavy DB queries
        
    - Expensive joins
        
    - Full-table scans
        
- Search endpoints with wildcards
    
- Regex-based filters
    

### Tests

- Empty search
    
- `*` wildcard
    
- Extremely long search strings
    

🎯 Impact: DB CPU exhaustion

---

## 2️⃣ Algorithmic Complexity Attacks

### Checks

- Sorting user-controlled input
    
- Nested loops with attacker data
    
- Regex backtracking (ReDoS)
    

### Tests

`(a+)+$ aaaaaaaaaaaaaaaaaaaa!`

---

## 3️⃣ Authentication DoS

### Checks

- Unlimited login attempts
    
- Password hashing cost abuse
    
- Account lockout abuse
    

🎯 Impact:

- CPU exhaustion
    
- User lockouts
    

---

## 4️⃣ File Upload / Processing DoS

### Checks

- Large file uploads
    
- Image decompression bombs
    
- Zip bombs
    
- Document parsing (PDF, DOCX)
    

### Tests

- 10MB → 1GB decompressed files
    

---

## 5️⃣ Memory Exhaustion

### Checks

- Large JSON payloads
    
- Deeply nested objects
    
- Infinite recursion risks
    

### Example

`{ "a": { "a": { "a": { "a": {...}}}} }`

---

## 6️⃣ Thread / Worker Exhaustion

### Checks

- Long-running requests
    
- Blocking I/O
    
- Synchronous external API calls
    

🎯 Impact:

- Worker starvation
    
- Queue backlog
    

---

## 7️⃣ Database Connection Pool Exhaustion

### Checks

- Endpoints that:
    
    - Open DB connections early
        
    - Don’t close on error
        
- Parallel slow queries
    

---

## 8️⃣ Cache Abuse & Eviction Attacks

### Checks

- Cache key influenced by user input
    
- Cache poisoning + eviction
    
- Large keyspace generation
    

---

## 9️⃣ Rate Limiting Bypass

### Checks

- Missing rate limits
    
- Limits per IP only
    
- Limits bypassable via:
    
    - Header spoofing
        
    - Account creation
        
    - Distributed requests
        

---

## 🔟 Resource Amplification Attacks

### Checks

- Small request → large response
    
- Pagination limits missing
    
- Export endpoints (CSV/PDF)
    

---

## 1️⃣1️⃣ WebSocket / SSE DoS

### Checks

- Unlimited connections
    
- Idle connection handling
    
- Message flood without limits
    

---

## 1️⃣2️⃣ Background Job & Queue DoS

### Checks

- User-triggered jobs:
    
    - Reports
        
    - Emails
        
    - Exports
        
- No deduplication
    
- No quotas
    

---

## 1️⃣3️⃣ Third-Party Dependency DoS

### Checks

- External APIs called synchronously
    
- No timeout / circuit breaker
    
- Failure cascades
    

---

## 1️⃣4️⃣ DNS & Dependency Chain DoS

### Checks

- App blocks on DNS resolution
    
- No caching
    
- External DNS dependency abuse
    

---

## 1️⃣5️⃣ Storage Exhaustion

### Checks

- Unlimited uploads
    
- Log growth
    
- Temp file leaks
    

---

## 1️⃣6️⃣ Session & State Explosion

### Checks

- Server-side sessions
    
- No session TTL
    
- Unlimited sessions per user/IP
    

---

## 1️⃣7️⃣ Locking & Race Condition DoS

### Checks

- Global locks
    
- DB row locks
    
- Deadlock scenarios
    

---

## 1️⃣8️⃣ HTTP Protocol Abuse

### Checks

- HTTP/1.1 slowloris
    
- Large headers
    
- Chunked encoding abuse
    

---

## 1️⃣9️⃣ GraphQL DoS (Advanced)

### Checks

- Deep nesting
    
- Circular fragments
    
- Introspection abuse
    
- Query cost limits missing
    

---

## 2️⃣0️⃣ API Versioning DoS

### Checks

- Old endpoints unprotected
    
- Deprecated APIs without limits
    

---

## 2️⃣1️⃣ Error Handling DoS

### Checks

- Exceptions expensive to handle
    
- Stack traces generated repeatedly
    

---

## 2️⃣2️⃣ Cloud-Specific DoS

### Checks

- Auto-scaling abuse (cost DoS)
    
- Lambda concurrency exhaustion
    
- Cold start abuse
    

---

## 2️⃣3️⃣ Business Logic DoS

### Checks

- Actions that:
    
    - Lock accounts
        
    - Pause services
        
    - Block workflows
        

🎯 Example:

- Locking many accounts intentionally
# Elevation of privilege
### **“Gain permissions you were never meant to have”**

> EoP breaks **AUTHORIZATION**.  
> If a low-privileged user can perform high-privileged actions → critical.
> 
## 🧭 STEP 1: Map the Privilege Model (You Can’t Break What You Don’t Understand)

### ✅ Identify

- Roles:
    
    - Guest
        
    - User
        
    - Premium
        
    - Support
        
    - Admin
        
    - Super Admin
        
- Permissions per role
    
- Horizontal vs Vertical privileges
    

### ❗ Common Mistake

- Developers think **role = permission**
    
- Attackers think **endpoint = permission**
    

---

## 🧭 STEP 2: Find All Privileged Entry Points

### ✅ Look For

- `/admin`
    
- `/manage`
    
- `/internal`
    
- `/staff`
    
- `/debug`
    
- Hidden API routes
    
- GraphQL mutations
    

### 🧪 Tests

`GET /admin/users`

---

## 🧭 STEP 3: Horizontal → Vertical Escalation

### ✅ Checks

- Can a normal user:
    
    - Access admin APIs?
        
    - Read admin-only data?
        
- Modify IDs only
    

### 🧪 Tests

`GET /api/admin/users Authorization: user_token`

---

## 🧭 STEP 4: Missing Server-Side Authorization (Most Common)

### ✅ Checks

- Authorization done:
    
    - In frontend only ❌
        
    - In middleware only for some routes ❌
        
- Authorization missing on:
    
    - PATCH
        
    - DELETE
        
    - Bulk endpoints
        

---

## 🧭 STEP 5: Role / Permission Tampering

### ✅ Checks

- Can you:
    
    - Modify role in request?
        
    - Inject new permissions?
        
- Is role derived from:
    
    - Client input ❌
        
    - Token claim without validation ❌
        

### 🧪 Tests

`{   "role": "admin" }`

---

## 🧭 STEP 6: JWT & Token-Based Privilege Escalation

### ✅ Checks

- Modify JWT claims:
    
    - `role`
        
    - `scope`
        
    - `isAdmin`
        
- Weak signature verification
    
- `alg=none`
    
- Token reuse across users
    

---

## 🧭 STEP 7: IDOR → Privilege Escalation

### ✅ Checks

- User owns object?
    
- Can object grant privileges?
    
- Modify:
    
    - teamId
        
    - orgId
        
    - projectId
        

### 🎯 Example

Change orgId → become org admin

---

## 🧭 STEP 8: Mass Assignment → EoP

### ✅ Checks

- API accepts fields user shouldn’t control
    
- Overposting creates admin users
    

### 🧪 Tests

`{   "email": "x@x.com",   "role": "admin" }`

---

## 🧭 STEP 9: Business Logic EoP

### ✅ Checks

- Can attacker:
    
    - Assign themselves as owner?
        
    - Approve their own requests?
        
    - Skip approval workflows?
        

### 🎯 Example

Submit → approve → execute as same user

---

## 🧭 STEP 🔟 Multi-Step Flow Escalation

### ✅ Checks

- Skip steps
    
- Replay admin-only steps
    
- Execute final step directly
    

---

## 🧭 STEP 11: Feature Flag Abuse

### ✅ Checks

- Enable admin features via flags
    
- Flags controlled client-side
    
- Flags not revalidated server-side
    

---

## 🧭 STEP 12: Admin Impersonation / Support Abuse

### ✅ Checks

- “Login as user” features
    
- Missing audit logs
    
- No approval or MFA for impersonation
    

---

## 🧭 STEP 13: Third-Party Integration Escalation

### ✅ Checks

- OAuth scopes too broad
    
- Webhooks triggering admin actions
    
- External identities mapped to admin roles
    

---

## 🧭 STEP 14: Multi-Tenancy Escalation (Very Common)

### ✅ Checks

- Access other tenants
    
- Become tenant admin
    
- Shared resource misbinding
    

---

## 🧭 STEP 15: GraphQL Privilege Escalation

### ✅ Checks

- Admin-only mutations accessible
    
- Resolver-level auth missing
    
- Introspection reveals hidden mutations
    

---

## 🧭 STEP 16: Race Conditions → EoP

### ✅ Checks

- Concurrent role changes
    
- TOCTOU vulnerabilities
    
- Double submission grants privileges
    

---

## 🧭 STEP 17: Caching & State Confusion

### ✅ Checks

- Privileged response cached
    
- User receives admin response
    
- Session role not refreshed
    

---

## 🧭 STEP 18: Legacy / Deprecated Endpoint Escalation

### ✅ Checks

- Old APIs without auth
    
- Mobile-only endpoints
    
- Beta features exposed
    

---

## 🧭 STEP 19: Misconfigured Middleware & Trust Boundaries

### ✅ Checks

- Admin checks only at gateway
    
- Internal APIs exposed
    
- Microservice trust abuse
    

---

## 🧭 STEP 20: Infrastructure-Assisted EoP

### ✅ Checks

- SSRF → metadata → admin creds
    
- Debug consoles exposed
    
- CI/CD secrets abuse
    

---

## 🧠 FINAL ATTACKER QUESTIONS (THE REAL TEST)

Ask these **in order**:

1. What privilege do I have?
    
2. What privilege exists?
    
3. Where is it checked?
    
4. Can I reach it directly?
    
5. Can I trick the system into granting it?
    

If you can answer all five → **you’ll find EoP**.

---

## 🔗 OWASP Mapping

|EoP|OWASP|
|---|---|
|Missing authZ|A01 – Broken Access Control|
|Mass assignment|A01|
|Token abuse|A07 – Identification & Authentication Failures|
|Business logic|A04 – Insecure Design|

---

## 🔥 GOLD RULE (FINAL STRIDE RULE)

> **Authentication tells you who you are.  
> Authorization tells you what you can do.  
> Elevation of Privilege happens when the second one lies.**

