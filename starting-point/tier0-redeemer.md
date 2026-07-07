# HTB Starting Point — Redeemer (Tier 0)

**Difficulty:** Very Easy | **Category:** Linux, Redis

## Enumeration
Started with a standard scan:
​```
nmap -sV 10.129.122.46
​```
Result: all 1000 default ports showed closed. Rather than assuming the box was unreachable, escalated to a full port range scan:
​```
nmap -p- -T5 10.129.122.46
​```
This turned up port 6379 open, running Redis — a service outside nmap's default top-1000 port list, which is exactly why the first scan missed it.

## Exploitation
Redis has no authentication enabled by default. Connected directly with:
​```
redis-cli -h 10.129.122.46
​```
No credentials were required at all — immediately dropped into an interactive Redis session.

From there, listed all keys in the database:
​```
keys *
​```
Found four keys, including one literally named `flag`. Retrieved it directly:
​```
get flag
​```

## Flag
Retrieved via `get flag` inside the Redis session — no file download step needed, since the flag was stored directly as a database value.

## Lesson learned
Two lessons here, one technical and one procedural:
1. **Redis instances with no authentication are a real, common, and serious misconfiguration** — anyone who can reach the port has full read/write access to the database, no login required. This happens on real, internet-facing systems, not just training labs.
2. **Don't stop at a clean default scan.** The initial top-1000 port scan showed nothing, but escalating to a full 65,535-port scan revealed the service. A thorough assessment doesn't take a "no open ports" result at face value if there's any reason to think otherwise.
