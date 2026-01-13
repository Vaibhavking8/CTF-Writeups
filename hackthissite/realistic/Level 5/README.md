# HackThisSite.org – Realistic Mission 5 (Hash Cracking / Information Disclosure)

## Challenge Information

* **CTF Name:** HackThisSite
* **Challenge Name:** Realistic 5 – Damn Telemarketers!
* **Category:** Web / Cryptography / Information Disclosure
* **Difficulty:** Medium–Hard
* **Challenge Description:**
  A telemarketing company has restored their database with an encrypted administrative password. The goal is to recover the admin password and regain access.

---

## TL;DR (Summary)

Sensitive files are exposed via `robots.txt`. A backup admin file leaks a password hash, which can be cracked offline to recover the admin password.

---

## Thought Process & Approach

The challenge description mentions that Google was indexing sensitive links and that extra precautions were taken. This strongly suggests checking `robots.txt` for disallowed paths.

Additionally, the password is described as a "message digest", implying the use of a hash rather than encryption.

---

## Reconnaissance

Accessing `robots.txt` reveals:

```
User-agent: *
Disallow: /lib
Disallow: /secret
```

These directories immediately become high-value targets.

---

## Exploring Restricted Directories

### /lib

Browsing `/lib` reveals a directory listing containing a file with a hash value. This suggests password-related material.

### /secret

Browsing `/secret` reveals:

```
admin.php
admin.bak.php
```

The presence of a backup file is a strong indicator of information leakage.

---

## Hash Disclosure

Accessing `admin.bak.php` displays an error message containing the following hash:

```
d0b2af60971ff57a337db56c2aa82a88
```

Accessing `admin.php` shows that this hash is used to validate the administrator password.

---

## Identifying the Hash Type

Using online hash identification tools suggests that the value is an **MD4 hash**.

Since MD4 is outdated and insecure, offline cracking becomes feasible.

---

## Cracking the Hash

Using **John the Ripper** with the raw MD4 format:

```
john --format=raw-md4 hash.txt
```

After running wordlist and incremental attacks, the hash is successfully cracked.

Recovered password:

```
c125a
```

---

## Result

Using the recovered password grants administrator access, allowing the database to be deleted and completing the mission.

---

## Final Answer

```
c125a
```
