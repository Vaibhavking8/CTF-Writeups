# picoCTF – JavaScript Kiddie

## Challenge Information

* **CTF Name:** picoCTF
* **Challenge Name:** JavaScript Kiddie
* **Category:** Web / JavaScript Analysis
* **Difficulty:** Medium
* **Description:**
  A website attempts to reconstruct an image using JavaScript. The image appears broken, and the goal is to recover the hidden flag.

---

## TL;DR (Summary)

The website reconstructs a scrambled PNG using a key-based shifting algorithm. By reversing the logic and finding the correct key, the image is restored and reveals a QR code containing the flag.

---

## Thought Process & Approach

The page loads a JavaScript function that builds an image from raw byte data. Since the image is broken, the goal is to understand how the transformation works and reverse it.

---

## Step 1 – Analyze JavaScript Logic

The function `assemble_png()` reconstructs the image using:

* A 16-character numeric key
* Column-wise shifting of byte data

Key observations:

* Data is split into blocks of 16 bytes
* Each column is shifted based on key digits
* Final output is converted to Base64 PNG

---

## Step 2 – Understand the Transformation

For each column:

* The key digit determines how much to shift rows
* The operation is essentially a cyclic permutation

This means the correct key will restore the original PNG structure.

---

## Step 3 – Identify PNG Structure

PNG files start with a known signature:

```
137 80 78 71 13 10 26 10
```

This can be used to determine correct shifts for the first 8 columns.

---

## Step 4 – Recover Remaining Key

The next bytes correspond to the IHDR chunk:

```
0 0 0 13 73 72 68 82
```

Using these known values, possible key candidates for remaining columns can be generated.

---

## Step 5 – Brute Force Valid Keys

Generate all valid key combinations and reconstruct images.

Use browser decoding to check if output is a valid PNG.

[https://claude.ai/public/artifacts/b6333abb-026c-44e8-86dd-33a9c6255c40](https://claude.ai/public/artifacts/b6333abb-026c-44e8-86dd-33a9c6255c40)

---

## Step 6 – Extract Flag

The correctly reconstructed image is a QR code.

Decoding the QR code reveals the flag.

---

## Result

Recovered flag:

```
picoCTF{234446682bd071682ddd41d241ad24a0}
```

---

## Final Answer

```
picoCTF{234446682bd071682ddd41d241ad24a0}
```
