# HackThisSite.org – Basic Level 1 (HTML)

## Challenge Information

- **CTF Name:** HackThisSite  
- **Challenge Name:** Level 1 – The Idiot Test  
- **Category:** Web / HTML  
- **Difficulty:** Very Easy (Beginner)  
- **Challenge Description:**  
  Enter the correct password to proceed. The challenge hints that the answer is extremely simple.

---

## TL;DR (Summary)

This challenge requires inspecting the HTML source of the webpage. The password is directly exposed inside an HTML comment. By viewing the page source, the password can be read and submitted to complete the level.

---

## Thought Process & Approach

Since this is labeled *“The Idiot Test”*, the challenge clearly suggests that no complex exploitation is required. For beginner web challenges, the **first instinct** should always be:

> *Check the page source.*

Web developers often leave comments in HTML, and beginner CTFs commonly hide clues there.

---

## Solution Steps

1. Open the challenge page.
2. Right-click anywhere on the page and select **Inspect** or **View Page Source**.
3. Look through the HTML code for comments.

Inside the HTML, the following comment is visible:

```html
<!-- the first few levels are extremely easy: password is 63ac5d16 -->
```
## Final Answer
63ac5d16
