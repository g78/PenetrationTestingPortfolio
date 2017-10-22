# Skill Demonstration Task 1: Open Source Intelligence (OSINT)

The first step in enumerating a target network is via `DNS`.  To help with the heavy lifting use `dig`

**Command:** *dig domainName*
![dig1](https://user-images.githubusercontent.com/8903296/31865016-9be7af68-b75f-11e7-82c0-c4be41d3ca77.PNG)

##Mail - MX Records
Now we have the Authority IP addresses we can use `dig` to query the MX records.  In many examples a company will hosts its webserver with a hosting company and run it's own mail functions.  Comparing the Authority and MX records can show discrepencies allowing you to zone in on the target networks real IP range

**Command:** *dig domainName MX*

![dig_mx](https://user-images.githubusercontent.com/8903296/31865599-32a2d35c-b769-11e7-8ace-88c62a3fe5a8.PNG)

**Command:** *dig domainName MX +noall +answer*
![dig_mx_noall_answer](https://user-images.githubusercontent.com/8903296/31865641-f6a79f58-b769-11e7-8cc5-5daa3da3ccc3.PNG)

**Command:** *dig domainName MX +short*

![dig_mx_short](https://user-images.githubusercontent.com/8903296/31865683-8b021d36-b76a-11e7-8eb2-c118c8aa18ac.PNG)

The results provided by our DNS searches are consistent meaning that we can add the results to our matrix and move onto querying the NS records. 

##Name Server - NS Records
An NS records is used to delgate a subdomain to a set of name servers.  When a domain is delegated to DNS the Top Level Domain (TLD) authorities place NS (Name Server) records for your domain in the TLD Name Servers.

**Command:** *dig domainName NS*

