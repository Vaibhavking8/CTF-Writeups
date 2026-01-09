# HackThisSite.org – Basic Level 3 (HTML)

## Challenge Information

* **CTF Name:** HackThisSite
* **Challenge Name:** Level 3
* **Category:** Web / HTML
* **Difficulty:** Easy (Beginner)
* **Challenge Description:**
  Network Security Sam remembered to upload the password file, but there were deeper problems than that.

---

## TL;DR (Summary)

The password file name is exposed inside a hidden HTML input field. By directly accessing that file through the browser URL, the password can be retrieved without interacting with the form.

---

## Thought Process & Approach

Since this is a web-based challenge, the first logical step is to inspect the HTML source. The challenge text hints that the password file exists, suggesting a flaw related to **improper file access** rather than brute forcing or guessing.

Hidden form fields often expose useful information and should never be trusted as a security mechanism.

---

## Analysis

Inspecting the HTML source reveals the following form:

```
<form action="/missions/basic/3/index.php" method="post">
    <input type="hidden" name="file" value="password.php">
    <input type="password" name="password"><br><br>
    <input type="submit" value="submit">
</form>
```

### Key Observation

* The hidden input field contains the value `password.php`.
* This strongly suggests that the file exists on the server and may be accessed directly.

---

## Solution Steps

1. Inspect the HTML source of the challenge page.
2. Identify the hidden input field revealing the file name.
3. Manually append the file name to the challenge URL.

Example path:

```
/missions/basic/3/password.php
```

4. Open the file in the browser to reveal the password.

---

## Final Answer

```
cb5a21d7
```

---

