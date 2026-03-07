# picoCTF – Can You See Writeup

## Overview

The challenge **"Can You See"** is a **Forensics challenge**.  
The goal is to analyze a provided file and discover hidden information inside it.

Often in forensic challenges, files may contain **hidden data using steganography**, encoded strings, or embedded files.

---

## Step 1 – Download the Challenge File

First, download the challenge file provided by picoCTF.

After downloading, we notice that the file is a **ZIP archive**.

Extract the ZIP file:

```
unzip challenge.zip
```

After extraction, we obtain a **JPEG image file**.

Example:

```
image.jpg
```

Since this is a **forensics challenge**, there is a high chance that the image contains **hidden data**.

---

## Step 2 – Check for Hidden Data

A common technique used in CTF challenges is **Steganography**, which hides data inside images.

To analyze the image, we can use **online steganography tools**.

Example tools:

- StegOnline
- AperiSolve
- StegSolve

Upload the extracted **JPEG image** to a steganography analysis tool.

---

## Step 3 – Extract Hidden Strings

While analyzing the image, the tool extracts **embedded data and strings** from the file.

Among the output, we find multiple random-looking strings.

However, one string stands out because it looks like **Base64 encoded data**.

Example:

```
cENURnttZXRhZGF0YV9oaWRkZW59
```

---

## Step 4 – Decode Base64

Base64 is a common encoding format used to store binary data as text.

To decode it, use any Base64 decoder.

Example:

```
https://www.base64decode.org/
```

Paste the encoded string and decode it.

Result:

```
picoCTF{metadata_hidden}
```

---

## Step 5 – Submit the Flag

Now go back to the challenge page and submit the flag:

```
picoCTF{metadata_hidden}
```

After submitting the flag, the challenge is successfully solved.

---

## Tools Used

- Steganography Online Tools
- Base64 Decoder

---

## Lessons Learned

This challenge demonstrates several important forensic concepts:

- Hidden data may exist inside image files.
- Steganography tools can reveal embedded data.
- Base64 encoding is commonly used to hide flags.
- Always check extracted strings when analyzing files.

---

## Conclusion

This was a simple and fun **forensics challenge**.  
The main steps involved:

1. Extracting the ZIP file
2. Analyzing the image for hidden data
3. Finding a Base64 encoded string
4. Decoding the string to reveal the flag

---

**Flag**

```
picoCTF{metadata_hidden}
```
