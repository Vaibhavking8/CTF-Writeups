# HackThisSite.org – Basic Level 4 (HTML)

## Challenge Information

* **CTF Name:** HackThisSite
* **Challenge Name:** Level 4
* **Category:** Web / HTML
* **Difficulty:** Easy (Beginner)
* **Challenge Description:**
  Sam hardcoded the password into a script and created a form that emails the password to him in case he forgets it.

---

## TL;DR (Summary)

The email recipient is controlled by a hidden form field. By changing the email address to your own, the server sends the password directly to you.

---

## Thought Process & Approach

The challenge mentions that the password is emailed automatically, which suggests that the application relies on **user-controlled input** to decide where the password is sent. This makes inspecting the HTML form the logical first step.

Hidden fields often expose sensitive logic and can usually be modified before submission.

---

## Analysis

Inspecting the page source reveals the following form:

```
<form action="/missions/basic/4/level4.php" method="post">
    <input type="hidden" name="to" value="sam@hackthissite.org">
    <input type="submit" value="Send password to Sam">
</form>
```

### Key Observation

* The `to` field contains Sam’s email address.
* Since it is client-side and hidden, it can be modified before submission.

---

## Solution Steps

1. Inspect the HTML source of the page.
2. Locate the hidden input field controlling the recipient email.
3. Change the value of the `to` field to your own email address.
4. Submit the form.
5. Check your email inbox to receive the password.

---

## Final Answer

```
eb6d7b2a
```
