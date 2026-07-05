# HTB Starting Point — Dancing (Tier 0)

**Difficulty:** Very Easy | **Category:** Windows, SMB

## Enumeration
Scanned the target with nmap:
​```
nmap -sV 10.129.121.60
​```
Result: four open ports — 135 (Microsoft RPC), 139 (NetBIOS), 445 (SMB), and 5985 (WinRM). This was my first Windows-based Starting Point box, a clear shift from the Linux services in Meow and Fawn.

## Exploitation
With SMB (445) open, I checked for a null/anonymous session — the same "check for no-auth access first" instinct as the earlier boxes, applied to a new protocol:
​```
smbclient -L //10.129.121.60 -N
​```
This listed the available shares: `ADMIN$`, `C$`, and `IPC$` (all standard default Windows admin shares present on any machine) alongside one non-default share: `WorkShares`.

Connected directly to that share:
​```
smbclient //10.129.121.60/WorkShares -N
​```
Inside, found two folders — `Amy.J` and `James.P` — apparently named after users on the system. Browsed both:
- `Amy.J` contained `worknotes.txt` (checked out of habit, even though it wasn't the flag)
- `James.P` contained `flag.txt`

Downloaded the flag directly from the SMB session:
​```
get flag.txt
​```

## Flag
Retrieved via `cat flag.txt` after exiting the SMB session.

## Lesson learned
Anonymous/null SMB sessions are a real, well-known misconfiguration on Windows networks — the same underlying category of issue as Meow's open Telnet and Fawn's anonymous FTP, just applied to a completely different protocol and OS. The core habit carries across platforms: before assuming you need credentials, always check whether a service allows unauthenticated access first. Also picked up the habit of checking every accessible file (not just the obvious flag) — on harder boxes, a stray notes file is often exactly where a credential or hint gets left behind.
