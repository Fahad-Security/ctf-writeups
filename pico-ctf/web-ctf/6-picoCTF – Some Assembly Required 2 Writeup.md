# picoCTF – Some Assembly Required 2 Writeup

## Overview

The challenge **"Some Assembly Required 2"** is a **Web Exploitation challenge** that involves analyzing **WebAssembly (WASM)** code running inside the browser.

Unlike the previous challenge, the flag is **not directly visible**.  
Instead, we must analyze the WebAssembly logic to understand how the flag verification works.

---

## Step 1 – Open the Challenge Website

The challenge provides a website with an input box for submitting a flag.

When entering a random value, the site returns:

```
That is incorrect
```

Since this is a web challenge, the next step is to inspect the page.

---

## Step 2 – Inspect the Page Source

Right-click the page and select:

```
View Page Source
```

We notice that the page includes a **JavaScript file**.

Inside the JavaScript code, we see a reference to another file with a strange name:

```
ad8svhybk
```

This file is being loaded and executed by the webpage.

---

## Step 3 – Identify the File Type

Download the file and analyze it using the `file` command.

```bash
file ad8svhybk
```

Output:

```
WebAssembly binary module
```

This confirms that the file is a **WebAssembly (.wasm) module**.

---

## Step 4 – Enable WebAssembly Debugging

Modern browsers like **Google Chrome** allow debugging WebAssembly code.

Steps:

1. Open Chrome
2. Press:

```
Ctrl + Shift + J
```

to open **Developer Tools**

3. Click the **Settings (gear icon)**
4. Enable:

```
WebAssembly DWARF support
```

This allows Chrome to **disassemble WebAssembly instructions**.

---

## Step 5 – Open the WebAssembly File

In the **Sources tab**, locate the WebAssembly module.

Chrome will automatically **disassemble the WASM code**.

You will see functions such as:

```
copy_char
check_flag
strcmp
```

---

## Step 6 – Analyze the JavaScript

Click the **Pretty Print button** to format the JavaScript code.

Inside the code, we find the function:

```
onButtonPress()
```

This function sends the user input to the WebAssembly module.

Example:

```javascript
exports.copy_char()
```

Set a **breakpoint** on this function.

---

## Step 7 – Debug the Program

Enter any test input such as:

```
asdf
```

The breakpoint will trigger and pause execution.

Now we can analyze how the input is processed.

---

## Step 8 – Understand the XOR Operation

The `copy_char` function performs the following operation:

```
input_byte XOR 8
```

This means each character in the input is XORed with the number `8`.

Example:

```
'a' = 97
97 XOR 8 = 105
```

So the program transforms the input before comparing it.

---

## Step 9 – Find the Comparison String

Later in the program, the function `check_flag` calls a string comparison.

Example:

```
strcmp(input, secret_string)
```

The secret string exists in memory at address:

```
1024
```

This string is the **XOR-encoded version of the flag**.

---

## Step 10 – Decode the String

Copy the encoded string and decode it using Python.

Example script:

```python
encoded = "encoded_string_here"

decoded = "".join(chr(ord(c) ^ 8) for c in encoded)

print(decoded)
```

This reverses the XOR operation.

---

## Step 11 – Recover the Flag

After decoding the string, we obtain the real flag:

```
picoCTF{...}
```

Submit the flag on the challenge page.

---

## Concepts Learned

This challenge teaches several important skills.

### 1. WebAssembly Reverse Engineering

WebAssembly can contain compiled code that runs inside the browser.

Understanding its logic requires debugging and analysis.

---

### 2. XOR Encryption

XOR is a common operation used in simple obfuscation.

Properties of XOR:

```
A XOR B XOR B = A
```

This makes XOR easy to reverse.

---

### 3. Browser Debugging

Browser developer tools can be used to:

- debug JavaScript
- inspect memory
- analyze WebAssembly modules
- set breakpoints

---

## Tools Used

- Chrome Developer Tools
- WebAssembly Debugger
- Python

---

## Conclusion

To solve the challenge we:

1. Inspected the website source
2. Located the WebAssembly module
3. Enabled WebAssembly debugging
4. Set breakpoints in the code
5. Discovered the XOR operation
6. Extracted the encoded string
7. Decoded it using Python
8. Recovered the flag

---

## Final Flag

```
picoCTF{...}
```
