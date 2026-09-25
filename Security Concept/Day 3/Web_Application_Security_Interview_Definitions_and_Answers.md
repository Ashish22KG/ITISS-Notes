# Web & Application Security — Interview Definitions & Answers

This guide covers the core web-security concepts that are important for **VAPT, Web Application Pentesting, Bug Bounty, SOC, AppSec, Red Team, and Security Engineering interviews**.

For each topic, use the interview pattern:

**Definition → How it works → Security risk → Prevention → Example**

---

# 1. HTTP Request / Response

## Definition

HTTP (Hypertext Transfer Protocol) is an application-layer protocol used for communication between clients and web servers.

A typical interaction contains:

```text
Client
   ↓
HTTP Request
   ↓
Web Server
   ↓
HTTP Response
   ↓
Client
```

### HTTP Request

An HTTP request can contain:

- Method
- URL/path
- HTTP version
- Headers
- Cookies
- Query parameters
- Request body

Example:

```http
GET /profile HTTP/1.1
Host: example.com
Cookie: session=abc123
```

### HTTP Response

An HTTP response can contain:

- Status code
- Headers
- Response body
- Cookies through `Set-Cookie`

Example:

```http
HTTP/1.1 200 OK
Content-Type: text/html

<html>...</html>
```

**Interview Answer:**

"An HTTP request is sent by a client to request a resource or perform an operation, and the server returns an HTTP response containing a status code, headers, and optionally a response body. During web security testing, I inspect both requests and responses for authentication, authorization, input validation, and security-control weaknesses."

---

# 2. Cookies

## Definition

A cookie is a small piece of data stored by a browser and sent with matching HTTP requests.

Example:

```http
Set-Cookie: session=abc123; Secure; HttpOnly; SameSite=Lax
```

### Important Cookie Attributes

| Attribute | Purpose |
|---|---|
| `Secure` | Sends cookie over HTTPS |
| `HttpOnly` | Prevents JavaScript from directly reading the cookie |
| `SameSite` | Controls cross-site cookie sending |
| `Domain` | Controls applicable domain |
| `Path` | Controls applicable URL path |
| `Expires` / `Max-Age` | Controls cookie lifetime |

**Interview Answer:**

"Cookies allow web applications to maintain client-side state, commonly for sessions or preferences. Sensitive session cookies should use appropriate security attributes such as Secure, HttpOnly, and an appropriate SameSite policy."

---

# 3. Sessions

## Definition

A session is server-side state used to maintain a user's authenticated or application state across multiple HTTP requests.

Because HTTP is stateless, applications commonly use a session identifier, often stored in a cookie.

```text
Login
  ↓
Server creates session
  ↓
Session ID sent to browser
  ↓
Browser sends session ID with requests
  ↓
Server identifies the session
```

**Interview Answer:**

"HTTP is stateless, so web applications commonly use sessions to maintain state between requests. After authentication, the server creates a session and associates it with the user. The browser then sends a session identifier with subsequent requests."

**Security Risks:**

- Session fixation
- Session hijacking
- Session theft
- Weak session IDs
- Failure to invalidate sessions
- Missing cookie security attributes

---

# 4. JWT

## Definition

JWT (JSON Web Token) is a compact token format commonly used to transmit claims between parties.

A JWT typically contains:

```text
Header.Payload.Signature
```

Example structure:

```text
xxxxx.yyyyy.zzzzz
```

**Interview Answer:**

"JWT is a token format containing claims that can be digitally signed and, in some designs, encrypted. A server can validate the token and use its claims for authentication or authorization decisions."

**Security Risks:**

- Weak signing secrets
- Incorrect algorithm handling
- Accepting forged tokens
- Excessive token lifetime
- Sensitive data stored in readable claims
- Failure to validate claims such as issuer, audience, or expiration

**Important:**

A normally signed JWT is **encoded, not encrypted**. Its payload should not be treated as confidential merely because it is a JWT.

---

# 5. Authentication

## Definition

Authentication is the process of verifying the identity of a user, device, or system.

**Examples:**

- Password
- MFA
- Certificate
- Security key
- Biometrics

**Interview Answer:**

"Authentication verifies who an entity is. For example, a web application may authenticate a user using a username, password, and MFA before creating an authenticated session."

---

# 6. Authorization

## Definition

Authorization determines what an authenticated user or system is allowed to access or perform.

