# picoCTF – Some Assembly Required 4

## Challenge Information

* **CTF Name:** picoCTF
* **Challenge Name:** Some Assembly Required 4
* **Category:** Web / Reverse Engineering / WebAssembly
* **Difficulty:** Medium–Hard
* **Description:**
  A web page loads an obfuscated JavaScript file and a WASM binary. The goal is to analyze the logic and retrieve the correct flag.

---

## TL;DR (Summary)

De-obfuscated JavaScript to identify WASM usage, extracted and decompiled the WASM file, analyzed the transformation logic, and reversed it using a script to recover the original flag.

---

## Thought Process & Approach

The challenge name hints at assembly/WebAssembly. The presence of obfuscated JavaScript suggests the real logic is hidden and likely interacts with a WASM binary.

Plan:

* Inspect page source
* De-obfuscate JavaScript
* Extract WASM binary
* Reverse engineer logic
* Recover original input (flag)

---

## Analysis / Discovery

### Step 1 – Inspect Page Source

Observation:

* A JS file is loaded: `qCCYVvL5W.js`
* Code is obfuscated using string indexing

After de-obfuscation:

```
fetch('ZorD23o0wd.wasm')
  .then(r => WebAssembly.instantiate(r.arrayBuffer()))

function onButtonPress() {
    for (let i = 0; i < input.length; i++)
        exports.copy_char(input.charCodeAt(i), i);

    exports.copy_char(0, input.length);
    exports.check_flag() == 1 ? "Correct" : "Incorrect";
}
```

Inference:

* Input is copied into WASM memory
* Validation happens inside WASM

---

### Step 2 – Identify Important Functions

Key exports:

* copy_char
* check_flag
* strcmp

Inference:

* strcmp compares transformed input with stored data
* check_flag performs transformation before comparison

---

### Step 3 – Extract WASM

Download the wasm file from sources.

Convert to readable format:

```
wasm2wat flag.wasm -o flag.wat
```

Observation:

* Encoded flag found in data section at offset 1024

---

### Step 4 – Understand Transformation Logic

Observation from WAT:

* Input stored at offset 1072
* Compared using:

  strcmp(1024, 1072)

Transformation steps applied per byte:

```
T1: byte ^= 20
T2: byte ^= mem[i-1]
T3: byte ^= mem[i-3]
T4: byte ^= (i % 10)
T5: byte ^= 9 (even i), 8 (odd i)
T6: byte ^= [7,6,5][i % 3]
T7: swap adjacent pairs
```

Key Insight:

* T2 and T3 use already transformed values
* Makes reversal dependent on order

---

## Exploitation / Steps

### Step 5 – Reverse the Transformation

Strategy:

* Reverse operations in opposite order
* Process right-to-left
* XOR is self-inverse

Script:

```
stored = [0x18,0x6a,0x7c,0x61,...]

n = len(stored)

# Undo swap
for i in range(0, n, 1):
    if i % 2 == 0 and i+1 < n:
        stored[i], stored[i+1] = stored[i+1], stored[i]

# Reverse transformations
for i in range(n-1, -1, -1):
    v = stored[i]

    v ^= [7,6,5][i % 3]
    v ^= (9 if i % 2 == 0 else 8)
    v ^= (i % 10)

    if i > 2:
        v ^= stored[i-3]
    if i > 0:
        v ^= stored[i-1]

    v ^= 20
    stored[i] = v

print("".join(chr(c) for c in stored))
```

---

## Result

Output:

```
picoCTF{1c4abb877272112e39233c05ade7abbb}
```

---

## Final Answer

```
picoCTF{1c4abb877272112e39233c05ade7abbb}
```
