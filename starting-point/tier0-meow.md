# HTB Starting Point — Meow (Tier 0)

**Difficulty:** Very Easy | **Category:** Linux, Telnet

## Enumeration
Scanned the target with nmap:
​```
nmap -sV 10.129.120.240
​```
Result: only port 23 (Telnet) open, running Linux telnetd.

## Exploitation
Telnet transmits credentials in plaintext with no encryption — a strong signal to test weak/default credentials directly. Connected via `telnet <target-ip>` and tried username `root` with no password.

Logged in immediately as root — no password required at all.

## Flag
Retrieved `flag.txt` from root's home directory via `cat flag.txt`.

## Lesson learned
Default or blank credentials on exposed administrative services (especially unencrypted ones like Telnet) remain a real, critical vulnerability class — not just a training exercise. This is exactly the kind of finding a SOC analyst or pentester flags immediately in a real assessment.
