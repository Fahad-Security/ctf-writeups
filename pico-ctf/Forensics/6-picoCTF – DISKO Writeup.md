# picoCTF – DISKO Writeup

## Overview

**Challenge Name:** DISKO  
**Category:** Forensics  
**Difficulty:** Easy  

Challenge description:

> Can you find the flag in this image?  
> Many strings can help if there's only one way to do this.

The challenge provides a downloadable file that appears to be an image, but it is actually **compressed**.

---

# Step 1 – Download the File

Copy the download link provided in the challenge.

Then use `wget` to download the file:

```bash
wget <file_link>
```

After downloading, check the file:

```bash
ls
```

Example output:

```
disco.gz
```

---

# Step 2 – Identify the File Type

Use the `file` command to identify the file type:

```bash
file disco.gz
```

Output:

```
gzip compressed data
```

This confirms the file is **Gzip compressed**.

---

# Step 3 – Decompress the File

Extract the file using `gunzip`:

```bash
gunzip disco.gz
```

Now check the directory again:

```bash
ls
```

Output:

```
disco
```

---

# Step 4 – Inspect the File

Check the file type again:

```bash
file disco
```

The file may not immediately reveal its contents, so we proceed with further analysis.

---

# Step 5 – Extract Strings

Use the `strings` command to extract human-readable text from the binary file.

```bash
strings disco
```

Since we know picoCTF flags usually contain the word **pico**, we can filter the output using `grep`.

```bash
strings disco | grep pico
```

---

# Step 6 – Find the Flag

Among the extracted strings, we find the flag:

```
picoCTF{strings_is_the_way}
```

---

# Step 7 – Submit the Flag

Copy the flag and submit it in the challenge portal.

```
picoCTF{strings_is_the_way}
```

The challenge is now solved.

---

# Concepts Learned

This challenge introduces several important forensic techniques.

## 1. File Type Identification

The `file` command helps determine the real type of a file.

Example:

```bash
file filename
```

---

## 2. Gzip Compression

`.gz` files are compressed using the **gzip algorithm**.

To extract them:

```bash
gunzip filename.gz
```

---

## 3. Extracting Hidden Text

The `strings` command extracts readable text from binary files.

Example:

```bash
strings filename
```

This can reveal:

- hidden messages
- URLs
- flags
- encoded data

---

## 4. Filtering Output

Using `grep` helps find specific keywords quickly.

Example:

```bash
strings file | grep pico
```

---

# Tools Used

- `wget`
- `file`
- `gunzip`
- `strings`
- `grep`

---

# Conclusion

Steps used to solve this challenge:

1. Download the compressed file
2. Identify the file type
3. Decompress the `.gz` file
4. Extract readable strings
5. Filter the results using `grep`
6. Recover the flag

---

# Final Flag

```
picoCTF{strings_is_the_way}
```
