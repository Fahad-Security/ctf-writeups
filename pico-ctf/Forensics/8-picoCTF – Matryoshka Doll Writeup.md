# picoCTF – Matryoshka Doll Writeup

## Overview

**Challenge Name:** Matryoshka Doll  
**Category:** Forensics  
**Difficulty:** Easy  

Challenge description:

> Matryoshka dolls are a set of wooden dolls of decreasing size placed one inside another.  
> What's the final one?

We are given a file:

```
dolls.jpeg
```

---

# Step 1 – Inspect the File

First, check the file type:

```bash
file dolls.jpeg
```

Output:

```
PNG image data
```

⚠️ The file is not actually a JPEG — it is a **PNG file**.

---

# Step 2 – Analyze with Binwalk

Use `binwalk` to inspect embedded data:

```bash
binwalk dolls.jpeg
```

Output shows:

```
ZIP archive data
```

This means the image contains a **hidden ZIP file**.

---

# Step 3 – Extract the First Layer

Try extracting the archive:

```bash
unzip dolls.jpeg
```

This creates:

```
base_images/
2000.jpg
```

---

# Step 4 – Recursive Extraction

Inside the new image:

```bash
cd base_images
binwalk 2000.jpg
```

Again, we find:

```
ZIP archive
```

This indicates a **nested structure** (like Matryoshka dolls):

```
Image → ZIP → Image → ZIP → ...
```

---

# Step 5 – Automate the Process

Instead of extracting manually many times, we automate it using a loop.

```bash
for i in {1..100}; do
    unzip -o *.jpg
    cd base_images
done
```

### Explanation

| Command | Purpose |
|--------|--------|
| `for i in {1..100}` | repeat extraction multiple times |
| `unzip -o *.jpg` | extract any image containing ZIP |
| `cd base_images` | move into next layer |

---

# Step 6 – Locate the Flag

After several iterations, we reach the deepest directory.

Check contents:

```bash
ls
```

We find:

```
flag.txt
```

---

# Step 7 – Read the Flag

```bash
cat flag.txt
```

Output:

```
picoCTF{...}
```

---

# Step 8 – Submit the Flag

Copy and submit the flag to complete the challenge.

---

# Key Concepts Learned

## 1. File Type Mismatch

Files may be disguised:

```
.jpeg → actually PNG
```

---

## 2. Steganography / Embedded Data

Files can contain hidden data like:

- ZIP archives
- other images
- binaries

---

## 3. Recursive Extraction

Some challenges use **nested layers**:

```
file → archive → file → archive → ...
```

Automation is important to handle deep nesting.

---

## 4. Binwalk

`binwalk` is useful for detecting embedded files:

```bash
binwalk filename
```

---

# Tools Used

- `file`
- `binwalk`
- `unzip`
- `bash`

---

# Conclusion

Steps to solve:

1. Identify real file type
2. Detect embedded ZIP archive
3. Extract recursively
4. Automate extraction with script
5. Locate final file
6. Retrieve the flag

---

# Final Flag

```
picoCTF{...}
```
