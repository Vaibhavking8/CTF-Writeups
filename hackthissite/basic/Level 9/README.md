# HackThisSite.org – Basic Level 9 (SSI Bypass)

## Challenge Information

* **CTF Name:** HackThisSite
* **Challenge Name:** Level 9
* **Category:** Web / Server-Side Includes (SSI)
* **Difficulty:** Medium (Beginner)
* **Challenge Description:**
  Sam saved the unencrypted password file in `/missions/basic/9/`. An attempt to restrict SSI exploitation to Level 8 introduced a validation flaw that can be bypassed.

---

## TL;DR (Summary)

The SSI vulnerability from Level 8 is still exploitable. By using directory traversal in an SSI `exec` directive, the Level 9 directory can be listed and the password file located.

---

## Thought Process & Approach

The challenge hints that Level 9 is not vulnerable on its own but can be solved by understanding how Level 8 input validation works. Since SSI was previously exploitable, the key idea is to reuse that vulnerability and traverse into the Level 9 directory.

The script only checks for a specific pattern (`<!--`) and does not properly restrict directory traversal, making it possible to escape the intended directory.

---

## Exploitation via Level 8 SSI

Using the input field from **Level 8**, the following SSI payload is entered:

```
<!--#exec cmd="ls ../../9/" -->
```

This command traverses up the directory tree and lists the contents of the Level 9 directory.

---

## Result

The generated `.shtml` page outputs:

```
index.php
p91e283zc3.php
```

The obscurely named PHP file is identified as the password file.

---

## Solution Steps

1. Return to Level 8, where SSI injection is possible.
2. Inject an SSI `exec` command with directory traversal.
3. List the contents of the Level 9 directory.
4. Identify the obscured PHP file.
5. Open the file directly in the browser to retrieve the password.

---

## Final Answer

```
ce42b8e1
```
