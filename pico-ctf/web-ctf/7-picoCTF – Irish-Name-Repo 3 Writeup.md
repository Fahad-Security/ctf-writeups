# picoCTF – Irish-Name-Repo 3 Writeup

## Overview

**Challenge Name:** Irish-Name-Repo 3  
**Category:** Web Exploitation  
**Difficulty:** Easy  

Challenge description:

> There is a secure website running at the provided link.  
> Try to see if you can log in as **admin**.

The goal of this challenge is to bypass the login system using **SQL Injection**, but with a small twist.

---

# Step 1 – Explore the Login Page

Opening the challenge website shows a simple login form.

Fields:

```
Username
Password
```

Attempting a classic SQL injection such as:

```
' OR 1=1--
```

does **not work**.

No error message is returned.

---

# Step 2 – Inspect the Page Source

Right-click on the page and choose **Inspect**.

Inside the login form we notice a hidden field:

```html
<input type="hidden" name="debug" value="0">
```

This suggests the application contains a **debug mode**.

---

# Step 3 – Enable Debug Mode

Change the value from:

```
0
```

to:

```
1
```

Then submit the login form again.

Now the server prints the SQL query used by the application.

Example:

```sql
SELECT * FROM users
WHERE username='admin'
AND password='password'
```

However, something unusual appears.

---

# Step 4 – Identify the Input Filtering

When attempting SQL injection with `OR`, the application **modifies the input**.

Example:

```
OR
```

becomes:

```
BE
```

So the application replaces **OR → BE**.

This prevents traditional SQL injection attempts.

---

# Step 5 – Bypass the Filter

Since the application replaces:

```
OR → BE
```

We can attempt an **inverse injection**.

Instead of typing `OR`, we type `BE`, expecting the filter to transform it back.

Payload:

```
' BE 1=1 BE '
```

After processing, the query becomes:

```sql
' OR 1=1 OR '
```

This successfully bypasses authentication.

---

# Step 6 – Retrieve the Flag

After submitting the payload, the application returns the flag:

```
picoCTF{...}
```

Copy the flag and submit it to complete the challenge.

---

# Key Concepts Learned

## 1. SQL Injection Filters

Some applications try to prevent SQL injection by replacing keywords.

Example filter:

```
OR → BE
```

However, simple replacements are **not secure**.

Attackers can bypass them using creative input.

---

## 2. Debug Mode

Hidden debug parameters may reveal sensitive information such as:

- SQL queries
- application errors
- backend logic

This greatly helps attackers.

---

## 3. Inverse Injection

Instead of using blocked keywords, we provide input that becomes malicious **after server processing**.

Example:

```
Input: BE
Server converts → OR
```

---

# Tools Used

- Browser Developer Tools
- Basic SQL knowledge

---

# Attack Flow

1. Identify login form
2. Attempt SQL injection
3. Inspect page source
4. Enable debug mode
5. Analyze SQL query
6. Discover keyword filtering
7. Use inverse injection
8. Retrieve the flag

---

# Final Flag

```
picoCTF{...}
```