**Interview Answer:**

"Authentication answers 'Who are you?' while authorization answers 'What are you allowed to do?' A secure application must enforce authorization on the server side for every protected resource or operation."

**Example:**

```text
Authentication:
User = Ashish

Authorization:
Ashish can view his profile
Ashish cannot access another user's admin settings
```

---

# 7. CORS

## Definition

CORS (Cross-Origin Resource Sharing) is a browser security mechanism that allows a web server to specify which origins may access its resources through browser-based cross-origin requests.

**Interview Answer:**

"CORS controls browser access to cross-origin resources. The server communicates its CORS policy through HTTP response headers such as `Access-Control-Allow-Origin`."

**Security Risk:**

An overly permissive CORS configuration can expose sensitive responses to an unauthorized origin, particularly when credentials are involved.

**Important:**

CORS is primarily a **browser-enforced access-control mechanism**. It is not a replacement for server-side authentication or authorization.

---

# 8. Same-Origin Policy

## Definition

The Same-Origin Policy (SOP) is a browser security mechanism that restricts how scripts from one origin can interact with resources from another origin.

An origin is generally defined by:

```text
Scheme + Host + Port
```

Example:

```text
https://example.com:443
```

**Interview Answer:**

"The Same-Origin Policy isolates web origins in browsers and restricts cross-origin script interactions. CORS provides a controlled mechanism for servers to relax certain cross-origin restrictions."

---

# 9. CSRF

## Definition

CSRF (Cross-Site Request Forgery) is a vulnerability where an attacker causes a victim's browser to send an unintended authenticated request to a target application.

**Typical condition:**

```text
Victim is logged in
       ↓
Browser automatically sends credentials
       ↓
Attacker causes a state-changing request
       ↓
Server accepts the request
```

**Interview Answer:**

"CSRF occurs when a web application accepts a state-changing request without sufficiently verifying that the request was intentionally initiated by the legitimate user."

**Prevention:**

- CSRF tokens
- Appropriate SameSite cookie settings
- Origin/Referer validation where appropriate
- Requiring appropriate re-authentication for sensitive actions

---

# 10. XSS

## Definition

XSS (Cross-Site Scripting) occurs when an application allows attacker-controlled content to execute as script in another user's browser.

### Types

#### Reflected XSS

Malicious input is immediately reflected in a response.

#### Stored XSS

Malicious content is stored by the application and later served to other users.

#### DOM-based XSS

Client-side JavaScript processes attacker-controlled data and creates an unsafe DOM operation.

**Interview Answer:**

"XSS occurs when untrusted data reaches a browser execution context without appropriate output encoding or sanitization. It can allow malicious JavaScript to execute in a victim's browser under the application's origin."

**Prevention:**

- Context-aware output encoding
- Safe DOM APIs
- Input validation
- HTML sanitization where HTML is intentionally allowed
- Content Security Policy as a defense-in-depth control

---

# 11. SQL Injection

## Definition

SQL Injection occurs when untrusted input is incorporated into SQL statements in an unsafe way, allowing an attacker to alter the intended database query.

Conceptually:

```text
User Input
    ↓
Application
    ↓
Unsafe SQL Construction
    ↓
Modified SQL Query
```

**Interview Answer:**

"SQL Injection occurs when attacker-controlled input changes the structure or meaning of a SQL query. It can potentially affect confidentiality, integrity, or availability of database data."

**Prevention:**

- Parameterized queries / prepared statements
- Safe ORM usage
- Input validation
- Least-privileged database accounts
- Avoid dynamic SQL where possible

**Example Concept:**

Instead of constructing SQL by concatenating user input, use a parameterized query:

```sql
SELECT * FROM users WHERE username = ?
```

---

# 12. Command Injection

## Definition

Command Injection occurs when attacker-controlled input reaches an operating-system command interpreter or command execution function in an unsafe manner.

**Interview Answer:**

"Command Injection occurs when untrusted input is interpreted as part of an operating-system command. An attacker may be able to execute commands with the privileges of the vulnerable application."

**Prevention:**

- Avoid shell execution when possible
- Use safe APIs instead of shell commands
- Strict allowlisting
- Proper argument separation
- Least privilege
- Input validation

**Security Impact:**

Potential impact can include:

- Data access
- System modification
- Credential exposure
- Lateral movement
- Full server compromise

