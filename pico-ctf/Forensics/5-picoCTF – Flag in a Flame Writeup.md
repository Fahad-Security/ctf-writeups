# picoCTF – Flag in a Flame Writeup

## Overview

**Challenge Name:** Flag in a Flame  
**Category:** Forensics  
**Difficulty:** Easy  

Challenge description:

> The SOC discovered a suspiciously large file after a recent breach.  
> When they opened it, they found an enormous block of encoded text instead of typical logs.  
> Could there be something hidden within?

We are given a file called:

```
logs.txt
```

The challenge hint tells us that the data is **Base64 encoded**, and we need to decode it to reveal an image.

---

# Step 1 – Inspect the File

Open the file:

```bash
cat logs.txt
```

You will notice that the content looks like a **very large Base64 string**.

Example:

```
iVBORw0KGgoAAAANSUhEUgAA...
```

This strongly indicates that the file is **Base64 encoded binary data**.

---

# Step 2 – Decode the Base64 Data

We can decode the Base64 data and save the result into an image file.

Run the following command:

```bash
cat logs.txt | base64 -d > hello.jpeg
```

Explanation:

| Command | Purpose |
|------|------|
| `cat logs.txt` | prints the Base64 data |
| `base64 -d` | decodes Base64 into binary |
| `>` | redirects the output to a file |
| `hello.jpeg` | the resulting decoded image |

---

# Step 3 – View the Image

After decoding the file, open the image:

```bash
xdg-open hello.jpeg
```

The image displays a **hacker-themed picture**.

Inside the image, there is a **string of characters** that looks like encoded data.

Example:

```
7069636f4354467b666f72656e736963735f616e616c797369735f69735f616d617a696e677d
```

---

# Step 4 – Identify the Encoding

The string contains only:

```
0-9
a-f
```

This suggests the string is **Hexadecimal encoded data**.

---

# Step 5 – Decode the Hex String

We can decode the hex string using an online tool or a command-line method.

Example using Python:

```python
bytes.fromhex("7069636f4354467b666f72656e736963735f616e616c797369735f69735f616d617a696e677d").decode()
```

Output:

```
picoCTF{forensics_analysis_is_amazing}
```

---

# Step 6 – Submit the Flag

Submit the decoded flag:

```
picoCTF{forensics_analysis_is_amazing}
```

The challenge is now solved.

---

# Concepts Learned

This challenge demonstrates several important forensic techniques.

## 1. Base64 Decoding

Base64 is commonly used to encode binary data into text format.

Decode with:

```bash
base64 -d
```

---

## 2. Hexadecimal Encoding

Hex encoding represents binary data using:

```
0-9
a-f
```

Each two characters represent one byte.

---

## 3. Multi-Step Encoding

Many CTF challenges hide flags using **multiple encoding layers**, such as:

```
Base64 → Image → Hex → Flag
```

---

# Tools Used

- Linux Terminal
- `base64`
- Python
- Image viewer

---

# Conclusion

To solve this challenge we:

1. Downloaded the `logs.txt` file
2. Observed that the data was Base64 encoded
3. Decoded it into an image file
4. Opened the image
5. Found a hex encoded string
6. Decoded the hex string to reveal the flag

---

# Final Flag

```
picoCTF{forensics_analysis_is_amazing}
```
