# picoCTF – Some Assembly Required 1 Writeup

## Overview

The challenge **"Some Assembly Required 1"** is a **Web Exploitation challenge** in picoCTF.

The challenge only provides a **website link**, which suggests that the solution will involve inspecting how the web application works.

The name of the challenge hints at **WebAssembly (WASM)**.

---

## Step 1 – Open the Challenge Website

First, open the provided challenge link in the browser.

You will see a simple webpage with an input box asking for a **flag**.

Since this is a **web exploitation challenge**, the first step is to inspect the page.

---

## Step 2 – Inspect the Page

Open the browser developer tools:

```
Right Click → Inspect
```

Then go to the **Debugger / Sources** tab.

While exploring the JavaScript code, you may notice that the code has been **minified (compressed)**.

To make it easier to read, click the **Beautify / Format button**.

---

## Step 3 – Look for WebAssembly

After formatting the JavaScript code, scroll through it.

You will find a section that loads **WebAssembly**.

Example:

```javascript
fetch("something.wasm")
```

The code fetches a `.wasm` file and executes it.

This indicates that the **flag verification logic is inside the WebAssembly file**.

---

## Step 4 – Locate the WASM File

In the **Sources tab**, look at the file structure.

You will see a directory called:

```
wasm/
```

Inside it, there will be a `.wasm` file.

Example:

```
main.wasm
```

If it does not appear immediately, refresh the page.

Sometimes the file only loads after refreshing or interacting with the page.

---

## Step 5 – Inspect the WASM File

Click the `.wasm` file in the Sources panel.

The browser will show the **WebAssembly instructions**.

Scroll down through the file.

Surprisingly, the **flag appears directly in plain text** near the bottom.

---

## Step 6 – Copy the Flag

Once you find the flag in the WASM file, copy it.

Example format:

```
picoCTF{...}
```

Paste it into the challenge submission box.

---

## Concepts Learned

This challenge introduces several important concepts.

### 1. WebAssembly (WASM)

WebAssembly is a **binary instruction format for the web**.

It allows high-performance code (C, C++, Rust) to run in the browser.

Files usually have the extension:

```
.wasm
```

---

### 2. Client-Side Security

If sensitive logic or secrets are stored on the **client side**, users can easily inspect them.

Developer tools allow access to:

- JavaScript
- WebAssembly
- Network requests
- Source files

---

### 3. Browser Developer Tools

Important tabs when analyzing web challenges:

| Tab | Purpose |
|----|----|
| Elements | Inspect HTML |
| Console | Run JavaScript |
| Network | Monitor requests |
| Sources | View loaded files |
| Debugger | Analyze scripts |

---

## Conclusion

This challenge demonstrates that **client-side code should never contain secrets**.

Steps used to solve the challenge:

1. Open the challenge website
2. Inspect the page using developer tools
3. Find the WebAssembly file
4. Open the `.wasm` file in the Sources tab
5. Scroll through the code
6. Locate the flag in plain text

---

## Final Flag

```
picoCTF{...}
```
