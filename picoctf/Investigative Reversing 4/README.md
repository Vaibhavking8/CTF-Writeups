# picoCTF – Investigative Reversing 4

## Challenge Information

* **CTF Name:** picoCTF
* **Challenge Name:** Investigative Reversing 4
* **Category:** Reverse Engineering / Steganography
* **Difficulty:** Hard
* **Description:**
  A binary and several images are provided. Analyze how the binary processes the images and recover the hidden flag.

---

## TL;DR (Summary)

The binary embeds flag bits into BMP images using LSB steganography. By reversing the encoding logic and extracting bits from the pixel data, the flag can be reconstructed.

---

## Thought Process & Approach

This challenge combines reverse engineering and steganography. The plan:

1. Analyze the binary
2. Understand how data is encoded
3. Reverse the encoding logic
4. Extract hidden bits from images

---

## Step 1 – Initial Recon

Online tool: [https://www.boxentriq.com/analysis/binary-analysis](https://www.boxentriq.com/analysis/binary-analysis)  

Using basic analysis tools:

```
strings mystery
```

Reveals useful hints:

```
Item01_cp.bmp
Item01.bmp
flag.txt
encodeDataInFile
```

This suggests the binary encodes flag data into BMP images.

---

## Step 2 – Disassembly

Disassemble the encoding function:

```
objdump -d mystery | grep -A 200 '<encodeDataInFile>'
```

### Key Observations

* Skips 941 bytes into pixel data (after BMP header)
* Outer loop runs 50 iterations
* Every 5th iteration encodes 1 character
* Each character is stored across 8 pixels (LSB of each byte)
* Remaining iterations copy data unchanged
* Global flag index increments across multiple images
* Images processed in reverse order:

  Item05 → Item04 → Item03 → Item02 → Item01

---

## Step 3 – Understanding Encoding

For each character:

* 8 bits are stored
* Each bit is embedded in the least significant bit of a pixel byte

This is classic **LSB steganography**.

---

## Step 4 – Extraction Strategy

To recover the flag:

1. Skip BMP header
2. Jump to encoding offset (+941 bytes)
3. Iterate through pixels
4. Extract LSBs
5. Reconstruct characters

---

## Step 5 – Extraction Script

Python script used:

```
import struct

def extract_flag_chars(filename, skip=941):
    with open(filename, 'rb') as f:
        data = f.read()

    pixel_offset = struct.unpack_from('<I', data, 10)[0]
    pixels = data[pixel_offset + skip:]

    flag_chars = []
    pos = 0

    for outer in range(50):
        if outer % 5 == 0:
            char_val = 0
            for bit in range(8):
                char_val |= (pixels[pos] & 1) << bit
                pos += 1
            flag_chars.append(char_val)
        else:
            pos += 1

    return flag_chars

all_chars = []

for i in range(5, 0, -1):
    chars = extract_flag_chars(f'Item0{i}_cp.bmp')
    all_chars.extend(chars)

print(''.join(chr(c) for c in all_chars))
```

---

## Result

Recovered flag:

```
picoCTF{N1c3_R3ver51ng_5k1115_000000000008c05144c}
```

---

## Final Answer

```
picoCTF{N1c3_R3ver51ng_5k1115_000000000008c05144c}
```
