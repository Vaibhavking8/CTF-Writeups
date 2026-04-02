# picoCTF – Surfing the Waves

## Challenge Information

* **CTF Name:** picoCTF
* **Challenge Name:** Surfing the Waves
* **Category:** Forensics / Steganography
* **Difficulty:** Medium
* **Description:**
  A WAV audio file is provided. The goal is to analyze it and extract hidden data.

---

## TL;DR (Summary)

The WAV file encodes hexadecimal nibbles in audio samples using multiples of 500. By decoding these samples into bytes, the embedded Python script is recovered, revealing the flag.

---

## Thought Process & Approach

The file is a WAV, but the challenge category suggests steganography. So instead of treating it as normal audio, the goal is to inspect its structure and identify patterns in the raw data.

---

## Analysis / Discovery

### Step 1 – Inspect File Type

```
file main.wav
```

Observation:

```
RIFF (little-endian) data, WAVE audio, Microsoft PCM, 16 bit, mono 2736 Hz
```

Inference:

* Sample rate (2736 Hz) is highly unusual
* File size is very small

Conclusion:

* Likely not real audio → possible data encoding

---

### Step 2 – Extract Raw Samples

```
import wave, struct

f = wave.open("main.wav", "rb")
frames = f.readframes(f.getnframes())
f.close()

samples = struct.unpack(f'<{len(frames)//2}h', frames)
```

Observation:

* All values are positive
* Range: ~1000 to ~8500
* Values cluster around multiples of 500

Inference:

* Not normal audio (no oscillation around 0)
* Suggests structured encoding

---

### Step 3 – Identify Encoding Pattern

Check quotient and remainder:

```
for s in samples[:10]:
    print(s // 500, s % 500)
```

Observation:

* Quotients range from 2 to 17
* Exactly 16 unique values

Inference:

* 16 values → matches hexadecimal (0–15)

Mapping:

```
hex_nibble = (sample // 500) - 2
```

Encoding formula:

```
sample = 1000 + nibble × 500 + noise
```

---

### Step 4 – Decode Nibbles to Bytes

```
nibbles = [s // 500 - 2 for s in samples]

decoded_bytes = []
for i in range(0, len(nibbles) - 1, 2):
    byte = (nibbles[i] << 4) | nibbles[i+1]
    decoded_bytes.append(byte)

text = ''.join(chr(c) for c in decoded_bytes)
```

Observation:

* Output starts with Python script

---

## Exploitation / Steps

### Step 5 – Extract Embedded Script

```
print(text)
```

Observation:

* Full Python script is revealed

Key insight:

* The WAV file encodes its own generator script

---

<img width="1440" height="656" alt="image" src="https://github.com/user-attachments/assets/931592a2-10f9-4ba1-8d4c-e8f4e1ba56b8" />


## Result

Output:

```
picoCTF{mU21C_1s_1337_5db6b85e}
```

---

## Final Answer

```
picoCTF{mU21C_1s_1337_5db6b85e}
```
