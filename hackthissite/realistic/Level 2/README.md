# HackThisSite.org – Realistic Mission 2 (SQL Injection)

## Challenge Information

* **CTF Name:** HackThisSite
* **Challenge Name:** Realistic 2 – Chicago American Nazi Party
* **Category:** Web / SQL Injection
* **Difficulty:** Medium
* **Challenge Description:**
  A racist group is using a website to organize a rally. The goal is to gain access to the administrator panel and post messages to disrupt their plans.

---

## TL;DR (Summary)

The admin login page is vulnerable to SQL injection. By manipulating the username field, authentication can be bypassed without knowing valid credentials.

---

## Thought Process & Approach

The main page does not expose any obvious functionality, but inspecting the HTML reveals a hidden link to an update page. This leads to an administrator login form.

Testing the login inputs shows that supplying a single quote causes a database error, indicating unsanitized input being used in an SQL query. This strongly suggests an SQL injection vulnerability.

---

## Discovery of the Admin Panel

Although the "update" link is visually hidden due to matching text and background colors, it is present in the HTML:

```
/missions/realistic/2/update.php
```

Visiting this page presents a username and password login form.

---

## Vulnerability Analysis

Testing different inputs reveals:

* Normal values return an authentication failure
* Inputs containing a single quote (`'`) trigger an SQL error

This behavior indicates direct insertion of user input into an SQL query without proper sanitization, likely resembling:

```
SELECT * FROM users
WHERE username = '<INPUT>' AND password = '<INPUT>';
```

---

## Exploitation

By entering the following payload in the **username** field:

```
' OR 1=1--
```

The SQL query becomes logically true and the remainder of the statement is commented out, bypassing authentication entirely.

The password field can be left empty or filled with any value.

Submitting the form with this payload grants administrator access and completes the mission.

---

## Final Result

Administrator access successfully obtained via SQL injection.
