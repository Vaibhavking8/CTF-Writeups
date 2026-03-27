# picoCTF – Power Cookie

## Challenge Information

* **CTF Name:** picoCTF
* **Challenge Name:** Power Cookie
* **Category:** Web / Client-Side Manipulation
* **Difficulty:** Easy
* **Description:**
  A login page for an online gradebook provides a guest access option. The goal is to escalate privileges and retrieve the flag.

---

## TL;DR (Summary)

A JavaScript function sets a cookie controlling admin access. By modifying the cookie value, admin privileges can be obtained and the flag revealed.

---

## Thought Process & Approach

The page provides a "Continue as guest" option, which suggests limited access. Inspecting the client-side code can reveal how this access is controlled.

---

## Analysis

Inspecting the HTML shows a script being loaded:

```
<script src="guest.js"></script>
```

Opening `guest.js` reveals:

```
function continueAsGuest()
{
  window.location.href = '/check.php';
  document.cookie = "isAdmin=0";
}
```

Key observation:

* A cookie named `isAdmin` is set to `0`
* This likely controls authorization on the server

---

## Exploitation

Instead of using the default value, modify the cookie to:

```
isAdmin=1
```

Steps:

1. Open browser developer tools
2. Navigate to **Application/Storage → Cookies**
3. Change `isAdmin` from `0` to `1`
4. Click "Continue as guest" or refresh the page

---

## Result

The application now treats the user as an admin and reveals the flag:

```
picoCTF{gr4d3_A_c00k13_0d351e23}
```

---

## Final Answer

```
picoCTF{gr4d3_A_c00k13_0d351e23}
```
