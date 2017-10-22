# Skill Demonstration Task 1: Open Source Intelligence (OSINT)

The first step in enumerating a target network is via `DNS`.  To help with the heavy lifting use `dig`

**Command:** *dig domainName*
![dig1](https://user-images.githubusercontent.com/8903296/31865016-9be7af68-b75f-11e7-82c0-c4be41d3ca77.PNG)

Now we have the Authority IP addresses we can use `dig` to query the MX records.  In many examples a company will hosts its webserver with a hosting company and run it's own mail functions.  Comparing the Authority and MX records can show discrepencies allowing you to zone in on the target networks real IP range

**Command: ** *dig domainName MX*
