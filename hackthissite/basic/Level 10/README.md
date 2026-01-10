# HackThisSite.org – Basic Level 10 (Client-Side Authentication)

## Challenge Information

* **CTF Name:** HackThisSite
* **Challenge Name:** Level 10
* **Category:** Web / Client-Side Security
* **Difficulty:** Easy–Medium (Beginner)
* **Challenge Description:**
  Sam used a temporary and "hidden" approach to authenticating users, relying on JavaScript logic without considering whether users could inspect or modify it.

---

## TL;DR (Summary)

Authentication is controlled by a client-side cookie. By modifying the cookie value, access can be granted without knowing the password.

---

## Thought Process & Approach

The main challenge page does not reveal any useful information, but the outer page containing the level description hints at JavaScript-based authentication. This suggests that the validation logic may be happening entirely on the client side.

Since client-side authentication often relies on cookies or local storage, checking browser storage becomes the next logical step.

---

## Analysis

Inspecting the browser’s storage reveals a cookie named:

```
level10_authorized = "no"
```

This strongly indicates that the application checks this cookie to decide whether a user is authorized, instead of validating credentials on the server.

---

## Exploitation

1. Open browser developer tools.

2. Navigate to **Storage → Cookies**.

3. Locate the cookie `level10_authorized`.

4. Change its value from:

   ```
   no
   ```

   to:

   ```
   yes
   ```

5. Submit the form with any password.

The application now grants access without verifying the actual password.

---

## Final Answer

```
Authentication bypassed via cookie manipulation
```
