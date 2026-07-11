## Crocidile
**Platform:** HackTheBox  
**Category:** Enumeration / FTP   
**Difficulty:** Very Easy

### What I did:

**Step 1:** Ran nmap to scan all ports and found port 21 
open with a FTP server running on it. It has a anonymous login allowed and show me two important information.
`nmap -sV -sC <target_ip>`

**Step 2:** Use this command to connect to ftp server on target machine.
`ftp <target_ip>`

**Step 3:** So use this command on ftp server to download that two files.
`get <file_name>`

**Step 4:** So in this step i used gobuster tool to find login PHP file.
`gobuster dir -u http://<target_ip> -w <list_path> -x php`

**Step 5:** Used from credentials found in Step 3 to login.

and FINISHHH ! We find flag

### What I learned:
- connect and how to use ftp server
- use from gobuster tool

### Flag: HTB{...}