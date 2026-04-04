# picoCTF – format string 0

## Challenge Information

* **CTF Name:** picoCTF
* **Challenge Name:** format string 0
* **Category:** Binary Exploitation
* **Difficulty:** Easy–Medium
* **Description:**
  A menu-driven program allows user input that is passed directly into printf. The goal is to exploit format string vulnerabilities to retrieve the flag.

---

## TL;DR (Summary)

User input is used as a format string in printf. By selecting inputs containing format specifiers, we first bypass a length check and then trigger a segmentation fault, which invokes a signal handler that prints the flag.

---

## Thought Process & Approach

The challenge name suggests a format string vulnerability. The first step is to inspect how user input is handled and identify unsafe printf usage.

---

## Analysis / Discovery

### Step 1 – Identify Vulnerability

Relevant code:

```
int count = printf(choice1);
printf(choice2);
```

Observation:

* User input is directly passed as format string
* No format specifier like "%s" is used

Inference:

* Classic format string vulnerability

---

### Step 2 – Identify Flag Trigger

```
void sigsegv_handler(int sig) {
    printf("\n%s\n", flag);
    exit(1);
}
```

Observation:

* Flag is printed on segmentation fault

Inference:

* Goal: trigger a crash

---

### Step 3 – Analyze Stage 1 (Patrick)

Options include:

```
Gr%114d_Cheese
```

Observation:

* Contains %114d

Inference:

* Forces printf to print large output

Condition:

```
if (count > 64)
```

Result:

* %114d → output length > 64 → passes condition

---

### Step 4 – Analyze Stage 2 (Bob)

Option:

```
Cla%sic_Che%s%steak
```

Observation:

* Contains multiple %s specifiers

Inference:

* %s expects pointer arguments
* No arguments provided → reads garbage stack values
* Dereferencing invalid pointers → crash

---

## Exploitation / Steps

### Step 5 – Trigger Execution Path

Run binary:

```
./format-string-0
```

Input 1:

```
Gr%114d_Cheese
```

Input 2:

```
Cla%sic_Che%s%steak
```

---

## Result

* Program crashes (SIGSEGV)
* Signal handler executes
* Flag is printed

---

## Final Answer

```
picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_a1d85b3e}
```
