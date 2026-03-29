# picoCTF – SideChannel

## Challenge Information

* **CTF Name:** picoCTF
* **Challenge Name:** SideChannel
* **Category:** Reverse Engineering / Side-Channel Attack
* **Difficulty:** Medium–Hard
* **Description:**
  A PIN-checking binary validates input. The goal is to recover the correct PIN and use it to retrieve the flag from a remote server.

---

## TL;DR (Summary)

The binary leaks information via execution time. By measuring response times for different inputs, the correct PIN can be reconstructed digit by digit.

---

## Thought Process & Approach

The challenge provides a compiled binary. Since it is stripped and no obvious output is given, the approach is to:

1. Analyze behavior
2. Identify side-channel leaks
3. Exploit timing differences

---

## Step 1 – Binary Analysis

Check file type:

```
file pin_checker
```

Output:

```
ELF 32-bit executable
```

Install required libraries:

```
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install libc6:i386 libncurses5:i386 libstdc++6:i386
```

---

## Step 2 – Initial Inspection

Using tools:

```
strings pin_checker
ltrace ./pin_checker
```

No direct password leakage is observed.

---

## Step 3 – Identify Side-Channel

Observation:

* The program takes slightly longer when more digits of the PIN are correct

This indicates a **timing-based side-channel vulnerability**, where the program likely checks digits sequentially.

---

## Step 4 – Exploit Timing Differences

Strategy:

* Guess PIN one digit at a time
* Measure execution time
* The correct digit causes longer execution

---

## Step 5 – Automation Script

Python script used:

```
import subprocess
import time

def check(pin):
    start = time.time()
    p = subprocess.Popen(["./pin_checker"], stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
    try:
        p.communicate(input=(pin + "\n").encode(), timeout=1)
    except:
        pass
    return time.time() - start

pin = ""

for i in range(8):
    best_digit = None
    best_time = 0

    for d in "0123456789":
        test = pin + d + "0"*(7-len(pin))
        t = sum(check(test) for _ in range(3))

        if t > best_time:
            best_time = t
            best_digit = d

    pin += best_digit
    print("Current PIN:", pin)

print("Final PIN:", pin)
```

---

## Step 6 – Recovered PIN

```
48390513
```

---

## Step 7 – Retrieve Flag

Connect to remote service:

```
nc saturn.picoctf.net 60703
```

Enter PIN to receive flag.

---

## Result

```
picoCTF{t1m1ng_4tt4ck_9803bd25}
```

---

## Final Answer

```
picoCTF{t1m1ng_4tt4ck_9803bd25}
```
