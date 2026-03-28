# OverTheWire Bandit – Level 0

## Challenge Information

* **CTF Name:** OverTheWire Bandit
* **Challenge Name:** Level 0
* **Category:** Linux / SSH
* **Difficulty:** Easy
* **Description:**
  The goal is to log into the Bandit game server using SSH with the provided credentials.

---

## TL;DR (Summary)

Used SSH to connect to the remote server using the given hostname, port, username, and password.

---

## Thought Process & Approach

The challenge explicitly mentions SSH access, so the task is straightforward: establish a remote connection using the provided credentials.

---

## Analysis / Discovery

Important details provided:

* Host: bandit.labs.overthewire.org
* Port: 2220
* Username: bandit0
* Password: bandit0

SSH syntax requires specifying the username, host, and port.

---

## Exploitation / Steps

Step 1 – Connect using SSH

```
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

Observation:

* First-time connection shows a host authenticity warning
* Requires confirmation

Step 2 – Accept Host Key

```
yes
```

Observation:

* Host key is added to known hosts

Step 3 – Enter Password

```
bandit0
```

Result:

* Successfully logged into the Bandit server

--[ Playing the games ]--

  This machine might hold several wargames.
  If you are playing "somegame", then:

    * USERNAMES are somegame0, somegame1, ...
    * Most LEVELS are stored in /somegame/.
    * PASSWORDS for each level are stored in /etc/somegame_pass/.

  Write-access to homedirectories is disabled. It is advised to create a
  working directory with a hard-to-guess name in /tmp/.  You can use the
  command "mktemp -d" in order to generate a random and hard to guess
  directory in /tmp/.  Read-access to both /tmp/ is disabled and to /proc
  restricted so that users cannot snoop on eachother. Files and directories
  with easily guessable or short names will be periodically deleted! The /tmp
  directory is regularly wiped.

--[ Tips ]--

  This machine has a 64bit processor and many security-features enabled
  by default, although ASLR has been switched off.  The following
  compiler flags might be interesting:

    -m32                    compile for 32bit
    -fno-stack-protector    disable ProPolice
    -Wl,-z,norelro          disable relro

  In addition, the execstack tool can be used to flag the stack as
  executable on ELF binaries.

  Finally, network-access is limited for most levels by a local
  firewall.

--[ Tools ]--

 For your convenience we have installed a few useful tools which you can find
 in the following locations:

    * gef (https://github.com/hugsy/gef) in /opt/gef/
    * pwndbg (https://github.com/pwndbg/pwndbg) in /opt/pwndbg/
    * gdbinit (https://github.com/gdbinit/Gdbinit) in /opt/gdbinit/
    * pwntools (https://github.com/Gallopsled/pwntools)
    * radare2 (http://www.radare.org/)


---

## Result

Access to the Bandit Level 0 shell was successfully obtained.

---

## Final Answer

```
SSH login successful using provided credentials
```
