# HTTP Request/Response & Cookies — Security Interview Revision

## 1. HTTP Basics

### What is HTTP?

**HTTP (Hypertext Transfer Protocol)** is an application-layer protocol used for communication between a client and a web server.

Typical flow:

```text
Browser
   |
   | HTTP Request
   v
Web Server
   |
   | HTTP Response
   v
Browser
```

Common ports:

- HTTP → `80`
- HTTPS → `443`

### Interview Answer

> HTTP is an application-layer protocol used for communication between clients and web servers. The client sends an HTTP request, and the server processes it and returns an HTTP response containing information such as a status code, headers, and optionally a response body.

---

# 2. HTTP Request

An HTTP request is a message sent by a client to a server asking it to perform an action or return a resource.

Structure:

```text
HTTP Request
|
├── Request Line
├── Headers
├── Blank Line
└── Body (optional)
```

Example:

```http
GET /login HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: text/html
Cookie: session=abc123
```

---

# 3. Request Line

Example:

```http
GET /login HTTP/1.1
```

Contains:

```text
GET       /login       HTTP/1.1
 |          |             |
Method     Path        HTTP Version
```

## Common HTTP Methods

| Method | Purpose |
|---|---|
| GET | Retrieve data |
| POST | Submit/create data |
| PUT | Replace/update a resource |
| PATCH | Partially update a resource |
| DELETE | Delete a resource |
| HEAD | Get headers without response body |
| OPTIONS | Ask what methods/options are supported |

---

# 4. GET Request

Example:

```http
GET /products?id=10 HTTP/1.1
Host: example.com
```

The parameter is in the URL:

```text
/products?id=10
         └── Query parameter
```

Multiple parameters:

```http
GET /search?q=laptop&sort=price HTTP/1.1
```

### Security Concern

Sensitive information should generally not be placed in URLs because URLs may appear in:

- Browser history
- Server/proxy logs
- Analytics systems
- Referrer-related information

Bad example:

```http
GET /login?username=ashish&password=123456
```

---

# 5. POST Request

POST commonly sends data in the request body.

Example:

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 27

username=ashish&password=test
```

### Important

POST does **not** automatically make data secure.

HTTPS/TLS provides encryption in transit.

---

# 6. HTTP Headers

Headers provide additional information about a request or response.

Example:

```http
Host: example.com
User-Agent: Mozilla/5.0
Accept: text/html
Content-Type: application/json
Authorization: Bearer eyJ...
Cookie: session=abc123
```

## Important Security-Relevant Request Headers

### Host

```http
Host: example.com
```

Identifies the target host.

Security relevance:

- Host header attacks
- Virtual hosting
- Some password-reset poisoning scenarios

### User-Agent

```http
User-Agent: Mozilla/5.0
```

Identifies the client/software.

**Security note:** Do not trust it for authentication because attackers can modify it.

### Cookie

```http
Cookie: session=abc123
```

Sends stored cookies to the server.

Security relevance:

- Session hijacking
- Session fixation
- Cookie theft
- Authentication attacks

### Authorization

```http
Authorization: Bearer eyJ...
```

Often carries authentication credentials/tokens.

Security relevance:

- Token theft
- Broken authentication
- Improper authorization

### Content-Type

Specifies the format of the request body.

Example:

```http
Content-Type: application/json
```

Body:

```json
{
  "username": "ashish",
  "password": "test123"
}
```

Common values:

```text
application/json
application/x-www-form-urlencoded
multipart/form-data
text/plain
```

---

# 7. Request Body

The body contains data sent to the server.

Example:

```http
POST /api/login HTTP/1.1
Host: example.com
Content-Type: application/json

{
    "username": "ashish",
    "password": "test123"
}
```

Commonly used with:

- POST
- PUT
- PATCH

### Security Testing

A pentester may manipulate body parameters:

```json
{
  "username": "ashish",
  "role": "admin"
}
```

If the server improperly trusts client-controlled fields, vulnerabilities such as:

- Privilege escalation
- Mass assignment
- Broken access control

may occur.

---

# 8. HTTP Response

A response is sent by the server after processing the request.

Example:

```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1250
Set-Cookie: session=abc123; Secure; HttpOnly

<html>
    <body>Welcome</body>
</html>
```

Structure:

```text
HTTP Response
|
├── Status Line
├── Response Headers
├── Blank Line
└── Response Body
```

---

# 9. Status Line

Example:

```http
HTTP/1.1 200 OK
```

Contains:

```text
HTTP/1.1    200    OK
   |          |      |
