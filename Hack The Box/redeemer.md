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
`redis-cli -h 10.129.130.84 -p 6379`

**Step 4:** Used KEYS * to saw all keys that exists on that.

**Step 5:** Used GET flag to get flag.

### What I learned:
- How to scan all ports with nmap
- What Redis is and how it works
- How to interact with Redis using redis-cli

### Flag: HTB{...}