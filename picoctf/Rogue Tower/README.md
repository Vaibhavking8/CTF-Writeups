# picoCTF – Rogue Tower

## Challenge Information

* **CTF Name:** picoCTF
* **Challenge Name:** Rogue Tower
* **Category:** Forensics / Network Analysis
* **Difficulty:** Medium–Hard
* **Description:**
  A suspicious cell tower is detected. Analyze network traffic to identify the rogue tower, the compromised device, and recover the exfiltrated flag.

---

## TL;DR (Summary)

Identify rogue tower via UDP broadcast, find compromised device via HTTP headers, reconstruct exfiltrated data from POST requests, then decode (Base64 + XOR) to recover the flag.

---

## Thought Process & Approach

This is a network forensics challenge using a packet capture. The goal is to:

1. Identify the rogue tower
2. Find the compromised device
3. Recover and decode exfiltrated data

Wireshark is used for all analysis.

---

## Step 1 – Identify Rogue Tower

<img width="1916" height="939" alt="image" src="https://github.com/user-attachments/assets/ae1f0740-4604-4b00-9bda-fd3651bdfd63" />

Filter for suspicious broadcasts:

```
udp.port == 55000
```

A broadcast packet is observed containing strings like:

```
UNAUTHORIZED_TEST_NETWORK
CELLID=91521
```

This indicates a rogue cellular tower broadcasting a fake network.

---

## Step 2 – Identify Compromised Device

Next, analyze HTTP traffic:

```
http
```

Look for devices interacting with external servers. One device repeatedly communicates with:

```
198.51.100.102
```

This device is associated with the rogue tower’s CELLID, indicating it connected to the fake network.

---

## Step 3 – Locate Exfiltration Traffic

Filter POST requests:

```
http.request.method == "POST"
```

Multiple POST requests to:

```
/upload
```

are observed. The payloads are small chunks of encoded data.

---

<img width="1919" height="473" alt="image" src="https://github.com/user-attachments/assets/483d7e0e-9744-4d7c-952d-113836a5a37f" />

## Step 4 – Reconstruct Data

Combine the payloads from all POST requests in order.

Resulting data (Base64):

```
RVlUW3VsdEJHAFBBBWdRCllcaEAGTwFLagNWAVINB1sHTQ==
```

---

## Step 5 – Decode and Decrypt

### Base64 Decode

Decode the string to get raw bytes.

### XOR Decryption

The key is derived from IMSI digits (7–14):

```
50746829
```

Python script used:

```
import base64

decoded = base64.b64decode('RVlUW3VsdEJHAFBBBWdRCllcaEAGTwFLagNWAVINB1sHTQ==')
key = b'50746829'

result = bytes([decoded[i] ^ key[i % len(key)] for i in range(len(decoded))])
print(result)
```

---

## Result

Decoded output:

```
picoCTF{r0gu3_c3ll_t0w3r_3a5d55b2}
```

---

## Final Answer

```
picoCTF{r0gu3_c3ll_t0w3r_3a5d55b2}
```