---

# 13. SSRF

## Definition

SSRF (Server-Side Request Forgery) occurs when an attacker can influence a server into making requests to unintended destinations.

Conceptually:

```text
Attacker
   ↓
Vulnerable Web Application
   ↓
Internal / External Target
```

**Interview Answer:**

"SSRF occurs when a server-side application makes attacker-influenced requests without adequately restricting the destination. It can allow access to internal services or resources that are not directly reachable by the attacker."

**Potential Targets:**

- Internal web services
- Cloud metadata services
- Internal APIs
- Local services

**Prevention:**

- Strict destination allowlisting
- Network-level egress controls
- Validate and normalize URLs
- Restrict access to internal address ranges
- Disable unnecessary server-side network access

---

# 14. IDOR / BOLA

## IDOR

IDOR (Insecure Direct Object Reference) occurs when an application exposes an object reference and fails to enforce authorization when the reference is changed.

## BOLA

BOLA (Broken Object Level Authorization) is the API-focused term commonly used when an application fails to enforce authorization for object-level access.

**Example:**

```text
GET /api/users/100/profile
```

If user 100 changes the identifier to:

```text
GET /api/users/101/profile
```

and receives another user's data without authorization, the application may have a BOLA/IDOR vulnerability.

**Interview Answer:**

"IDOR or BOLA occurs when the application relies on an object identifier without properly checking whether the authenticated user is authorized to access that object."

**Prevention:**

- Server-side authorization checks
- Object ownership validation
- Deny-by-default access control
- Avoid relying on obscurity of IDs

---

# 15. File Upload Vulnerabilities

## Definition

A file upload vulnerability occurs when an application accepts files without adequately validating or safely processing them.

**Potential risks:**

- Malicious file execution
- Stored XSS through uploaded content
- Server-side code execution in unsafe configurations
- Path manipulation
- Malware hosting
- Denial of service
- Sensitive file exposure

**Interview Answer:**

"File upload vulnerabilities occur when an application does not securely validate, store, process, or serve uploaded files. Security should be applied to both the file type and the storage/execution behavior."

**Prevention:**

- Allowlist permitted file types
- Validate actual file content, not only extension
- Generate server-side filenames
- Store uploads outside executable web directories where possible
- Apply size limits
- Restrict permissions
- Scan files where appropriate
- Safely serve uploaded content

---

# 16. Path Traversal

## Definition

Path Traversal occurs when attacker-controlled input allows access to files or directories outside the application's intended directory.

Conceptually:

```text
Application Directory
       ↓
Attacker-controlled path
       ↓
Parent-directory traversal
       ↓
Unauthorized file access
```

**Interview Answer:**

"Path Traversal occurs when an application uses attacker-controlled file paths without safely restricting the resolved path to the intended directory."

**Prevention:**

- Avoid direct filesystem paths from user input
- Use allowlists or object identifiers
- Canonicalize paths
- Verify the resolved path remains within the intended directory
- Apply least-privilege filesystem permissions

---

# 17. XXE

## Definition

XXE (XML External Entity) is a vulnerability that can occur when an XML parser processes external entities from untrusted XML.

**Potential impact:**

- Local file disclosure
- SSRF
- Denial of service
- Internal resource access

**Interview Answer:**

"XXE occurs when an XML parser processes attacker-controlled external entities in an unsafe configuration. An attacker may abuse this behavior to access files or cause server-side requests."

**Prevention:**

- Disable external entity processing where unnecessary
- Use secure parser configurations
- Prefer safer data formats when appropriate
- Keep XML libraries updated

---

# 18. SSTI

## Definition

SSTI (Server-Side Template Injection) occurs when attacker-controlled input is interpreted as template code by a server-side template engine.

**Concept:**

```text
Attacker Input
     ↓
Template Engine
     ↓
Template Evaluation
     ↓
Unexpected Server-Side Behavior
```

**Interview Answer:**

"SSTI occurs when untrusted input is embedded into a server-side template in a way that allows the input to be interpreted as template syntax rather than ordinary data."

**Potential Impact:**

Depending on the template engine and configuration:

- Information disclosure
- Application manipulation
- Potential server-side code execution

**Prevention:**

- Never treat user input as template source
- Keep templates separate from data
- Use safe rendering mechanisms
- Restrict template functionality
- Apply least privilege

---