Version    Status   Reason
```

---

# 10. HTTP Status Codes

## 1xx — Informational

| Code | Meaning |
|---|---|
| 100 | Continue |

## 2xx — Success

| Code | Meaning |
|---|---|
| 200 | OK |
| 201 | Created |
| 204 | No Content |

## 3xx — Redirection

| Code | Meaning |
|---|---|
| 301 | Permanent Redirect |
| 302 | Temporary Redirect |
| 304 | Not Modified |
| 307 | Temporary Redirect |
| 308 | Permanent Redirect |

Example:

```http
HTTP/1.1 302 Found
Location: /login
```

Security relevance:

- Open redirects
- Authentication-flow manipulation
- OAuth-related attacks

## 4xx — Client Errors

| Code | Meaning |
|---|---|
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 405 | Method Not Allowed |
| 429 | Too Many Requests |

## 5xx — Server Errors

| Code | Meaning |
|---|---|
| 500 | Internal Server Error |
| 502 | Bad Gateway |
| 503 | Service Unavailable |
| 504 | Gateway Timeout |

A poorly handled `500` response can expose:

- Stack traces
- File paths
- Framework information
- Database errors
- Debug information

---

# 11. 401 vs 403

### 401 Unauthorized

Usually indicates that valid authentication credentials are missing or invalid.

Think:

> **Who are you?**

### 403 Forbidden

The server understood the request but refuses to authorize it.

Think:

> **I know who you are, but you are not allowed to do this.**

The exact behavior can vary by application.

---

# 12. Response Headers

Example:

```http
Content-Type: text/html
Content-Length: 1250
Set-Cookie: session=abc123; HttpOnly; Secure
Location: /dashboard
Strict-Transport-Security: max-age=31536000
Content-Security-Policy: default-src 'self'
```

Important security headers include:

### Set-Cookie

Tells the browser to create/update a cookie.

### Content-Security-Policy

```http
Content-Security-Policy: default-src 'self'
```

Controls which resources the browser can load and can reduce the impact of certain XSS attacks.

### Strict-Transport-Security

```http
Strict-Transport-Security: max-age=31536000
```

Instructs the browser to use HTTPS for the specified period.

### X-Content-Type-Options

```http
X-Content-Type-Options: nosniff
```

Helps prevent MIME-type sniffing.

---

# 13. Response Body

The response body contains data returned by the server.

It can contain:

- HTML
- JSON
- XML
- JavaScript
- Images
- Files
- API data

Example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "username": "ashish",
    "role": "user"
}
```

---

# 14. Complete HTTP Request/Response Example

## Request

```http
POST /api/login HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Content-Type: application/json
Cookie: session=abc123

{
    "username": "ashish",
    "password": "test123"
}
```

## Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
Set-Cookie: session=xyz789; Secure; HttpOnly; SameSite=Lax

{
    "message": "Login successful",
    "role": "user"
}
```

---

# 15. HTTP vs HTTPS

## HTTP

HTTP does not provide encryption by itself.

## HTTPS

HTTPS is HTTP protected using TLS.

TLS provides protections such as:

- Confidentiality
- Integrity
- Server authentication

Conceptually:

```text
Client ===== TLS ===== Server
```

### Interview Answer

> HTTPS is HTTP communication protected by TLS. TLS provides confidentiality, integrity, and server authentication.

---

# 16. Burp Suite and HTTP

Burp Suite can act as an intercepting proxy:

```text
Browser
   |
   | HTTP Request
   v
Burp Suite
   |
   | Modified Request
   v
Web Server
   |
   | HTTP Response
   v
Burp Suite
   |
   v
Browser
```

Example:

```http
GET /profile?id=100 HTTP/1.1
Host: example.com
Cookie: session=abc123
```

A pentester may test whether changing:

```text
id=100
```

to:

```text
id=101
```

allows unauthorized access to another user's data.

This can indicate **IDOR/BOLA or broken access control**, depending on the application/API context.

---

# 17. HTTP Security Testing Areas

| HTTP Component | Security Testing Area |
|---|---|
| URL parameters | SQL Injection, XSS, IDOR |
| Request body | SQL Injection, XSS, Mass Assignment |
| Cookies | Session attacks |
| Authorization header | Authentication/token attacks |
| Host header | Host header attacks |
| HTTP methods | Method manipulation |
| Headers | Header injection/misconfiguration |
| Redirects | Open redirect |
| Response headers | Security-header weaknesses |
| Response body | Information disclosure/XSS |
| Status codes | Access-control enumeration |
| File upload request | Unrestricted file upload |

---

# 18. Cookies

## What is a Cookie?

A cookie is a small piece of data stored/managed by the browser and associated with a website. It is sent with applicable requests.

### Interview Answer

> A cookie is a small piece of data stored by the browser and sent with applicable requests to maintain state, remember preferences, or identify a session.

---

# 19. Why Are Cookies Needed?

HTTP is fundamentally stateless.

Cookies provide a mechanism for maintaining state between requests.

Typical login flow:

```text
User Login
    |
    v
