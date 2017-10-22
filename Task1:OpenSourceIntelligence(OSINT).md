# Skill Demonstration Task 1: Open Source Intelligence (OSINT)

The first step in enumerating a target network is via `DNS`.  To help with the heavy lifting use `dig`

**Command:** *dig domainName*
![dig1](https://user-images.githubusercontent.com/8903296/31865016-9be7af68-b75f-11e7-82c0-c4be41d3ca77.PNG)

## Mail - MX Records
Now we have the Authority IP addresses we can use `dig` to query the MX records.  In many examples a company will hosts its webserver with a hosting company and run it's own mail functions.  Comparing the Authority and MX records can show discrepencies allowing you to zone in on the target networks real IP range

**Command:** *dig domainName MX*

![dig_mx](https://user-images.githubusercontent.com/8903296/31865599-32a2d35c-b769-11e7-8ace-88c62a3fe5a8.PNG)

**Command:** *dig domainName MX +noall +answer*
![dig_mx_noall_answer](https://user-images.githubusercontent.com/8903296/31865641-f6a79f58-b769-11e7-8cc5-5daa3da3ccc3.PNG)

**Command:** *dig domainName MX +short*

![dig_mx_short](https://user-images.githubusercontent.com/8903296/31865683-8b021d36-b76a-11e7-8eb2-c118c8aa18ac.PNG)


The results provided by our DNS searches are consistent meaning that we can add the results to our matrix and move onto querying the NS records. 

## Name Server - NS Records
An NS records is used to delgate a subdomain to a set of name servers.  When a domain is delegated to DNS the Top Level Domain (TLD) authorities place NS (Name Server) records for your domain in the TLD Name Servers.

**Command:** *dig domainName NS*
![dig_ns](https://user-images.githubusercontent.com/8903296/31865812-b5979272-b76c-11e7-8522-cc9caccd6478.PNG)

**Command:** *dig domaninName NS +noall +answer*
![dig_ns_noall_answer](https://user-images.githubusercontent.com/8903296/31866049-ef2f0fd4-b770-11e7-9461-e5dd6f09023c.PNG)

**Command:** *dig domainName NS +short*

![dig_ns_short](https://user-images.githubusercontent.com/8903296/31866063-124292ca-b771-11e7-8b73-24cff98a5960.PNG)

## Address - A Records

**Command:** *dig domainName A*

![dig_a](https://user-images.githubusercontent.com/8903296/31866067-2b60a6a2-b771-11e7-9e12-7aba5be4008f.PNG)

**Command:** *dig domainName A +noall +answer*

![dig_a_noall_answer](https://user-images.githubusercontent.com/8903296/31866071-44c3a05e-b771-11e7-8b14-6f4685ca9625.PNG)

**Command:** *dig domainName A +short*

![dig_a_short](https://user-images.githubusercontent.com/8903296/31866073-55c291b2-b771-11e7-8817-dafcf487b4f9.PNG)

## Pointer - PTR Records
**Command:** *dig domainName PTR*

![dig_ptr](https://user-images.githubusercontent.com/8903296/31866078-744ca230-b771-11e7-8d3d-b9fafc290251.PNG)

## Attempting a Zone Transfer
The next three examples using dig are a basic attempts that should fail unless the DNS has been badly misconfigured
**Command:** *dig domainName AXFR*

![dig_axfr](https://user-images.githubusercontent.com/8903296/31866083-8a3a0f74-b771-11e7-9b47-5ba6a68d281b.PNG)

**Command:** *dig domainName AXFR +noall +answer*

![dig_axfr_noall_answer](https://user-images.githubusercontent.com/8903296/31866091-a8cb7752-b771-11e7-8c54-b7fc94b665eb.PNG)

Dig can also be used to undertake `Incremental` Zone Transfers

**Command:** *dig domainName IXFR*

![dig_ixfr](https://user-images.githubusercontent.com/8903296/31866095-bc577faa-b771-11e7-8417-d0aab25150e8.PNG)

### Bruteforce Zone Transfers
When Zone Transfers do not work the next technique to try is a DNS Bruteforce which is performed by appending names to a domain name

**Command:** *perl blindcrawl.pl -d domainName*

![blindcrawl_zt](https://user-images.githubusercontent.com/8903296/31866181-9e3e853e-b773-11e7-9972-61b9a5a2f35a.PNG)

When you need ideas for subdomains then you can always try Google!  The script for the task is gxfr.py

**Command:** *python gxfr.py domainName --dns-lookup -v*

'''
root@kali:~/toolz# python gxfr.py target.com --dns-lookup -v
[-] domain: target.com
[-] user-agent: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Trident/4.0; FDM; .NET CLR 2.0.50727; InfoPath.2; .NET CLR 1.1.4322)
[-] querying search engine, please wait...
[+] using query: https://www.google.com/m/search?q=site%3Atarget.com&start=0...
[!] subdomain found: www
[!] subdomain found: style
[!] subdomain found: investors
[!] subdomain found: coupons
[+] sleeping to avoid lock-out...
[+] using query: https://www.google.com/m/search?q=site%3Atarget.com+-site%3Awww.target.com+-site%3Astyle.target.com+-site%3Ainvestors.target.com+-site%3Acoupons.target.com&start=0...
[!] subdomain found: affiliate
[+] sleeping to avoid lock-out...
[+] using query: https://www.google.com/m/search?q=site%3Atarget.com+-site%3Awww.target.com+-site%3Astyle.target.com+-site%3Ainvestors.target.com+-site%3Acoupons.target.com+-site%3Aaffiliate.target.com&start=0...
[-] all available subdomains found...
[-] successful queries made: 3
[+] final query string: https://www.google.com/m/search?q=site%3Atarget.com+-site%3Awww.target.com+-site%3Astyle.target.com+-site%3Ainvestors.target.com+-site%3Acoupons.target.com+-site%3Aaffiliate.target.com&start=0
 
[subdomains] - 5
www.target.com 184.87.187.104
style.target.com 104.86.110.34,104.86.110.80
investors.target.com 87.237.22.178,87.237.22.192
coupons.target.com 64.75.15.140
affiliate.target.com 98.129.229.242
 
[-] querying dns, please wait...
[+] querying dns for www.target.com...
[+] querying dns for style.target.com...
[+] querying dns for investors.target.com...
[+] querying dns for coupons.target.com...
[+] querying dns for affiliate.target.com...
 
[ip]            [subdomain]
64.75.15.140    coupons.target.com
87.237.22.192   investors.target.com
87.237.22.178   investors.target.com
98.129.229.242  affiliate.target.com
184.87.187.104  www.target.com
104.86.110.80   style.target.com
104.86.110.34   style.target.com
'''