# 19. Open Redirect

## Definition

An open redirect occurs when an application redirects users to an attacker-controlled destination without sufficiently validating the destination.

Example concept:

```text
https://example.com/redirect?url=https://attacker.example
```

**Interview Answer:**

"An open redirect occurs when an application accepts an untrusted redirect destination and sends users there without adequate validation."

**Potential Risks:**

- Phishing
- Social engineering
- Abuse of trusted URLs
- OAuth-related attack chains in poorly designed implementations

**Prevention:**

- Use allowlisted destinations
- Prefer relative internal paths
- Validate redirect targets
- Avoid arbitrary user-controlled redirect URLs

---

# 20. Host Header Attacks

## Definition

A Host header attack occurs when an application incorrectly trusts the HTTP `Host` header for security-sensitive decisions.

Example:

```http
Host: attacker.example
```

**Potential impact depends on application design:**

- Password reset poisoning
- Cache poisoning
- Incorrect URL generation
- Routing manipulation
- Security-control bypass in poorly designed applications

**Interview Answer:**

"The Host header identifies the requested host in HTTP/1.1. A vulnerability can occur when an application trusts attacker-controlled Host header values for sensitive operations such as generating password-reset links."

**Prevention:**

- Validate allowed hostnames
- Do not use arbitrary Host values in security-sensitive URL generation
- Configure trusted proxy behavior correctly
- Validate forwarded-host headers when applicable

---

# 21. Authentication Flaws

## Definition

Authentication flaws occur when an application incorrectly implements identity verification.

**Examples:**

- Weak passwords
- Missing MFA for sensitive access
- Username enumeration
- Brute-force protection weaknesses
- Credential recovery flaws
- Improper session creation
- Authentication bypass
- Poor account lockout/rate limiting
- Insecure password storage

**Interview Answer:**

"Authentication flaws allow attackers to bypass or weaken identity verification. During testing, I examine login, registration, password reset, MFA, session creation, and recovery mechanisms."

**Prevention:**

- Strong password policies
- MFA
- Secure password hashing
- Rate limiting
- Secure account recovery
- Consistent authentication errors
- Session regeneration after authentication
- Monitoring and alerting

---

# 22. Session Attacks

## Definition

Session attacks target weaknesses in how applications create, store, transmit, or invalidate session identifiers.

**Common examples:**

- Session fixation
- Session hijacking
- Session token theft
- Session prediction
- Failure to invalidate sessions
- Excessive session lifetime

**Interview Answer:**

"Session attacks target weaknesses in session management. A secure application should generate unpredictable session identifiers, protect them in transit and storage, rotate them at important authentication boundaries, and invalidate them appropriately."

**Important Controls:**

- Secure random session IDs
- HTTPS
- Secure cookie attributes
- Session rotation
- Session expiration
- Logout invalidation
- Re-authentication for sensitive actions

---

# 23. Security Misconfiguration

## Definition

Security misconfiguration occurs when systems, applications, servers, or security controls are configured insecurely.

**Examples:**

- Default credentials
- Debug mode enabled
- Detailed error messages
- Unnecessary services
- Exposed administration interfaces
- Weak security headers
- Excessive permissions
- Unnecessary features
- Outdated components

**Interview Answer:**

"Security misconfiguration occurs when a system is deployed with insecure or unnecessary settings. I would review application configuration, server configuration, access controls, error handling, exposed services, and security headers."

**Prevention:**

- Secure configuration baselines
- Remove unnecessary components
- Disable debug functionality
- Change default credentials
- Least privilege
- Regular configuration reviews
- Automated security checks

---

# 24. Business Logic Vulnerabilities

## Definition

A business logic vulnerability occurs when an application technically accepts a request but the application's workflow or business rules can be abused to produce an unintended result.

**Examples:**

- Bypassing purchase restrictions
- Manipulating price or quantity
- Reusing one-time actions
- Skipping required workflow steps
- Abusing referral/discount logic
- Race conditions
- Circumventing approval processes

**Interview Answer:**

"Business logic vulnerabilities occur when the application fails to correctly enforce the intended business rules. Unlike many technical injection vulnerabilities, they often require understanding how the application's workflow is supposed to operate."

**Prevention:**

- Enforce business rules server-side
- Validate workflow state
- Prevent replay of one-time operations
- Use transaction controls
- Test negative and edge cases
- Apply authorization at every sensitive operation

