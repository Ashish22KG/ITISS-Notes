# CORS & Same-Origin Policy — Short Revision

## 1. Same-Origin Policy (SOP)

**Definition:**  
A browser security mechanism that restricts a web page/script from freely accessing data from a different origin.

### Origin = Scheme + Host + Port

Example:
```text
https://example.com:443
```

All three must match for two URLs to be same-origin.

| URL | Same Origin? | Reason |
|---|---|---|
| `https://example.com/test` | Yes | Same scheme, host, port |
| `http://example.com` | No | Different scheme |
| `https://api.example.com` | No | Different host |
| `https://example.com:8080` | No | Different port |

**Purpose:** Prevent a malicious website from reading sensitive data belonging to another website.

> Important: SOP does not mean all cross-origin requests are blocked. Browsers can allow some cross-origin writes/embeds while restricting cross-origin reads.

---

## 2. CORS

**Full form:** Cross-Origin Resource Sharing

**Definition:**  
An HTTP-header-based mechanism that allows a server to specify which origins can access its resources from browser-based JavaScript.

### Example

Frontend:
```text
https://app.example.com
```

API:
```text
https://api.example.com
```

Server response:
```http
Access-Control-Allow-Origin: https://app.example.com
```

The browser can then allow JavaScript from the permitted origin to read the response.

---

## 3. Important CORS Headers

| Header | Purpose |
|---|---|
| `Access-Control-Allow-Origin` | Specifies allowed origin(s) |
| `Access-Control-Allow-Methods` | Specifies allowed HTTP methods |
| `Access-Control-Allow-Headers` | Specifies allowed request headers |
| `Access-Control-Allow-Credentials` | Allows credentialed cross-origin requests |
| `Access-Control-Expose-Headers` | Headers browser JavaScript can access |
| `Access-Control-Max-Age` | Caches preflight result |

---

## 4. CORS Preflight

For certain cross-origin requests, the browser first sends an **OPTIONS** request.

Example:
```http
OPTIONS /api/users HTTP/1.1
Origin: https://app.example.com
Access-Control-Request-Method: DELETE
Access-Control-Request-Headers: Authorization
```

Server:
```http
Access-Control-Allow-Origin: https://app.example.com
Access-Control-Allow-Methods: GET, POST, DELETE
Access-Control-Allow-Headers: Authorization
```

If permitted, the browser sends the actual request.

### Flow

```text
Browser
   ↓
OPTIONS (preflight)
   ↓
Server
   ↓
Permission granted
   ↓
Actual request
```

---

## 5. CORS + Credentials

For credentialed cross-origin access:

```http
Access-Control-Allow-Origin: https://app.example.com
Access-Control-Allow-Credentials: true
```

Important:

```http
Access-Control-Allow-Origin: *
Access-Control-Allow-Credentials: true
```

is not valid for credentialed CORS requests.

---

## 6. SOP vs CORS

| SOP | CORS |
|---|---|
| Browser security policy | HTTP-based mechanism |
| Restricts cross-origin access | Allows controlled cross-origin access |
| Provides default security boundary | Provides controlled exceptions |
| Enforced by browser | Configured by server and enforced by browser |

### Memory Trick

**SOP:**  
> "Different origins cannot freely read each other's data."

**CORS:**  
> "The server specifies which different origins are allowed."

---

## 7. CORS vs CSRF

**CORS:**  
Focuses mainly on whether browser JavaScript can **read** a cross-origin response.

**CSRF:**  
Focuses on preventing an attacker from causing a victim's browser to perform an unwanted state-changing action.

> CORS is **not** a replacement for CSRF protection.

---

## 8. CORS Misconfiguration

Potentially dangerous configuration includes:

```http
Access-Control-Allow-Origin: *
```

for sensitive/private resources, or blindly trusting arbitrary attacker-controlled origins.

For credentialed access, use a strict allowlist of trusted origins.

---

# Interview Questions

### What is SOP?
> SOP is a browser security mechanism that restricts scripts from one origin from accessing data from another origin.

### What is CORS?
> CORS is an HTTP-header-based mechanism that allows a server to specify which origins can access its resources from browser JavaScript.

### Why is CORS needed?
> Because browsers enforce SOP. CORS allows legitimate cross-origin applications to communicate with APIs in a controlled way.

### What is a preflight request?
> A browser-sent OPTIONS request that checks whether a cross-origin request's method and headers are allowed before sending the actual request.

### What is `Access-Control-Allow-Origin`?
> A response header that specifies which origin is allowed to access the response.

### Is CORS client-side or server-side?
> The server configures CORS using response headers, while the browser enforces those permissions.

---

## ⭐ Interview Formula

```text
SOP → Restricts cross-origin access
       ↓
CORS → Controlled permission for cross-origin access
       ↓
Browser → Enforces the server's CORS policy
```
