###### https://docs.sysreptor.com/ :
	website for customizing reporting and make pdf
##### `Sysreptor basic guide : u can edit boilerplate as u want, in scope assets, out of scope assets.
## findings :
		- use suitable title for your findings (use chatgpt)
		- CWE (Common Weakness Enumeration) exactly name
		- cvss 

## description :
		What the _Description_ section is for
		Think of **Description** as:
 **“What is the vulnerability, where is it, and why does it exist?”**

	it should help:
		Developers understand the issue
	    Managers understand the context
	    Triagers quickly reproduce it (without full exploitation steps)
    
### What to include in Description ✅

### 1. **What the vulnerability is**
Name it clearly.
- “SQL Injection vulnerability”
- “Insecure Direct Object Reference (IDOR)”
- “Stored Cross-Site Scripting (XSS)”

### 2. **Where it occurs**
Be specific.
- Endpoint / URL
- Parameter name
- Function / feature affected

### 3. **Why it happens*
Root cause (brief).
- Lack of input validation
- Missing authorization checks
- Unsafe output handling
- Misconfiguration

### 4. **High-level exploitation context**
Explain **how it can be abused** without payload spam.

### Simple Description Template

> **Description:**  
> The application contains a **[vulnerability name]** in the **[feature/endpoint]**. The issue occurs because **[root cause]**, allowing an attacker to **[high-level abuse]**.

### Good Description Examples

#### 🔴 SQL Injection
> **Description:**  
> A SQL Injection vulnerability was identified in the `/login` endpoint within the `username` parameter. The application directly incorporates user-supplied input into SQL queries without proper parameterization or validation, allowing an attacker to manipulate database queries.
#### 🔴 Stored XSS
> **Description:**  
> A stored Cross-Site Scripting (XSS) vulnerability exists in the user comment functionality. User-supplied input is stored and later rendered to other users without proper output encoding, enabling the execution of arbitrary JavaScript in victims’ browsers.
#### 🔴 IDOR
> **Description:**  
> An Insecure Direct Object Reference (IDOR) vulnerability was discovered in the `/api/orders/{id}` endpoint. The application fails to verify whether the authenticated user is authorized to access the requested object, allowing unauthorized access to other users’ data.
#### 🔴 CSRF
> **Description:**  
> The application is vulnerable to Cross-Site Request Forgery (CSRF) due to the absence of anti-CSRF tokens on state-changing requests. This allows an attacker to trick authenticated users into executing unintended actions.
#### 🔴 Broken Access Control
> **Description:**  
> The application fails to enforce proper access control checks on administrative endpoints, allowing non-privileged users to access restricted functionality.

## impact : 
	An attacker can **[capability]**, leading to **[technical consequence]**, which may result in **[business risk]**.
	example:
		An attacker could execute arbitrary SQL queries on the backend database, allowing them to read, modify, or delete sensitive data such as user credentials and personal information. This could lead to a full data breach, account compromise, and severe reputational and financial damage to the organization.
## Remediation :
			- What _Remediation_ should answer
			**“What exact steps should the developer take to prevent this vulnerability?”**
	It should be:
	- Clear
	- Actionable
	- Defensive (future-proof)
	- Developer-friendly

### Remediation Examples (by vulnerability)
#### 🔴 SQL Injection
> **Remediation:**  
> Use parameterized queries (prepared statements) or ORM frameworks to ensure user input is not directly concatenated into SQL queries. Implement input validation and apply the principle of least privilege to database accounts. Additionally, enable database logging and monitoring to detect suspicious query activity.
#### 🔴 XSS (Stored / Reflected)
> **Remediation:**  
> Properly encode and escape all user-supplied input before rendering it in the browser based on the output context (HTML, JavaScript, URL). Implement a strong Content Security Policy (CSP) and avoid using dangerous functions such as `innerHTML`. Validate input on both client and server sides.
#### 🔴 IDOR
> **Remediation:**  
> Enforce strict server-side authorization checks to ensure users can only access resources they own or are permitted to access. Avoid relying on user-controlled identifiers alone and use indirect object references where possible. Conduct access control testing across all endpoints.
#### 🔴 Broken Authentication
> **Remediation:**  
> Implement secure authentication mechanisms including strong password policies, proper session management, and protection against brute-force attacks. Use multi-factor authentication where applicable and ensure session identifiers are securely generated, stored, and invalidated upon logout.
#### 🔴 CSRF
> **Remediation:**  
> Implement CSRF tokens for all state-changing requests and validate them on the server side. Ensure cookies are set with the `SameSite` attribute and avoid using GET requests for sensitive actions. Confirm user intent for critical operations.

## References :  
	reference to similar vulnerability
## Details
	What to include in **Details** ✅
#### 1. **Affected endpoint / feature**
Be precise.
- URL / API endpoint
- HTTP method
- Parameter name
- Auth state (authenticated / unauthenticated)
#### 2. **Request & response behavior**
Explain what happens when:
- Normal input is used
- Malicious / manipulated input is used
(You can include snippets, but keep them minimal.)
#### 3. **Observed result**
What changed?
- Data returned    
- Access granted
- Script executed
- Error messages exposed
#### 4. **Proof of concept (PoC)**
Short and clean.
- One request
- One payload
- One clear outcome
##### Clean **Details** Template

> **Details:**  
> The vulnerability was identified in the **[HTTP method]** request to **[endpoint]**. The **[parameter]** parameter accepts user-controlled input without proper validation.
> 
> When a crafted value is supplied, the application responds with **[unexpected behavior]**, confirming that the input is processed insecurely.
### Example Details (by vulnerability)
#### 🔴 SQL Injection
> **Details:**  
> A POST request to `/login` was observed to process the `username` parameter directly in a SQL query. Supplying a crafted input caused the application to return a successful login response without valid credentials, indicating that the input was not properly parameterized.
#### 🔴 Stored XSS
> **Details:**  
> Input submitted through the comment field is stored in the backend database and later rendered on the comments page. When HTML/JavaScript content is submitted, it is returned unencoded in the response and executed in the browser of any user viewing the page
#### 🔴 IDOR
> **Details:**  
> A GET request to `/api/profile/{id}` returns user profile information. Modifying the `id` value to another valid identifier returns data belonging to a different user, demonstrating the absence of server-side authorization checks.
#### 🔴 CSRF
> **Details:**  
> 	The password change endpoint accepts POST requests without requiring a CSRF token. A forged request submitted from a third-party origin successfully changed the victim’s account password while the user was authenticated.
