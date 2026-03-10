# picoCTF 2019 – Glory of the Garden Writeup

## Overview

**Challenge Name:** Glory of the Garden  
**Category:** Forensics  
**Difficulty:** Easy  
**Event:** picoCTF 2019  

Challenge description:

> This garden contains more than it seems.

The challenge provides a **link to an image file** named:

```
garden.jpg
```

Our task is to analyze the image and extract the hidden flag.

---

# Step 1 – Download the File

First, copy the file link provided in the challenge.

Then open the terminal and create a working directory:

```bash
mkdir garden
cd garden
```

Download the file using `wget`:

```bash
wget <file_link>
```

After downloading, the file will appear as:

```
garden.jpg
```

---

# Step 2 – Verify the File Type

Before analyzing the file, verify its type using the `file` command.

```bash
file garden.jpg
```

Output:

```
garden.jpg: JPEG image data
```

This confirms that the file is a **JPEG image**.

---

# Step 3 – Check Metadata

Images often contain metadata such as:

- camera information
- creation date
- author
- software used

We can extract metadata using **ExifTool**:

```bash
exiftool garden.jpg
```

The command prints a large amount of metadata.

However, in this challenge the metadata **does not reveal the flag directly**.

---

# Step 4 – Extract Readable Strings

Next, we can use the `strings` command.

The `strings` tool extracts **human-readable text** from binary files.

Run:

```bash
strings garden.jpg
```

This command prints all readable strings inside the image file.

Most of the output will look like random data, but near the **end of the output**, we find:

```
picoCTF{more_than_meets_the_eye}
```

---

# Step 5 – Submit the Flag

Copy the flag and submit it in the challenge portal.

```
picoCTF{more_than_meets_the_eye}
```

After submitting, the challenge is solved.

---

# Concepts Learned

This challenge demonstrates several basic forensic techniques.

## 1. File Inspection

Always verify the file type using:

```bash
file
```

---

## 2. Metadata Analysis

Use tools like:

```bash
exiftool
```

to inspect metadata.

---

## 3. Extracting Strings from Binary Files

The `strings` command is extremely useful for finding hidden information inside files.

Example:

```bash
strings filename
```

This can reveal:

- hidden text
- URLs
- encoded data
- flags in CTF challenges

---

# Tools Used

- `wget`
- `file`
- `exiftool`
- `strings`

---

# Conclusion

To solve this challenge we:

1. Downloaded the image file
2. Verified the file type
3. Checked metadata using `exiftool`
4. Extracted readable strings using `strings`
5. Found the flag embedded inside the image data

---

# Final Flag

```
picoCTF{more_than_meets_the_eye}
```
