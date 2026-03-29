# picoCTF – m00nwalk2

## Challenge Information

* **CTF Name:** picoCTF
* **Challenge Name:** m00nwalk2
* **Category:** Forensics / Steganography / Audio
* **Difficulty:** Hard
* **Description:**
  A suspicious transmission is provided as audio files. Analyze them to uncover a hidden message.

---

## TL;DR (Summary)

The audio files are SSTV-encoded images. After decoding them, clues reveal a steganography tool and password. The final image contains a hidden flag embedded using JPEG steganography.

---

## Thought Process & Approach

This challenge involves **multi-layered steganography**:

1. Audio → Image (SSTV decoding)
2. Image → Hidden data (JPEG steganography)

---

## Step 1 – Identify Audio Encoding

Spectral analysis of the `.wav` files shows:

* Frequency range: **1200–2300 Hz**
* Matches SSTV (Slow-Scan Television)

Additional confirmation:

* VIS header detected in clue1.wav

Conclusion: Audio contains SSTV-encoded images.

---

## Step 2 – Decode SSTV Audio

Online tool: [https://sstv-decoder.mathieurenaud.fr/](https://sstv-decoder.mathieurenaud.fr/)  

Using SSTV tools (Robot36 / MMSSTV / RXSSTV), decode each file:

### clue1.wav

```
Password: hidden_stegosaurus
```

### clue2.wav

```
Alan Elasen
the FutureBoy
```

→ Points to stego tool: [https://futureboy.us/stegano/](https://futureboy.us/stegano/)

### clue3.wav

```
"The quieter you are the more you can HEAR"
```

→ Hint toward hidden data inside decoded image

### message.wav

```
CTF{beep-boop-im-in-space}
```

→ Appears to be a flag, but is actually a decoy

---

## Step 3 – Recognize the Decoy

The visible flag in the decoded image:

```
CTF{beep-boop-im-in-space}
```

is **not the real flag**. The challenge hints at deeper hidden data.

---

## Step 4 – Identify File Type

Although saved as `.png`, checking file type:

```
file decoded-message.png
```

Shows:

```
JPEG image data
```

This indicates the image is actually a JPEG — suitable for steganography.

---

## Step 5 – Extract Hidden Data

Using the tool hinted earlier:

```
https://futureboy.us/stegano/decinput.html
```

Inputs:

* File: decoded-message.png
* Password: hidden_stegosaurus

---

## Result

The hidden payload is extracted, revealing the **real flag**.

---

## Final Answer

```
picoCTF{the_answer_lies_hidden_in_plain_sight}
```
