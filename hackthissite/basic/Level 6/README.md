# HackThisSite.org – Basic Level 6 (Cryptography)

## Challenge Information

* **CTF Name:** HackThisSite
* **Challenge Name:** Level 6
* **Category:** Cryptography
* **Difficulty:** Easy (Beginner)
* **Challenge Description:**
  Network Security Sam has encrypted his password using a publicly available encryption system. The encrypted password is given, and the task is to decrypt it.

---

## TL;DR (Summary)

The encryption adds the character index to each character’s ASCII value. Decryption is done by subtracting the index from each encrypted character.

---

## Thought Process & Approach

Inspecting the HTML reveals that the encryption logic is server-side, so there is nothing useful to modify client-side. This indicates the challenge is purely about understanding and reversing the cipher.

Testing the encryption with a known input helps identify the pattern used.

---

## Cipher Analysis

When submitting the input:

```
54321
```

The encrypted output is:

```
55555
```

This implies:

* Each character is shifted forward by its position index
* The shift starts at index 0 and increments by 1 for each next character

Encryption logic:

```
encrypted_char = original_char + index
```

To decrypt:

```
original_char = encrypted_char - index
```

---

## Decryption Process

Encrypted password:

```
532fh9j>
```

Applying reverse shifting character-by-character:

* index 0: '5' → '5'
* index 1: '3' → '2'
* index 2: '2' → '0'
* index 3: 'f' → 'c'
* index 4: 'h' → 'd'
* index 5: '9' → '4'
* index 6: 'j' → 'd'
* index 7: '>' → '7'

---

## Final Answer

```
520cd4d7
```
