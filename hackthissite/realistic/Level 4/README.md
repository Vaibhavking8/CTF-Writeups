# HackThisSite.org – Realistic Mission 4 (SQL Injection – Data Extraction)

## Challenge Information

* **CTF Name:** HackThisSite
* **Challenge Name:** Realistic 4 – Fischer's Animal Products
* **Category:** Web / SQL Injection
* **Difficulty:** Medium
* **Challenge Description:**
  Fischer's Animal Products maintains a mailing list for customers. The goal is to extract all email addresses from this list so activists can raise awareness.

---

## TL;DR (Summary)

The mailing list form is protected by strict validation, but a separate products page is vulnerable to SQL injection. By exploiting the `category` parameter, the email table can be dumped using a UNION-based injection.

---

## Thought Process & Approach

The most obvious target is the mailing list form, but repeated testing shows that the email input is strictly validated server-side. This suggests that the vulnerability likely exists elsewhere on the site.

Exploring other functionality reveals a products page that takes a numeric parameter directly from the URL, making it a strong candidate for SQL injection.

---

## Initial Exploration

The mailing list form submits data to:

```
addemail.php
```

Behavior observed:

* Valid email → "Email added successfully"
* Invalid input → database error message indicating failed insertion

Despite the database error, injection attempts are blocked by email validation, making this endpoint unusable for exploitation.

---

## Identifying the Vulnerable Endpoint

Browsing the product catalog reveals URLs of the form:

```
products.php?category=1
```

Changing the parameter value leads to altered output, confirming that the value is processed server-side without proper sanitization.

Testing with:

```
?category=1 OR 1=1
```

Returns all products, confirming a **SQL injection vulnerability**.

---

## Data Extraction via UNION Injection

Once SQL injection is confirmed, a UNION-based payload is used to extract data from the email table.

Example payload:

```
?category=1 UNION ALL SELECT NULL, email, NULL, NULL FROM email
```

The injected query causes email addresses to be rendered in the product listing output.

By adjusting column counts and placement, the full mailing list can be retrieved.

---

## Result

All customer email addresses from the mailing list are successfully extracted and sent to the requester, completing the mission.
