## Burp Suite – URL Fuzzing 

### Objective

Discover hidden directories under:

```
https://www.hackthissite.org/missions/basic/11/
```

---

### Tool

* **Burp Suite → Intruder**
* Attack type: **Sniper**
* Target: https://www.hackthissite.org

---

### Step 1: Capture a Valid Request

Use **Burp Browser** to visit:

```
https://www.hackthissite.org/missions/basic/11/
```

Intercept the request and send it to **Intruder**.

---

### Step 2: Prepare the Request

Replace the entire request with:

```
GET /missions/basic/11/§payload§/ HTTP/1.1
Host: www.hackthissite.org
Cookie: HackThisSite=<your_session_cookie>
```

**Notes:**

* HTTP method: **GET**
* **No request body**
* **No query parameters**

---

### Step 3: Configure Positions

* Click **Clear §**
* Highlight `payload`
* Click **Add §**

Final path:

```
/missions/basic/11/§payload§/
```

---

### Step 4: Configure Payloads

* Payload type: **Simple list**
* Payloads:
```
Add from list...
```
---

### Step 5: Run the Attack

* Start the Intruder attack
* Use capture and view filter to filter required responses easily

---

### Result Analysis

* Most payloads return with `404` status
* Payload `e` returns a `200` status
* Visiting:

```
/missions/basic/11/e/
```

reveals an **Apache directory listing**

---

### Final Outcome

Hidden directory discovered:

```
/missions/basic/11/e/
```