Server authenticates user
    |
    v
Server creates session
    |
    v
Set-Cookie: sessionId=abc123
    |
    v
Browser stores cookie
    |
    v
Future requests:
Cookie: sessionId=abc123
```

---

# 20. Set-Cookie vs Cookie

## Set-Cookie

Server → Browser

```http
Set-Cookie: sessionId=abc123
```

Used by the server to create/update a cookie.

## Cookie

Browser → Server

```http
Cookie: sessionId=abc123
```

Used by the browser to send stored cookies.

### Memory Trick

> **Set-Cookie = Server sets it**  
> **Cookie = Browser sends it**

---

# 21. What Can a Cookie Store?

Examples:

```text
sessionId=abc123
theme=dark
language=en
```

Security-sensitive values should not simply be trusted because they are stored in cookies.

Bad design:

```http
Cookie: role=admin
```

The client may be able to modify it.

Authorization must always be enforced server-side.

---

# 22. Cookie Attributes

The most important attributes for security interviews:

```text
Secure
HttpOnly
SameSite
Domain
Path
Expires
Max-Age
```

---

# 23. Secure Flag

Example:

```http
Set-Cookie: sessionId=abc123; Secure
```

The browser sends the cookie only over HTTPS connections.

### Interview Answer

> The Secure flag tells the browser to send the cookie only over HTTPS. It helps protect sensitive cookies such as session identifiers from being transmitted over plain HTTP.

### Important

`Secure` does not itself encrypt the cookie.

TLS/HTTPS protects the network communication.

---

# 24. HttpOnly Flag

Example:

```http
Set-Cookie: sessionId=abc123; HttpOnly
```

Prevents client-side JavaScript from accessing the cookie through APIs such as:

```javascript
document.cookie
```

### Why?

If XSS exists, malicious JavaScript may try to access cookies.

Without HttpOnly:

```text
XSS
 ↓
JavaScript
 ↓
document.cookie
 ↓
Potential cookie theft
```

With HttpOnly, the associated cookie is not exposed through `document.cookie`.

### Interview Answer

> The HttpOnly flag prevents client-side JavaScript from accessing the cookie through APIs such as document.cookie. It helps reduce the risk of session-cookie theft through XSS.

### Important

```text
HttpOnly ≠ XSS prevention
```

It protects the cookie from JavaScript access; it does not eliminate the XSS vulnerability.

---

# 25. SameSite

Example:

```http
Set-Cookie: sessionId=abc123; SameSite=Lax
```

Controls whether cookies are sent in cross-site contexts.

Three common values:

```text
Strict
Lax
None
```

## SameSite=Strict

```http
SameSite=Strict
```

Provides the strongest cross-site restriction.

## SameSite=Lax

```http
SameSite=Lax
```

Allows some cross-site navigation scenarios while restricting many cross-site request contexts.

## SameSite=None

```http
SameSite=None; Secure
```

Allows cross-site cookie usage, subject to browser rules.

Modern browsers require `Secure` when `SameSite=None` is used.

### Interview Answer

> SameSite controls whether a cookie is sent with cross-site requests. Strict provides the strongest restriction, Lax allows some cross-site navigation scenarios, and None permits cross-site cookie usage and requires Secure. SameSite helps mitigate CSRF but should not necessarily be the only CSRF defense.

---

# 26. Domain Attribute

Example:

```http
Set-Cookie: sessionId=abc123; Domain=example.com
```

Controls which hosts can receive the cookie according to cookie-domain matching rules.

Security concern:

Overly broad domain scope can increase exposure to other subdomains.

---

# 27. Path Attribute

Example:

```http
Set-Cookie: sessionId=abc123; Path=/account
```

Scopes the cookie to matching URL paths.

Examples:

```text
/account
/account/profile
/account/settings
```

---

# 28. Expires

Example:

```http
Set-Cookie: sessionId=abc123; Expires=Wed, 30 Sep 2026 18:00:00 GMT
```

Specifies an expiration date/time for the cookie.

---

# 29. Max-Age

Example:

```http
Set-Cookie: sessionId=abc123; Max-Age=3600
```

Specifies the cookie lifetime in seconds.

```text
3600 seconds = 1 hour
```

### Expires vs Max-Age

| Attribute | Meaning |
|---|---|
| Expires | Specific expiration date/time |
| Max-Age | Lifetime in seconds |

---

# 30. Session vs Persistent Cookies

## Session Cookie

A cookie without `Expires` or `Max-Age` is generally a session cookie.

Example:

```http
Set-Cookie: sessionId=abc123; Secure; HttpOnly
```

It is associated with the browser session and is normally removed when the browser session ends, subject to browser behavior.

## Persistent Cookie

Has an explicit lifetime.

Example:

```http
Set-Cookie: theme=dark; Max-Age=86400
```

---

# 31. Typical Secure Authentication Cookie

Example:

```http
Set-Cookie: sessionId=abc123; Secure; HttpOnly; SameSite=Lax; Path=/
```

Remember:

```text
Secure
   ↓
