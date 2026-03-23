### OAuth 2.0: Purpose and Flow

- **Primary Use Case:** OAuth 2.0 was designed for **delegated authorization** — letting applications access user data on other services with explicit user consent, without sharing passwords.
    
- **Core Components:**
    
    |Term|Definition|Example|
    |---|---|---|
    |Resource Owner|The user who owns the data|You, the user|
    |Client|The application requesting access|Yelp app|
    |Authorization Server|The server that authenticates user and grants consent|[accounts.google.com](http://accounts.google.com/)|
    |Resource Server|The API holding the data|Google Contacts API|
    |Authorization Grant|Proof that user consent was given|Authorization Code|
    |Access Token|The key that grants access to specific resources|Bearer token to access contacts|
    
- **Authorization Code Flow (Most Common):**
    
    1. User clicks “Connect with Google” on client app.
    2. Browser redirects to Authorization Server for user login and consent.
    3. Authorization Server redirects back to client with an **authorization code** via browser (front channel).
    4. Client exchanges authorization code for an **access token** via server-to-server call (back channel).
    5. Client uses access token to access user data on Resource Server.
    
    - This two-step exchange enhances **security** by separating front and back channel communication, protecting sensitive tokens and secrets from exposure in the browser.
- **Scopes:** OAuth scopes define granular permissions (e.g., “read contacts,” “write contacts”). The authorization server uses scopes to generate the consent screen and to limit the access token’s privileges.
    
- **Implicit Flow:** For single-page apps without a backend (no back channel), OAuth can return the access token directly via the front channel, but this is less secure.
    
- **Other Flows:**
    
    - Resource Owner Password Credentials Flow (less recommended)
    - Client Credentials Flow (machine-to-machine communication)

---

### Challenges and Confusion Around OAuth

- OAuth 2.0 is complex and poorly documented for beginners, with conflicting and sometimes incorrect information online. The protocol’s flexibility leads to varied implementations, causing interoperability issues.
    
- OAuth was **not designed for authentication**, only authorization. Using OAuth for login (“social login”) is a hack, resulting in inconsistent and non-standardized ways to obtain user identity.
    

---

### OpenID Connect: Extending OAuth for Authentication

- **OpenID Connect (OIDC)** is a thin layer on top of OAuth 2.0 that standardizes authentication.
    
- **Key Addition:**
    
    - **ID Token:** A JWT (JSON Web Token) that contains verified user identity information (e.g., user ID, email). This token is cryptographically signed by the authorization server, allowing the client to verify the token’s authenticity without calling back to the server.
- **How OIDC Works:**
    
    - The client requests the OAuth scopes plus the special “openid” scope.
    - After user consent, the authorization server returns an ID token along with the access token.
    - The client decodes and verifies the ID token to authenticate the user.
    - Additional user info can be fetched from the **User Info Endpoint** if needed.
- OIDC fixes the problem of OAuth’s lack of user info and provides a **standardized, interoperable way to handle authentication**.
    

---

### Modern Use Cases and Recommendations

|Use Case|Recommended Protocol & Flow|Notes|
|---|---|---|
|Basic Web Login|OpenID Connect Authorization Code Flow|Client gets ID token + access token; backend stores tokens; session cookie management|
|Native Mobile Apps (iOS/Android)|OpenID Connect Authorization Code Flow + PKCE|PKCE (Proof Key for Code Exchange) enhances security on mobile devices|
|Single Page Apps (SPA)|OAuth 2.0 Implicit Flow (or Authorization Code Flow with PKCE if possible)|Implicit flow is less secure; mitigate token theft risks|
|Delegated Authorization|OAuth 2.0 Authorization Code Flow|Granular access to user resources without exposing credentials|

- Companies often use OAuth/OIDC providers like Okta to abstract complexity and integrate with legacy systems such as SAML.
    
- The **authorization server setup requires registering the client app, obtaining a client ID and client secret**, where the client secret must be kept confidential and used only in back-channel communication.
    

---

### Key Insights

- **OAuth 2.0 is for delegated authorization, not authentication.** Misusing it for login causes confusion and security pitfalls.
    
- **OpenID Connect is a standardized extension of OAuth 2.0 that enables secure user authentication.**
    
- The **authorization code flow** with back-channel exchange is the most secure and recommended method for web and mobile apps.
    
- **Scopes enable fine-grained access control**, allowing users to consent to specific permissions.
    
- The separation of **front channel (browser) and back channel (server-to-server)** communication is critical for securing tokens and client secrets.
    
- Tools like **OAuth debugger** and platforms such as **Okta** can help developers implement and test OAuth/OIDC flows correctly.
    

---

### Additional Resources

- [OAuth.com](http://oauth.com/): Free detailed eBook on OAuth 2.0 protocol including token revocation and validation.
    
- Okta Developer Platform: Free cloud-based authorization server to experiment with OAuth and OpenID Connect.
    
- [JWT.io](http://jwt.io/): Tool for decoding JSON Web Tokens (ID tokens).