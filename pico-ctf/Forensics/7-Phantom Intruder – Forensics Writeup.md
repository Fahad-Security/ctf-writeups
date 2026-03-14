# Phantom Intruder – Forensics Writeup

## Challenge Overview

**Challenge Name:** Phantom Intruder  
**Category:** Forensics  
**Difficulty:** Easy  

Challenge description:

> A digital ghost has breached my defenses and my sensitive data has been stolen.  
> Your mission is to uncover how this Phantom Intruder infiltrated my system and retrieved the hidden flag.  
> To solve this challenge, analyze the provided **PCAP file**, track the attack method, and uncover the hidden flag.

The challenge provides a **PCAP network capture file** that must be analyzed.

---

# Step 1 – Open the PCAP File

First, download the provided file.

Then open it using **Wireshark**.

```bash
wireshark file.pcap
```

Once the file is opened, we can start analyzing the network traffic.

---

# Step 2 – Inspect the Packets

At first glance, the capture does **not look like a normal TCP session**.

Observations:

- The packets appear as **isolated TCP segments**
- They do **not form a full TCP conversation**
- Each packet contains **small payload data**

Looking at the **TCP Segment Data**, we notice something interesting:

```
VGhpcyBpcyBhIHRlc3Q=
```

This looks like **Base64 encoded data**.

---

# Step 3 – Verify Base64 Encoding

In Wireshark we can test this assumption.

1. Select a packet
2. Go to:

```
Follow → TCP Stream
```

Or inspect the payload bytes.

Then decode the payload as **Base64**.

Wireshark confirms that the data decodes correctly.

---

# Step 4 – Identify Relevant Packets

Looking closely at the packet list, we notice something important.

There are **two types of packets based on payload size**:

| Packet Length | Meaning |
|---|---|
| 8 bytes | Not relevant |
| 12 bytes | Contains Base64 fragments |

Therefore, the useful packets are the ones with **length = 12**.

---

# Step 5 – Sort Packets by Time

The packet numbers are **not in the correct order**.

So we sort the packets by **Time** instead of packet number.

This reveals the correct sequence of the Base64 fragments.

---

# Step 6 – Extract Packet Payloads

For each relevant packet:

1. Open the packet
2. Expand:

```
Transmission Control Protocol → TCP Segment Data
```

3. Copy the Base64 string.

Example fragments:

```
cGljb0NURg==
e2ZvcmVuc2lj
c19hbmFseXNp
cw==
```

---

# Step 7 – Decode the Base64 Data

Now we combine all fragments together.

Example combined string:

```
cGljb0NURntmb3JlbnNpY19hbmFseXNpc30=
```

We decode it using **CyberChef** or the command line.

### Using CyberChef

Recipe:

```
From Base64
```

### Using Linux

```bash
echo "BASE64_STRING" | base64 -d
```

---

# Step 8 – Recover the Flag

After decoding, we obtain the flag:

```
picoCTF{forensic_analysis}
```

---

# Step 9 – Submit the Flag

Submit the flag to the challenge platform.

```
picoCTF{forensic_analysis}
```

Challenge solved successfully.

---

# Key Concepts Learned

## 1. PCAP Network Analysis

A **PCAP file** stores captured network packets and can be analyzed using tools such as:

- Wireshark
- tcpdump
- NetworkMiner

---

## 2. Packet Payload Analysis

Sometimes attackers hide data inside:

- TCP payloads
- DNS queries
- HTTP headers

Extracting these payloads can reveal hidden information.

---

## 3. Base64 Encoding

Base64 encoding is often used to hide or transport binary data.

Example:

```
SGVsbG8=
```

Decodes to:

```
Hello
```

---

## 4. Reassembling Data

Attackers may split encoded data across multiple packets.

To recover it we must:

1. Identify the correct packets
2. Sort them properly
3. Combine the payload
4. Decode the data

---

# Tools Used

- **Wireshark**
- **CyberChef**
- **base64 (Linux command)**

---

# Final Flag

```
picoCTF{forensic_analysis}
```

---

# Conclusion

To solve this challenge we:

1. Opened the PCAP file in Wireshark
2. Identified Base64 encoded payloads
3. Filtered packets by payload size
4. Sorted them by timestamp
5. Reassembled the Base64 fragments
6. Decoded the data to reveal the flag