---

# WEB SECURITY INTERVIEW RAPID-FIRE

## Q1. Authentication vs Authorization?

**Answer:**

"Authentication verifies identity, while authorization determines what that identity is allowed to access or perform."

---

## Q2. What is the difference between a cookie and a session?

**Answer:**

"A cookie is client-side data stored by the browser and sent with applicable requests. A session is application state maintained by the server. A session identifier is commonly stored in a cookie to associate requests with server-side session state."

---

## Q3. What is the difference between CSRF and XSS?

**Answer:**

"CSRF causes a victim's browser to perform an unintended action against an application where the victim is authenticated. XSS causes attacker-controlled script or script-like content to execute in the victim's browser under the application's origin."

---

## Q4. What is the difference between SQL Injection and Command Injection?

**Answer:**

"SQL Injection targets the structure of database queries, while Command Injection targets operating-system command execution."

---

## Q5. What is IDOR/BOLA?

**Answer:**

"IDOR/BOLA occurs when an application fails to enforce authorization when a user accesses a different object by changing an identifier or object reference."

---

## Q6. What is SSRF?

**Answer:**

"SSRF occurs when an attacker can influence a server to make a request to an unintended destination."

---

## Q7. What is the difference between CORS and SOP?

**Answer:**

"The Same-Origin Policy is a browser security restriction on cross-origin interactions. CORS is a mechanism that allows a server to specify which cross-origin browser requests or responses may be permitted."

---

## Q8. What is JWT?

**Answer:**

"JWT is a compact token format containing claims that can be signed and optionally encrypted. Applications can validate the token and use its claims for authentication or authorization decisions."

---

## Q9. What is path traversal?

**Answer:**

"Path traversal occurs when attacker-controlled path input allows access outside the intended directory."

---

## Q10. What is XXE?

**Answer:**

"XXE is a vulnerability caused by unsafe XML external entity processing. It can potentially lead to file disclosure, SSRF, or denial of service."

---

## Q11. What is SSTI?

**Answer:**

"SSTI occurs when attacker-controlled input is interpreted as server-side template syntax rather than ordinary data."

---

## Q12. What is an open redirect?

**Answer:**

"An open redirect allows an attacker to make the application redirect users to an attacker-controlled destination."

---

## Q13. What is a security misconfiguration?

**Answer:**

"It is an insecure system, application, or infrastructure configuration such as default credentials, debug mode, unnecessary services, excessive permissions, or exposed administration interfaces."

---

## Q14. What are business logic vulnerabilities?

**Answer:**

"They occur when an attacker can abuse an application's intended workflow or business rules to achieve an unintended result."

---

# WEB VULNERABILITY COMPARISON

| Vulnerability | Main Target | Typical Root Cause |
|---|---|---|
| XSS | Browser | Unsafe handling/output of untrusted data |
| SQL Injection | Database | Unsafe SQL construction |
| Command Injection | OS | Unsafe command execution |
| SSRF | Server-side network access | Unrestricted server-side requests |
| IDOR/BOLA | Authorization | Missing object-level access control |
| CSRF | User actions | Missing request-intent validation |
| Path Traversal | Filesystem | Unsafe file path handling |
| XXE | XML parser | Unsafe external entity processing |
| SSTI | Template engine | Untrusted template interpretation |
| Open Redirect | Redirect logic | Unvalidated destination |
| Host Header attack | HTTP routing/URL generation | Trusting unvalidated Host values |
| File Upload | File handling | Unsafe validation/storage/processing |
| Authentication flaw | Identity verification | Weak or broken authentication |
| Session attack | Session management | Weak token/session controls |
| Security misconfiguration | Deployment/configuration | Insecure settings |
| Business logic flaw | Application workflow | Incorrect business-rule enforcement |

---

# HOW TO APPROACH A WEB APPLICATION DURING A SECURITY TEST

For an authorized security assessment, think in this order:

```text
1. Understand the Application
          ↓
2. Identify Attack Surface
          ↓
3. Map Authentication
          ↓
4. Map Authorization
          ↓
5. Understand Sessions
          ↓
6. Identify Inputs
          ↓
7. Test Access Controls
          ↓
8. Test Input Handling
          ↓
9. Test File/URL Processing
          ↓
10. Test Business Logic
          ↓
11. Review Security Configuration
          ↓
12. Validate Findings and Impact
```

