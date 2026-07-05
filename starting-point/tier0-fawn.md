# HTB Starting Point — Fawn (Tier 0)

**Difficulty:** Very Easy | **Category:** Linux, FTP

## Enumeration
​```
nmap -sV 10.129.121.10
​```
Result: port 21 open, running vsftpd 3.0.3.

## Exploitation
Connected via `ftp <target-ip>` and logged in using the anonymous FTP account (username `anonymous`, blank password) — a classic FTP misconfiguration.

Listed the directory and found `flag.txt` sitting in the anonymous-accessible root.

## Flag
Downloaded with `get flag.txt`, then read locally with `cat flag.txt`.

## Lesson learned
Anonymous FTP access left enabled on a production system is a real, recurring misconfiguration — same category of issue as Meow's Telnet gap: a legacy, unauthenticated service exposing sensitive files.
