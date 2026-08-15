# Authentication & JWT — Learning Notes

A practical overview of how applications verify users, protect passwords, use JWTs for authentication, and manage package versions with npm.

# 📚 Table of Contents

1. Authentication Basics
2. Password Hashing
3. JWT Introduction
4. JWT Components
5. JWT Integrity & Security
6. Authentication Using JWT
7. npm Semantic Versioning
8. Version Range Comparison
9. Quick Revision

---

# 1. What is Authentication?

Authentication means confirming the identity of a user.

For example, when a user logs into an application, they usually provide:

- Email or username
- Password

The server verifies these details against the information stored in the database.

## Basic Process
```
User
  │
  │ Login Credentials
  ▼
Server
  │
  ▼
Database
  │
  ▼
Credentials Valid?
  │
  ├── No ──► Login Rejected
  │
  └── Yes
        │
        ▼
    Create Token
        │
        ▼
    Send Response
```
Authentication answers:

«"Who is this user?"»

---

# 2. Why Should Passwords Be Hashed?

A database should never store users' passwords as plain text.

Instead, passwords are processed using a password-hashing algorithm such as bcrypt.

For example:
```
Original Password
       │
       ▼
     bcrypt
       │
       ▼
Password Hash
```
A stored value may look something like:

$2b$10$................................

The database stores the hash rather than the original password.

## Important Characteristics

Password hashing is designed to be:

- One-way
- Difficult to reverse
- Resistant to guessing attacks
- Used with a salt by algorithms such as bcrypt

## During Login

The server does not simply decrypt the stored password.

Instead:
```
Entered Password
       │
       ▼
Password Verification
       │
       ▼
Compare Against Stored Hash
       │
       ▼
Match?
  │       │
 Yes      No
  │       │
  ▼       ▼
Login    Reject
```
## Why Does This Matter?

If a database is exposed, plain-text passwords would immediately reveal users' credentials.

With properly hashed passwords, the attacker gets password hashes instead of the original passwords.

---

# 3. What is JWT?

JWT stands for JSON Web Token.

A JWT is a compact token commonly used to carry claims between a client and server.

After successful login, a server may issue a JWT to the client.

The client can then include that token when accessing protected resources.

Instead of repeatedly sending the password, the client uses the token to demonstrate that it has already authenticated.

---

# 4. JWT Components

A JWT has three sections:

Header.Payload.Signature

They are separated by periods (".").

For example:

xxxxx.yyyyy.zzzzz

## A. Header

The header contains information about the token, such as the signing algorithm and token type.

Example:
```
{
  "alg": "HS256",
  "typ": "JWT"
}
```
---

## B. Payload

The payload contains claims about the user or token.

Example:

{
  "userId": 15,
  "email": "user@example.com",
  "role": "user"
}

Possible claims include:

- User ID
- Role
- Username
- Email
- Expiration time

⚠️ Important

A normal JWT payload is not encrypted.

It is encoded so it can be represented inside the token.

Therefore, do not put confidential information inside it.

Never Put Sensitive Data Such As:

❌ Passwords
❌ OTPs
❌ Private keys
❌ API secrets
❌ Banking information

---

## C. Signature

The signature helps the server determine whether the token has been modified.

Conceptually, the signature is created from:
```
Header
   +
Payload
   +
Secret Key
   ↓
Signature
```
For example, a server might keep its signing secret in an environment variable:

JWT_SECRET=your_server_secret

The secret should remain on the server and should not be exposed to clients.

---

# 5. How Does JWT Protect Against Tampering?

Suppose the original token contains:
```
{
  "userId": 15
}

Someone modifies the payload to:

{
  "userId": 1
}
```
The original signature was generated using the old payload.

Therefore, the modified token will no longer have a valid signature.

The server can verify the token using something such as:

jwt.verify(token, JWT_SECRET);

If verification fails, the server should reject the request.

## Remember

A JWT is:

- Readable → its encoded contents can generally be decoded
- Signed → changes can be detected
- Not automatically encrypted → signing does not hide the payload

---

# 6. JWT Authentication Process

A typical JWT-based login process looks like this:
```
             Login
               │
               ▼
       Email + Password
               │
               ▼
       Server Validation
               │
               ▼
       Check Password
          │         │
        Wrong      Correct
          │         │
          ▼         ▼
       Reject    Create JWT
                    │
                    ▼
              Send Token
                    │
                    ▼
             Client Uses JWT
                    │
                    ▼
          Protected Request
                    │
                    ▼
        Authorization Header
          Bearer <token>
                    │
                    ▼
            Server Verifies
                    │
              ┌─────┴─────┐
            Invalid      Valid
              │             │
              ▼             ▼
          Reject        Allow Access
```
A protected request might contain:

Authorization: Bearer <token>

The server verifies the token before allowing access to protected resources.

---

# 7. npm Versioning: "~" vs "^"

npm packages commonly follow Semantic Versioning, written as:

MAJOR.MINOR.PATCH

Example:

4.17.21

Meaning:

4  → Major
17 → Minor
21 → Patch

## Major

Usually represents potentially breaking changes.

## Minor

Usually adds functionality while maintaining compatibility.

## Patch

Normally contains bug fixes and small corrections.

---

## Exact Version

"express": "4.17.21"

This specifies that exact version.

---

## Tilde "~"

"express": "~4.17.21"

Generally permits compatible patch-level updates within the same minor version.

For example:

4.17.22   ✅
4.17.25   ✅
4.18.0    ❌
5.0.0     ❌

---

## Caret "^"

"express": "^4.17.21"

For a normal non-zero major version, this generally permits compatible minor and patch updates without moving to the next major version.

## For example:

4.17.22   ✅
4.18.0    ✅
4.20.3    ✅
5.0.0     ❌

«Note: npm's exact version-range behavior has additional rules for "0.x" versions, so the simple "^ = minor + patch" explanation mainly applies to versions with a major version greater than zero.»

---

# 8. Version Range Comparison
```
Version Format| Patch Updates| Minor Updates| Major Updates
"4.17.21"     | ❌           | ❌          | ❌
"~4.17.21"    | ✅           | ❌          | ❌
"^4.17.21"    | ✅           | ✅          | ❌
```
Easy Way to Remember

Exact → Stay on this version

~     → Small patch-level changes

^     → Minor + patch changes

---


# 9. Quick Revision

## Authentication

- Confirms a user's identity.
- Usually starts with login credentials.
- Successful authentication can result in a token being issued.

## Password Security

- Never store plain-text passwords.
- Use a password-hashing algorithm such as bcrypt.
- Verify passwords against stored hashes.

## JWT

- JWT means JSON Web Token.
- Commonly used for token-based authentication.
- Contains Header, Payload, and Signature.
- The payload should not contain secrets.
- The signature helps detect modification.

## npm

Exact → Exact version
~     → Patch-level updates
^     → Minor + patch updates for typical non-zero major versions

⭐ Main Idea

Authentication identifies the user, password hashing protects stored passwords, JWT carries authentication-related claims between client and server, and npm version ranges control which package updates are allowed.