---

# BURP SUITE INTERVIEW CONNECTION

For web application pentesting, understand what you inspect in Burp Suite:

| Area | What to Look For |
|---|---|
| HTTP requests | Parameters, headers, cookies, methods |
| HTTP responses | Status codes, headers, body |
| Cookies | Secure, HttpOnly, SameSite, lifetime |
| Authentication | Login, MFA, password reset |
| Authorization | User/object access |
| Sessions | Token generation and invalidation |
| Input fields | Injection and validation issues |
| File uploads | Type, content, storage behavior |
| APIs | BOLA, authentication, authorization |
| Redirects | Open redirect behavior |
| CORS | Origin and credential behavior |
| Host header | Trust and URL generation |
| Errors | Information disclosure |
| Business logic | Workflow manipulation |

---

# SECURITY TESTING MINDSET

For every feature, ask:

### 1. Authentication

**"Who am I?"**

Can an attacker bypass authentication?

### 2. Authorization

**"What am I allowed to access?"**

Can one user access another user's data?

### 3. Input Validation

**"What happens if I provide unexpected input?"**

Could input reach a parser, database, operating system, template engine, or filesystem unsafely?

### 4. Session Management

**"How does the application identify me between requests?"**

Can sessions be stolen, fixed, predicted, or reused?

### 5. Business Logic

**"Can I perform actions in a way the developer did not intend?"**

### 6. Configuration

**"Is the application deployed securely?"**

---

# FINAL WEB SECURITY CHECKLIST

## HTTP & Web Fundamentals

- [ ] HTTP request
- [ ] HTTP response
- [ ] HTTP methods
- [ ] Status codes
- [ ] Headers
- [ ] Cookies
- [ ] Sessions
- [ ] JWT
- [ ] Authentication
- [ ] Authorization
- [ ] CORS
- [ ] Same-Origin Policy

## OWASP-Style Vulnerabilities

- [ ] CSRF
- [ ] XSS
- [ ] SQL Injection
- [ ] Command Injection
- [ ] SSRF
- [ ] IDOR / BOLA
- [ ] File Upload vulnerabilities
- [ ] Path Traversal
- [ ] XXE
- [ ] SSTI
- [ ] Open Redirect
- [ ] Host Header attacks
- [ ] Authentication flaws
- [ ] Session attacks
- [ ] Security misconfiguration
- [ ] Business logic vulnerabilities

## Interview Skills

- [ ] Explain each vulnerability in one minute
- [ ] Explain root cause
- [ ] Explain security impact
- [ ] Explain prevention
- [ ] Give a simple example
- [ ] Know how to identify the issue in an HTTP request/response
- [ ] Understand how Burp Suite helps investigate web vulnerabilities
- [ ] Understand authentication vs authorization
- [ ] Understand client-side vs server-side security controls

---

# INTERVIEW ANSWER FORMULA

For every vulnerability, use:

**1. Definition**

**2. How it happens**

**3. Impact**

**4. Prevention**

**5. Example**

### Example: IDOR / BOLA

**Definition:**  
"IDOR or BOLA occurs when an application fails to enforce authorization for an object."

**How it happens:**  
"The application trusts an object identifier supplied by the client without checking whether the authenticated user owns or is authorized to access that object."

**Impact:**  
"It can expose or modify another user's information."

**Prevention:**  
"Perform server-side object-level authorization checks on every sensitive request."

**Example:**  
"A user changes `/api/orders/1001` to `/api/orders/1002` and can access another customer's order without authorization."

---

# KEY TAKEAWAY

A strong web-security candidate should understand the complete flow:

```text
Browser
   ↓
HTTP Request
   ↓
Authentication
   ↓
Session / JWT
   ↓
Authorization
   ↓
Application Logic
   ↓
Input Processing
   ↓
Database / Files / Internal Services
   ↓
HTTP Response
```

Security testing asks whether an attacker can manipulate any part of this flow to:

- Bypass authentication
- Bypass authorization
- Execute unintended code
- Access unauthorized data
- Make the server access unintended resources
- Manipulate business workflows
- Abuse insecure configuration

For interviews, focus not only on memorizing vulnerability names but on understanding **where the trust boundary exists, what the root cause is, how the vulnerability affects confidentiality/integrity/availability, and how to prevent it**.
