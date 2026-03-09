# picoCTF – Information Writeup

## Overview

The challenge **"Information"** is a **Forensics challenge** in picoCTF.

Description:

> Files can always be changed in a secret way. Can you find the flag?

The challenge provides a single file:

```
cat.jpeg
```

Our task is to analyze the image and discover the hidden flag.

---

## Step 1 – Download the File

Download the provided image file:

```
cat.jpeg
```

Open the image to inspect it.

The image appears slightly **distorted**, which might initially suggest that the image contains hidden data using **steganography** techniques such as **LSB (Least Significant Bit)** manipulation.

However, before attempting complex steganography analysis, it is always good to check **metadata** first.

---

## Step 2 – Analyze the Image Metadata

Images often contain metadata such as:

- Camera information
- Author
- Copyright
- Software used
- Hidden notes

To extract metadata, we use the **ExifTool** utility.

### Install ExifTool (if not installed)

```bash
sudo apt install exiftool
```

---

## Step 3 – Extract Metadata

Run the following command:

```bash
exiftool cat.jpeg
```

The command prints all metadata associated with the image.

Example output:

```
License : cGljb0NURnttZXRhZGF0YV9pc19tb2RpZmllZH0
```

The **License field** contains a suspicious string.

---

## Step 4 – Identify the Encoding

The string looks like **Base64 encoded data**.

Base64 is commonly recognized by:

- Uppercase letters
- Lowercase letters
- Numbers
- Sometimes `+`, `/`, or `=`

Even though this string does not contain `=` padding, it still matches the Base64 character set.

---

## Step 5 – Decode Base64

To decode the string, use the following command:

```bash
echo "cGljb0NURnttZXRhZGF0YV9pc19tb2RpZmllZH0" | base64 -d
```

Output:

```
picoCTF{metadata_is_modified}
```

---

## Step 6 – Submit the Flag

Submit the decoded flag:

```
picoCTF{metadata_is_modified}
```

The challenge is now solved.

---

## Concepts Learned

This challenge demonstrates several important forensic concepts.

### 1. Metadata Analysis

Files can contain hidden information in metadata fields.

Always inspect metadata using tools such as:

```
exiftool
```

---

### 2. Base64 Encoding

Base64 is a common encoding technique used to represent binary data as text.

Characteristics:

- Uses 64 characters
- Often ends with `=` padding
- Easily decoded with tools like `base64`

---

### 3. Multi-Category Challenges

Even though this challenge is classified as **Forensics**, it also uses **Cryptography concepts** (Base64 encoding).

CTF challenges often combine multiple categories.

---

## Tools Used

- `exiftool`
- `base64`
- Linux Terminal

---

## Conclusion

To solve the challenge we:

1. Downloaded the image file
2. Inspected the image
3. Extracted metadata using `exiftool`
4. Found a Base64 encoded string
5. Decoded the string
6. Recovered the flag

---

## Final Flag

```
picoCTF{metadata_is_modified}
```
