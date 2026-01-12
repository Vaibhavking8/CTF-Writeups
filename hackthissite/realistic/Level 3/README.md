# HackThisSite.org – Realistic Mission 3 (Path Traversal / File Overwrite)

## Challenge Information

* **CTF Name:** HackThisSite
* **Challenge Name:** Realistic 3 – Peace Poetry: HACKED
* **Category:** Web / Path Traversal
* **Difficulty:** Medium
* **Challenge Description:**
  A peace poetry website has been defaced with propaganda. The original owner no longer has access. The goal is to restore the original website content.

---

## TL;DR (Summary)

The poem submission feature writes files to disk without sanitizing filenames. By using directory traversal, the main `index.html` file can be overwritten with the original content.

---

## Thought Process & Approach

The defaced homepage contains an HTML comment revealing that the original site was copied to `oldindex.html`. This suggests that the attacker only replaced the main page and that restoring it may be possible.

Exploring the site shows a poetry submission form that stores poems online immediately, indicating a server-side file write operation based on user input.

---

## Discovering the Original Content

Inspecting the defaced page reveals the following comment:

```
<!-- The old website is still up. I simply copied the old index.html file to oldindex.html -->
```

Visiting `oldindex.html` confirms that the original Peace Poetry homepage still exists.

---

## Vulnerable Functionality

The poem submission page (`submitpoems.php`) allows users to submit:

* Poem name
* Poem content

With the note:

```
Poems will be stored online immediately
```

This strongly implies that the poem name is used as a filename on the server.

---

## Exploitation

By abusing the poem name field, directory traversal can be used to overwrite existing files.

Input used:

Poem name:

```
../index.html
```

Poem content:

```
(contents of oldindex.html)
```

Submitting the form causes the server to write the poem content directly to `index.html`, restoring the original homepage.

---

## Result

The defaced propaganda page is replaced with the original Peace Poetry website, successfully completing the mission.
