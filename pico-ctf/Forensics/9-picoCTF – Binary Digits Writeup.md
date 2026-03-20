# picoCTF – Binary Digits Writeup

## Overview

**Challenge Name:** Binary Digits  
**Category:** Forensics  
**Difficulty:** Easy  

**Description:**

> This file doesn't look like much, just a bunch of ones and zeros.  
> But maybe it's not just random noise. Can you recover anything meaningful?

---

## Step 1 – Download the File

We are given a file:

```
digits.bin
```

---

## Step 2 – Inspect the File

Open the file using a text editor:

```bash
cat digits.bin
```

Output:

```
0101010101101010101010...
```

The file contains only:

```
0s and 1s → Binary Data
```

---

## Step 3 – Identify the Data Type

At first glance, it looks like raw binary.

Key idea:

> This binary might represent something meaningful (not random).

Possible interpretations:

- Text
- Image
- Encoded file

---

## Step 4 – Decode the Binary

We use an online tool like:

👉 https://gchq.github.io/CyberChef/

### Steps in CyberChef:

1. Upload `digits.bin`
2. Apply operation:

```
From Binary
```

3. Then:

```
Render Image
```

---

## Step 5 – Extract the Flag

After decoding:

- The output becomes an **image**
- The flag is visible inside the image

---

## Step 6 – Submit the Flag

Copy the flag and submit it.

---

## Key Concepts Learned

### 1. Binary Encoding

Binary (0s and 1s) can represent:

- Text
- Images
- Files

---

### 2. Data Transformation

Raw data may require multiple steps:

```
Binary → Bytes → Image
```

---

### 3. Forensics Thinking

Always ask:

> Is this really random, or encoded data?

---

## Tools Used

- CyberChef

---

## Exploitation Flow

```
Download File → Identify Binary → Decode → Render Image → Extract Flag
```

---

## Final Flag

```
picoCTF{...}
```

---

## Conclusion

This challenge demonstrates:

- How simple-looking data can hide meaningful information
- The importance of trying different decoding techniques
- Using tools like CyberChef to quickly analyze data

---

🔥 Always remember: If it looks like noise, it might be encoded.
