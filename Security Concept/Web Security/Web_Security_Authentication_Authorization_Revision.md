# Web Security — Authentication & Authorization Quick Revision

## 1. Authentication

**Definition:** Authentication is the process of verifying the identity of a user, device, or system.

**Question answered:** **Who are you?**

Example:
```text
Username + Password
        ↓
    Verification
        ↓
   Authenticated
```

### Common Authentication Methods
- Username/password
- MFA
- Session-based authentication
- JWT / token-based authentication
- OAuth/OIDC

### Authentication Factors
- **Something you know:** password, PIN
- **Something you have:** phone, security key
- **Something you are:** fingerprint, face

**MFA:** Uses two or more different authentication factor categories.

### Password Security
- Never store plaintext passwords.
- Use password hashing algorithms such as **Argon2id, bcrypt, or scrypt**.
- Use a unique salt per password.
- Password hashing ≠ encryption.

### Common Authentication Vulnerabilities
- Brute force
- Credential stuffing
- User enumeration
- Authentication bypass
- Session fixation
- Session hijacking
- Weak password handling

### Session Security
After login, a server may create a session:

```text
Login → Session ID → Cookie → Subsequent requests
```

Important cookie attributes:
- **HttpOnly:** Prevents JavaScript from directly reading the cookie.
- **Secure:** Sends cookie only over HTTPS.
- **SameSite:** Controls cross-site cookie sending and helps with CSRF defense.

**Session fixation:** Attacker gets a session ID and the application keeps it after login.  
**Prevention:** Regenerate the session ID after successful authentication.

**Session hijacking:** Attacker obtains a valid session identifier and uses it to impersonate the user.

---

## 2. Authorization

**Definition:** Authorization determines whether an authenticated user is allowed to access a resource or perform an action.

**Question answered:** **What are you allowed to do?**

Example:
```text
Authenticated User
       ↓
Can this user access /admin?
       ↓
      Yes/No
```

### Authentication vs Authorization

| Authentication | Authorization |
|---|---|
| Who are you? | What can you do? |
| Verifies identity | Verifies permissions |
| Login | Access-control decision |
| Happens before authorization | Usually follows authentication |

**Remember:** Authentication = **Identity**; Authorization = **Permission**.

---

## 3. RBAC

**Role-Based Access Control** assigns permissions through roles.

Example:
```text
User → View profile
Manager → Approve orders
Admin → Manage users
```

### Least Privilege
Give users/systems only the permissions required for their tasks.

---

## 4. Authorization Vulnerabilities

### Vertical Privilege Escalation
A lower-privileged user accesses higher-privileged functionality.

```text
Normal User → /admin → Unauthorized access
```

### Horizontal Privilege Escalation
A user accesses another user's resources at the same privilege level.

```text
User A → User B's data
```

### IDOR / BOLA
**IDOR:** Insecure Direct Object Reference.  
**BOLA:** Broken Object Level Authorization.

Example:
```http
GET /api/orders/1001
```

Attacker changes:
```http
GET /api/orders/1002
```

If order `1002` belongs to another user and the server returns it without checking authorization, there is an authorization flaw.

### Broken Access Control
The application fails to correctly enforce authorization rules.

Examples:
- Normal user accesses admin functions.
- User A accesses User B's data.
- User modifies/deletes another user's resource.

**Important:** Authorization must be enforced **server-side**. Hiding buttons in the frontend is not sufficient.

---

## 5. JWT Quick Revision

**JWT = JSON Web Token**

Structure:
```text
HEADER.PAYLOAD.SIGNATURE
```

- **Header:** Algorithm and token type.
- **Payload:** Claims.
- **Signature:** Protects integrity/authenticity when correctly verified.

JWT payloads are **Base64URL encoded, not encrypted by default**.

Common claims:
- `sub` — Subject/user identifier
- `iss` — Issuer
- `aud` — Audience
- `exp` — Expiration
- `iat` — Issued-at
- `nbf` — Not-before
- `jti` — JWT ID

### JWT Security
- Use strong signing keys.
- Explicitly allow expected algorithms.
- Verify signatures.
- Validate `exp`, `iss`, and `aud` where applicable.
- Don't put sensitive secrets/passwords in the payload.
- Protect tokens from theft.
- Consider XSS and CSRF depending on token storage.
- Use short-lived access tokens where appropriate.

**HS256:** Symmetric key; same secret signs and verifies.  
**RS256:** Asymmetric; private key signs, public key verifies.

---

## 6. Interview Framework

For security concepts:

> **What is it? → How does it work? → Security risk → Example → Prevention**

### Common Interview Answers

**Authentication:**  
> Verifies the identity of a user or system — “Who are you?”

**Authorization:**  
> Determines what an authenticated user is allowed to access or do — “What can you do?”

**IDOR/BOLA:**  
> An authorization vulnerability where the application fails to verify whether the authenticated user is authorized to access a specific object.

**Broken Access Control:**  
> A failure to properly enforce authorization rules, allowing users to access resources or perform actions they should not be allowed to.

**Session Hijacking:**  
> An attack where an attacker obtains a valid session identifier and uses it to impersonate the authenticated user.

**JWT:**  
> A compact token format commonly used for authentication and authorization, consisting of a header, payload, and signature.