HTTPS

HttpOnly
   ↓
No JavaScript access through document.cookie

SameSite
   ↓
Cross-site request restrictions

Domain
   ↓
Which host(s)

Path
   ↓
Which URL paths

Expires
   ↓
Expiration date

Max-Age
   ↓
Lifetime
```

---

# 32. Cookies and Sessions

A cookie and a session are **not the same thing**.

Common architecture:

```text
Browser
   |
   | sessionId=abc123
   v
Server
   |
   v
Session Store
   |
   └── abc123 → User 101
                 Role: user
                 Authenticated: true
```

The cookie contains the session identifier.

The actual session information may be maintained server-side.

---

# 33. Cookie vs Session

| Cookie | Session |
|---|---|
| Stored/managed by browser | Usually maintained server-side |
| Sent with applicable requests | Represents server-side user state |
| Can store different types of data | Commonly stores authentication state |
| Client can potentially modify it | Server controls session state |
| Can be persistent | Often expires after timeout/inactivity |

### Interview Answer

> A cookie is a mechanism for storing and transmitting data between a browser and server, while a session represents server-side state associated with a user. In a common authentication design, the browser stores a session ID in a cookie and sends it with subsequent requests.

---

# 34. Session Hijacking

Suppose:

```http
Cookie: sessionId=abc123
```

An attacker obtains a valid session identifier.

They may attempt:

```http
Cookie: sessionId=abc123
```

in their own requests.

If the server accepts the identifier, the attacker may gain access to the associated authenticated session.

### Important defenses

- HTTPS
- Secure cookies
- HttpOnly where appropriate
- Appropriate SameSite settings
- Strong/random session IDs
- Session expiration
- Session invalidation
- Session regeneration
- Protection against XSS

---

# 35. Cookie Theft Through XSS

If a session cookie is not HttpOnly:

```javascript
document.cookie
```

may expose it to JavaScript.

If an attacker has XSS, this can potentially lead to session-cookie theft.

With:

```http
Set-Cookie: session=abc123; HttpOnly
```

JavaScript cannot access that cookie through `document.cookie`.

---

# 36. Session Fixation

Basic flow:

```text
Attacker obtains/chooses a session identifier
             |
             v
Victim is induced to use that session
             |
             v
Victim authenticates
             |
             v
Attacker attempts to reuse the same session
```

### Key defense

Regenerate the session identifier after authentication.

---

# 37. Cookies and CSRF

Browsers automatically attach applicable cookies to requests.

Example:

```text
Victim logged into bank.example
          |
          v
Victim visits attacker.example
          |
          v
Attacker attempts to cause a request
to bank.example
```

Depending on cookie attributes and browser behavior, authentication cookies may be included.

This is one reason CSRF is possible.

Common defenses:

```text
SameSite cookies
+
CSRF tokens
+
Origin/Referer validation where appropriate
```

---

# 38. Important Security Misconceptions

## Does HttpOnly prevent XSS?

**No.**

```text
HttpOnly → Protects cookie from JavaScript access
```

It does not prevent XSS.

## Does Secure encrypt the cookie?

**No.**

```text
Secure → Cookie sent only over HTTPS
TLS     → Protects network communication
```

## Does POST make data secure?

**No.**

HTTPS/TLS provides protection in transit.

## Is a cookie trusted because it came from the server?

**No.**

The client can potentially modify cookie values.

Security-sensitive decisions must be enforced server-side.

---

# 39. Cookie Security Testing Checklist

When testing an authentication cookie in Burp Suite, check:

```text
☐ Is Secure set?
☐ Is HttpOnly set?
☐ Is SameSite appropriately configured?
☐ Is Domain appropriately scoped?
☐ Is Path appropriately scoped?
☐ Is the expiration reasonable?
☐ Is the session ID sufficiently random?
☐ Is the session ID regenerated after login?
☐ Is the session invalidated after logout?
☐ Is there a reasonable session timeout?
☐ Is sensitive information unnecessarily stored client-side?
☐ Does changing cookie values bypass authorization?
☐ Is session fixation possible?
☐ Is session hijacking possible?
```

---

# 40. Interview Scenario — Cookie Security

### Question

You found:

```http
HTTP/1.1 200 OK

