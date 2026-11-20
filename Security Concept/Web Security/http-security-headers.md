# HTTP Security Headers Reference

This reference covers the headers discussed for CORS, the Same-Origin Policy, CSRF, cookie protection, Content Security Policy (CSP), and HTTP Strict Transport Security (HSTS). Headers can reduce specific risks, but their effectiveness depends on correct server configuration and browser behavior.

| Header | What it does | What it helps prevent or control |
|---|---|---|
| `Origin` | Identifies the origin (scheme, host, and port) that initiated a request. Servers can compare it with an allowed origin. | Helps detect or reject cross-origin state-changing requests, reducing CSRF risk. It is also used in CORS checks. |
| `Access-Control-Allow-Origin` | In a response, specifies which origin may read the response through browser CORS. | Restricts cross-origin JavaScript access to response data; it is not, by itself, a CSRF defense. |
| `Access-Control-Allow-Methods` | Lists the request methods permitted by the CORS policy. | Restricts which methods a browser may use for an approved cross-origin request. |
| `Access-Control-Allow-Headers` | Lists request headers permitted by the CORS policy. | Restricts which non-simple headers an approved cross-origin request may send. |
| `Access-Control-Allow-Credentials` | Indicates whether a CORS request may include credentials such as cookies. | Controls credentialed cross-origin access; it does not independently prevent CSRF. |
| `Access-Control-Expose-Headers` | Lists response headers that cross-origin JavaScript may read. | Limits exposure of response headers to other origins. |
| `Access-Control-Max-Age` | Sets how long a browser may cache the result of a CORS preflight. | Reduces preflight traffic; it is not a security defense on its own. |
| `Access-Control-Request-Method` | In a preflight request, identifies the method the browser wants to use. | Lets the server decide whether that cross-origin method is allowed. |
| `Access-Control-Request-Headers` | In a preflight request, identifies the headers the browser wants to send. | Lets the server decide whether those cross-origin request headers are allowed. |
| `Referer` | Often identifies the page that caused a request. Servers may validate its origin when present. | Can help detect suspicious cross-origin state-changing requests and reduce CSRF risk; it may be omitted or trimmed. |
| `Cross-Origin-Resource-Policy` | Restricts which sites may load a response as a resource (`same-origin`, `same-site`, or `cross-origin`). | Helps prevent unwanted cross-origin embedding or loading of resources. It does not define the Same-Origin Policy itself. |
| `Cookie` | Sends stored cookies from the client to the server when cookie rules allow. | Not a protective header; it carries session or other cookie data. Secure cookie attributes govern when it is sent. |
| `Set-Cookie` | Sets or updates a cookie. Attributes can include `HttpOnly`, `Secure`, and `SameSite`. | With appropriate attributes, reduces JavaScript access to cookies, plaintext transmission, and some cross-site cookie sending. |
| `Sec-Fetch-Site` | Fetch Metadata request header that indicates the relationship between the request initiator and destination (for example, `same-origin`, `same-site`, or `cross-site`). | Lets servers reject unexpected cross-site state-changing requests, helping defend against CSRF. |
| `Content-Security-Policy` | Defines which sources and behaviors the browser should allow for a page. | Can reduce the impact of XSS and data injection by restricting scripts, resources, framing, and other behaviors. |
| `Content-Security-Policy-Report-Only` | Reports CSP violations without enforcing the policy. | Helps identify policy problems before enforcement; it does not block violations. |
| `Strict-Transport-Security` | Tells browsers to use HTTPS for the site for a specified duration; optional directives include `includeSubDomains` and `preload`. | Helps prevent downgrade attacks and HTTP connections that could expose traffic to interception. |
| `X-CSRF-Token` (custom) | Common application-defined request header carrying a CSRF token, checked by the server. | Helps prevent forged state-changing requests when tokens are unpredictable, validated, and unavailable to an attacking origin. |

## Notes

- `HttpOnly`, `Secure`, and `SameSite` are **`Set-Cookie` attributes**, not separate HTTP headers.
- CORS controls whether browser scripts can access cross-origin responses. It is not a substitute for CSRF defenses or server-side authorization.
- The Same-Origin Policy is a browser security model, not a single response header. CORS and headers such as `Cross-Origin-Resource-Policy` add specific cross-origin controls.
- A CSRF token may be sent in a custom header such as `X-CSRF-Token`; that header name is application-defined, not standardized.
- No header alone prevents all cookie theft. Use HTTPS, secure cookie attributes, and XSS prevention together.
