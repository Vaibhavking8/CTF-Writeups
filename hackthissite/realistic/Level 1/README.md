# HackThisSite.org – Realistic Mission 1 (Parameter Tampering)

## Challenge Information

* **CTF Name:** HackThisSite
* **Challenge Name:** Realistic 1 – Uncle Arnold's Local Band Review
* **Category:** Web / Parameter Tampering
* **Difficulty:** Easy–Medium
* **Challenge Description:**
  A friend made a $500 bet that his band would reach the top of a local band review site. The site allows users to submit ratings, and the goal is to manipulate the system so that his band, *Raging Inferno*, ends up on top.

---

## TL;DR (Summary)

The voting system trusts client-side parameters. By modifying the vote value sent to the server, the band’s rating can be artificially increased.

---

## Thought Process & Approach

The page presents multiple bands, each with a form that submits a vote using the `GET` method. Since ratings are numerical and no server-side validation is apparent, this suggests a classic **parameter tampering** vulnerability.

Instead of selecting values limited by the dropdown, modifying the request parameters directly may allow arbitrary ratings.

---

## Analysis

Each band uses a similar form structure:

```
<form action="v.php" method="get">
    <input type="hidden" name="PHPSESSID" value="abcaeadfc31a5c43b2534bf995c0553f">
    <input type="hidden" name="id" value="3">
    <select name="vote">
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
        <option value="4">4</option>
        <option value="5">5</option>
    </select>
    <input type="submit" value="vote!">
</form>
```

Key observations:

* The request is sent using **GET**, making parameters visible and easy to modify.
* The `vote` parameter is trusted entirely by the server.
* No apparent validation restricts the vote value to the listed options.

---

## Exploitation

For the band **Raging Inferno**, the following parameters are submitted:

* `id=3`
* `vote=<value>`

By manually editing the `vote` parameter (either via browser developer tools or an intercepting proxy) and submitting a very large value instead of `1–5`, the average rating increases dramatically.

Doing this process places **Raging Inferno** at the top of the list, completing the mission.

---

## Final Result

Raging Inferno successfully becomes the top-rated band, and the level is completed.