Set-Cookie: session=abc123
```

What would you check?

### Strong Answer

> First, I would determine whether the cookie contains a session identifier or sensitive information. If it is an authentication cookie, I would check whether Secure, HttpOnly, and an appropriate SameSite value are configured. I would also examine the Domain and Path scope, expiration, session ID randomness, session regeneration after authentication, logout invalidation, session timeout, and whether session fixation or session hijacking is possible.

---

# 41. High-Priority Interview Questions

## Q1. What is a cookie?

> A cookie is a small piece of data stored by the browser and sent with applicable requests to maintain state or store information.

## Q2. What is the difference between Cookie and Set-Cookie?

> `Set-Cookie` is sent by the server to set/update a cookie. `Cookie` is sent by the browser to the server with stored cookie values.

## Q3. What is the Secure flag?

> It tells the browser to send the cookie only over HTTPS connections.

## Q4. What is HttpOnly?

> It prevents client-side JavaScript from accessing the cookie through APIs such as `document.cookie`.

## Q5. What is SameSite?

> It controls whether cookies are sent in cross-site contexts and helps reduce CSRF risk.

## Q6. What is session hijacking?

> An attacker obtains a valid session identifier and attempts to use it to impersonate the associated user.

## Q7. What is session fixation?

> An attacker causes a victim to authenticate using a session identifier known to the attacker, then attempts to reuse that session.

## Q8. Cookie vs Session?

> A cookie is browser-side storage/transmission, while a session represents server-side state. A session ID is commonly stored in a cookie.

## Q9. Does HttpOnly prevent XSS?

> No. It only prevents JavaScript from accessing the associated cookie through mechanisms such as `document.cookie`.

## Q10. Does Secure encrypt the cookie?

> No. Secure restricts transmission to HTTPS. TLS provides encryption and integrity for the connection.

---

# 42. One-Minute Revision

```text
HTTP
 ↓
Client sends Request
 ↓
Server sends Response

REQUEST
├── Method
├── Path/URL
├── Headers
└── Body

RESPONSE
├── Status Code
├── Headers
└── Body

COOKIE
├── Browser-side data
├── Used for state/session identification
├── Set-Cookie → Server → Browser
└── Cookie → Browser → Server

SECURITY FLAGS
├── Secure   → HTTPS only
├── HttpOnly → No JS access via document.cookie
└── SameSite → Cross-site cookie restrictions

OTHER ATTRIBUTES
├── Domain
├── Path
├── Expires
└── Max-Age

SECURITY RISKS
├── Session Hijacking
├── Session Fixation
├── Cookie Theft
├── XSS-related cookie exposure
└── CSRF

TEST WITH BURP
├── Cookie flags
├── Session management
├── Authorization
├── Session expiration
├── Logout invalidation
└── Cookie manipulation
```

---

# 43. Final Interview Framework

For almost any web-security concept, use:

```text
1. What is it?
2. How does it work?
3. Why is it a security concern?
4. Give an example.
5. How would you detect/test it?
6. How would you prevent/mitigate it?
```

For cookies, for example:

```text
What?
→ Small browser-stored data.

How?
→ Server sends Set-Cookie; browser sends Cookie.

Security concern?
→ Authentication/session cookies can be stolen or abused.

Example?
→ session=abc123.

Test?
→ Inspect Set-Cookie in Burp and test session behavior.

Mitigation?
→ HTTPS, Secure, HttpOnly, appropriate SameSite,
   strong session management, expiration and invalidation.
```

## Must Memorize

```text
Secure   → HTTPS only
HttpOnly → JavaScript cannot access cookie via document.cookie
SameSite → Controls cross-site cookie sending
Cookie   → Browser → Server
Set-Cookie → Server → Browser
Session ID → Commonly stored in a cookie
```
