# picoCTF – Scan Surprise Writeup

## Overview

This challenge introduces a simple concept in digital forensics: **extracting hidden data from QR codes**.

The challenge provides an image file containing a QR code. The goal is to scan the QR code and retrieve the hidden flag.

**Category:** Forensics  
**Difficulty:** Easy  
**Goal:** Extract the flag hidden inside a QR code image.

---

## Tools Used

- ZBar Tools
- Custom analysis tools (e.g., image analysis utilities)
- Linux terminal
- Mobile QR scanner (optional)

---

## Initial Analysis

After downloading the provided challenge files, we receive an image that contains a **QR code**.

QR codes often store different types of data such as:

- URLs
- Text
- Contact information
- Hidden messages
- Flags in CTF challenges

The simplest approach is to **scan the QR code**.

---

## Method 1 – Using Image Analysis Tool

First, open the image using an analysis tool.

Example workflow:

1. Open the tool.
2. Navigate to the **photo or image analysis section**.
3. Load the downloaded image.
4. Detect the QR code within the image.
5. Decode the QR code.

After decoding the QR code, the tool reveals the hidden text containing the flag.

---

## Method 2 – Using ZBar Tools

Another method is to use **ZBar**, a popular tool for scanning barcodes and QR codes from images.

### Install ZBar

```bash
sudo apt install zbar-tools
```

### Scan the QR Code

Run the following command:

```bash
zbarimg image.png
```

Example output:

```
QR-Code:picoCTF{...}
```

---

## Result

After scanning the QR code, the flag is revealed.

```
Flag: picoCTF{...}
```

---

## Analysis

This challenge demonstrates a common technique used in **CTF forensics challenges**.

Instead of hiding the flag in plain text, the flag is embedded inside a **QR code image**. Participants must use QR scanning tools to extract the hidden information.

---

## Lessons Learned

- QR codes can contain hidden information used in forensic challenges.
- Tools like **ZBar** make it easy to decode QR codes from images.
- Always analyze images for embedded data in CTF challenges.

---

## Mitigation (Real-World Context)

If sensitive data is encoded in QR codes:

- Avoid embedding confidential information directly in QR codes.
- Use secure links instead of direct data storage.
- Apply access control to sensitive resources.

---

## References

- ZBar Tools Documentation
- QR Code Technical Overview
- picoCTF Forensics Challenges
