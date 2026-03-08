# picoCTF – Secret of the Polyglot Writeup

## Overview

The challenge **"Secret of the Polyglot"** is a **Forensics challenge** in picoCTF.

The challenge description:

> The Network Operations Center (NOC) of your local institution picked up a suspicious file.  
> They are getting conflicting information about the file type.  
> They brought you in as an external expert to examine the file.  
> Can you extract all the information from this strange file?

The goal of the challenge is to analyze a suspicious file and extract the hidden flag.

---

## Step 1 – Download the File

First, download the provided file:

```
flag2of2-final.pdf
```

When opening the file, it appears to be a **PDF document**.

If we scroll down inside the PDF, we can see **part of the flag**.

Example:

```
...PNG_and_PDF}
```

Copy this part of the flag and save it for later.

---

## Step 2 – Analyze the File

The challenge description mentions **conflicting file type information**, which suggests that the file might be a **polyglot file**.

A **polyglot file** is a file that is valid in more than one format at the same time.

In this case, the file is both:

- PNG Image
- PDF Document

---

## Method 1 – Using a Hex Editor (Windows)

Open the file using a hex editor such as:

```
HxD
```

When examining the file contents, we notice that it contains:

- PNG file data
- PDF file data

PNG files end with a special marker called:

```
IEND
```

Search for it in the hex editor.

Steps:

1. Open the file in HxD
2. Press **Ctrl + F**
3. Search for:

```
IEND
```

Right after the `IEND` marker, we notice that the **PDF file begins**.

This confirms that the file contains **two formats inside the same file**.

---

## Extract the PNG File

To extract the PNG image:

1. Select the PNG portion in the hex editor
2. Copy the hex values
3. Create a new file
4. Paste the hex values
5. Save the file as:

```
flag.png
```

Open the image.

Inside the image, we find the **first half of the flag**.

Example:

```
picoCTF{f1u3nt_
```

---

## Easier Method (Windows)

There is an easier trick.

Since the PNG data already exists inside the file, we can simply **rename the file extension**.

Rename:

```
flag2of2-final.pdf
```

to

```
flag.png
```

Then open the image file.

You will see the **first half of the flag**.

---

## Method 2 – Using Linux

We can also solve the challenge using Linux.

First, check the file type:

```bash
file flag2of2-final.pdf
```

Output:

```
PNG image data
```

Even though the file extension is `.pdf`, Linux detects it as a **PNG image**.

This confirms that the PNG data appears first in the file.

---

### Copy the File as PNG

Create a PNG copy of the file:

```bash
cp flag2of2-final.pdf flag.png
```

Now open it using an image viewer:

```bash
xdg-open flag.png
```

The image reveals the **first part of the flag**.

---

## Combine the Flag

Now combine the two parts:

**Part 1 (from the PNG image)**

```
picoCTF{f1u3nt_
```

**Part 2 (from the PDF)**

```
PNG_and_PDF}
```

Final flag:

```
picoCTF{f1u3nt_PNG_and_PDF}
```

---

## Concepts Learned

This challenge demonstrates several forensic concepts:

### 1. Polyglot Files

A polyglot file is a file that is valid in multiple formats simultaneously.

Example:

- PNG + PDF
- ZIP + Image
- HTML + JavaScript

---

### 2. Hex Analysis

Using hex editors helps detect:

- hidden file headers
- embedded files
- file signatures

---

### 3. File Signatures

Each file type has a unique signature.

Example:

| File Type | Signature |
|-----------|-----------|
| PNG | `89 50 4E 47` |
| PDF | `%PDF` |

---

### 4. File Identification Tools

Useful forensic tools include:

```
file
strings
binwalk
HxD
exiftool
```

---

## Conclusion

This challenge involved analyzing a **polyglot file containing both PNG and PDF data**.

Steps used to solve it:

1. Download the suspicious file
2. Open the PDF and extract the second part of the flag
3. Analyze the file using a hex editor
4. Discover embedded PNG data
5. Extract the PNG image
6. Combine both parts of the flag

---

## Final Flag

```
picoCTF{f1u3nt_PNG_and_PDF}
```
