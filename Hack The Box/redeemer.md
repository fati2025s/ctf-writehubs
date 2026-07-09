## Redeemer
**Platform:** HackTheBox  
**Category:** Enumeration / Redis  
**Difficulty:** Very Easy

### What I did:
**Step 1:** Ran nmap to scan all ports and found port 6379 
open with a Redis server running on it.
`nmap -p- -T5 <target_ip>`

**Step 2:** Researched Redis and learned that it is an 
in-memory database.

**Step 3:** Used redis-cli to connect to the server and 
retrieved the flag.

### What I learned:
- How to scan all ports with nmap
- What Redis is and how it works
- How to interact with Redis using redis-cli

### Flag: HTB{...}