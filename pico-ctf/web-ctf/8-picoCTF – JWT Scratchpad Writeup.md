# picoCTF – JWT Scratchpad Writeup

## Overview

**Challenge Name:** JWT Scratchpad  
**Category:** Web Exploitation  
**Difficulty:** Medium  

**Description:**

> Check the admin scratchpad at this website.

---

## Step 1 – Login as a Normal User

The application does not allow logging in as:

```
admin
```

However, we can log in with any other username, such as:

```
john
```

After logging in, we receive a **JWT token** stored in the browser (cookie).

---

## Step 2 – Decode the JWT

Copy the token and decode it using a tool like:

- https://jwt.io

### Decoded Token

**Header:**
```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

**Payload:**
```json
{
  "user": "john"
}
```

---

## Step 3 – Identify the Vulnerability

The token uses:

```
HS256 (HMAC with a secret key)
```

This means:

- The server signs the token using a **shared secret**
- If the secret is weak → it can be brute-forced

---

## Step 4 – Crack the Secret Key

We use **John the Ripper** to brute-force the secret.

### Save the token

```bash
echo "<JWT_TOKEN>" > token.txt
```

### Run John

```bash
john token.txt
```

After some time, we recover the secret:

```
ilovepico
```

---

## Step 5 – Forge an Admin Token

Go back to jwt.io and:

### Modify the payload:

```json
{
  "user": "admin"
}
```

### Use the cracked secret:

```
ilovepico
```

Now the token becomes:

```
Signature Verified ✅
```

---

## Step 6 – Replace the Cookie

1. Open browser Developer Tools
2. Go to **Application → Cookies**
3. Replace the old JWT with the new forged one

---

## Step 7 – Gain Admin Access

Refresh the page.

You are now logged in as:

```
admin
```

The flag will be displayed.

---

## Key Concepts

### 1. JSON Web Tokens (JWT)

Structure:

```
Header.Payload.Signature
```

---

### 2. Weak Secret Key

Using a weak secret allows attackers to:

- Brute-force the key
- Forge valid tokens

---

### 3. Token Forgery

Once the secret is known:

- Modify payload
- Re-sign token
- Escalate privileges

---

### 4. Broken Authentication

Improper JWT implementation leads to:

- Authentication bypass
- Full account takeover

---

## Tools Used

- jwt.io
- John the Ripper
- Browser Developer Tools

---

## Exploitation Flow

```
Login → Extract JWT → Crack Secret → Modify Payload → Re-sign → Replace Cookie → Admin Access
```

---

## Final Flag

```
picoCTF{...}
```

---

## Conclusion

This challenge demonstrates the risk of:

- Using weak JWT secrets
- Relying on symmetric signing (HS256) without strong protection

### Secure Practices

- Use strong, random secrets
- Prefer asymmetric algorithms (RS256)
- Validate tokens properly on the server
