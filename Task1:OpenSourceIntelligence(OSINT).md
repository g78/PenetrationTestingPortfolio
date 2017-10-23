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
When Zone Transfers do not work the next technique to try is a DNS Bruteforce which is performed by appending names to a domain name. 

Lets find domain names using two tools - blindcrawl.pl and gxfr.py

Lets see if our target has any sub domains from a list of common names:

**Command:** *perl blindcrawl.pl -d domainName*

![blindcrawl_zt](https://user-images.githubusercontent.com/8903296/31866181-9e3e853e-b773-11e7-9972-61b9a5a2f35a.PNG)

When you need ideas for subdomains then you can always try Google!  The script for the task is gxfr.py:

**Command:** *python gxfr.py domainName --dns-lookup -v*

```
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
```

To run the Bruteforce Zone Transfer the tool `fierce` is a excellent

**Command:** *fierce -dns domainName*
```
root@kali:~/toolz# fierce -dns target.com
DNS Servers for target.com:
	ns4-65.akam.net
	ns7-64.akam.net
	ns1-168.akam.net
	ns5-65.akam.net
	tezttsdcx03p.extdns.target.com

Trying zone transfer first...
	Testing ns4-65.akam.net
		Request timed out or transfer not allowed.
	Testing ns7-64.akam.net
		Request timed out or transfer not allowed.
	Testing ns1-168.akam.net
		Request timed out or transfer not allowed.
	Testing ns5-65.akam.net
		Request timed out or transfer not allowed.
unresolvable name: tezttsdcx03p.extdns.target.com at /usr/bin/fierce line 226.
	Testing tezttsdcx03p.extdns.target.com
		Request timed out or transfer not allowed.
unresolvable name: tezttsdcx03p.extdns.target.com at /usr/bin/fierce line 236.

Unsuccessful in zone transfer (it was worth a shot)
Okay, trying the good old fashioned way... brute force

Checking for wildcard DNS...
Nope. Good.
Now performing 2280 test(s)...
130.211.24.197	ads.target.com
161.225.136.32	auction.target.com
68.233.76.200	careers.target.com
161.225.41.30	chicago.target.com
206.132.3.45	email.target.com
161.225.130.72	images.target.com
161.225.130.130	ns3.target.com
161.225.136.136	ns4.target.com
161.225.130.150	ns5.target.com
161.225.136.53	owa.target.com
192.237.143.137	security.target.com
206.132.3.45	services.target.com
161.225.130.163	shop.target.com
161.225.142.27	vc.target.com
161.225.195.239	wap.target.com
161.225.195.239	wireless.target.com
207.171.181.21	www2.target.com

Subnets found (may want to probe here using nmap or unicornscan):
	130.211.24.0-255 : 1 hostnames found.
	161.225.130.0-255 : 4 hostnames found.
	161.225.136.0-255 : 3 hostnames found.
	161.225.142.0-255 : 1 hostnames found.
	161.225.195.0-255 : 2 hostnames found.
	161.225.41.0-255 : 1 hostnames found.
	192.237.143.0-255 : 1 hostnames found.
	206.132.3.0-255 : 2 hostnames found.
	207.171.181.0-255 : 1 hostnames found.
	68.233.76.0-255 : 1 hostnames found.

Done with Fierce scan: http://ha.ckers.org/fierce/
Found 17 entries.

Have a nice day.

```

**Command:** *fierce -range 161.255.130.0-255 -dnsserver 161.255.136.130*

```

```

###Getting a List of Machine IP addresses from an Identified IP Range

Using the results we have acquired from identifying DNS Servers then sub domains leading us to a number of IP ranges we can use `NMAP` to identify all IP addresses that have a live host.

**Command:** *nmap -sL 161.255.130.0-255*
```
root@kali:~# nmap -sL 161.255.130.0-255

Starting Nmap 7.60 ( https://nmap.org ) at 2017-10-23 17:07 BST
Nmap scan report for 161.255.130.0
Nmap scan report for 161-255-130-1.genericrev.telcel.net.ve (161.255.130.1)
Nmap scan report for 161-255-130-2.genericrev.telcel.net.ve (161.255.130.2)
Nmap scan report for 161-255-130-3.genericrev.telcel.net.ve (161.255.130.3)
Nmap scan report for 161-255-130-4.genericrev.telcel.net.ve (161.255.130.4)
Nmap scan report for 161-255-130-5.genericrev.telcel.net.ve (161.255.130.5)
Nmap scan report for 161-255-130-6.genericrev.telcel.net.ve (161.255.130.6)
Nmap scan report for 161-255-130-7.genericrev.telcel.net.ve (161.255.130.7)
Nmap scan report for 161-255-130-8.genericrev.telcel.net.ve (161.255.130.8)
Nmap scan report for 161-255-130-9.genericrev.telcel.net.ve (161.255.130.9)
Nmap scan report for 161-255-130-10.genericrev.telcel.net.ve (161.255.130.10)
Nmap scan report for 161-255-130-11.genericrev.telcel.net.ve (161.255.130.11)
Nmap scan report for 161-255-130-12.genericrev.telcel.net.ve (161.255.130.12)
Nmap scan report for 161-255-130-13.genericrev.telcel.net.ve (161.255.130.13)
Nmap scan report for 161-255-130-14.genericrev.telcel.net.ve (161.255.130.14)
Nmap scan report for 161-255-130-15.genericrev.telcel.net.ve (161.255.130.15)
Nmap scan report for 161-255-130-16.genericrev.telcel.net.ve (161.255.130.16)
Nmap scan report for 161-255-130-17.genericrev.telcel.net.ve (161.255.130.17)
Nmap scan report for 161-255-130-18.genericrev.telcel.net.ve (161.255.130.18)
Nmap scan report for 161-255-130-19.genericrev.telcel.net.ve (161.255.130.19)
Nmap scan report for 161-255-130-20.genericrev.telcel.net.ve (161.255.130.20)
Nmap scan report for 161-255-130-21.genericrev.telcel.net.ve (161.255.130.21)
Nmap scan report for 161-255-130-22.genericrev.telcel.net.ve (161.255.130.22)
Nmap scan report for 161-255-130-23.genericrev.telcel.net.ve (161.255.130.23)
Nmap scan report for 161-255-130-24.genericrev.telcel.net.ve (161.255.130.24)
Nmap scan report for 161-255-130-25.genericrev.telcel.net.ve (161.255.130.25)
Nmap scan report for 161-255-130-26.genericrev.telcel.net.ve (161.255.130.26)
Nmap scan report for 161-255-130-27.genericrev.telcel.net.ve (161.255.130.27)
Nmap scan report for 161-255-130-28.genericrev.telcel.net.ve (161.255.130.28)
Nmap scan report for 161-255-130-29.genericrev.telcel.net.ve (161.255.130.29)
Nmap scan report for 161-255-130-30.genericrev.telcel.net.ve (161.255.130.30)
Nmap scan report for 161-255-130-31.genericrev.telcel.net.ve (161.255.130.31)
Nmap scan report for 161-255-130-32.genericrev.telcel.net.ve (161.255.130.32)
Nmap scan report for 161-255-130-33.genericrev.telcel.net.ve (161.255.130.33)
Nmap scan report for 161-255-130-34.genericrev.telcel.net.ve (161.255.130.34)
Nmap scan report for 161-255-130-35.genericrev.telcel.net.ve (161.255.130.35)
Nmap scan report for 161-255-130-36.genericrev.telcel.net.ve (161.255.130.36)
Nmap scan report for 161-255-130-37.genericrev.telcel.net.ve (161.255.130.37)
Nmap scan report for 161-255-130-38.genericrev.telcel.net.ve (161.255.130.38)
Nmap scan report for 161-255-130-39.genericrev.telcel.net.ve (161.255.130.39)
Nmap scan report for 161-255-130-40.genericrev.telcel.net.ve (161.255.130.40)
Nmap scan report for 161-255-130-41.genericrev.telcel.net.ve (161.255.130.41)
Nmap scan report for 161-255-130-42.genericrev.telcel.net.ve (161.255.130.42)
Nmap scan report for 161-255-130-43.genericrev.telcel.net.ve (161.255.130.43)
Nmap scan report for 161-255-130-44.genericrev.telcel.net.ve (161.255.130.44)
Nmap scan report for 161-255-130-45.genericrev.telcel.net.ve (161.255.130.45)
Nmap scan report for 161-255-130-46.genericrev.telcel.net.ve (161.255.130.46)
Nmap scan report for 161-255-130-47.genericrev.telcel.net.ve (161.255.130.47)
Nmap scan report for 161-255-130-48.genericrev.telcel.net.ve (161.255.130.48)
Nmap scan report for 161-255-130-49.genericrev.telcel.net.ve (161.255.130.49)
Nmap scan report for 161-255-130-50.genericrev.telcel.net.ve (161.255.130.50)
Nmap scan report for 161-255-130-51.genericrev.telcel.net.ve (161.255.130.51)
Nmap scan report for 161-255-130-52.genericrev.telcel.net.ve (161.255.130.52)
Nmap scan report for 161-255-130-53.genericrev.telcel.net.ve (161.255.130.53)
Nmap scan report for 161-255-130-54.genericrev.telcel.net.ve (161.255.130.54)
Nmap scan report for 161-255-130-55.genericrev.telcel.net.ve (161.255.130.55)
Nmap scan report for 161-255-130-56.genericrev.telcel.net.ve (161.255.130.56)
Nmap scan report for 161-255-130-57.genericrev.telcel.net.ve (161.255.130.57)
Nmap scan report for 161-255-130-58.genericrev.telcel.net.ve (161.255.130.58)
Nmap scan report for 161-255-130-59.genericrev.telcel.net.ve (161.255.130.59)
Nmap scan report for 161-255-130-60.genericrev.telcel.net.ve (161.255.130.60)
Nmap scan report for 161-255-130-61.genericrev.telcel.net.ve (161.255.130.61)
Nmap scan report for 161-255-130-62.genericrev.telcel.net.ve (161.255.130.62)
Nmap scan report for 161-255-130-63.genericrev.telcel.net.ve (161.255.130.63)
Nmap scan report for 161-255-130-64.genericrev.telcel.net.ve (161.255.130.64)
Nmap scan report for 161-255-130-65.genericrev.telcel.net.ve (161.255.130.65)
Nmap scan report for 161-255-130-66.genericrev.telcel.net.ve (161.255.130.66)
Nmap scan report for 161-255-130-67.genericrev.telcel.net.ve (161.255.130.67)
Nmap scan report for 161-255-130-68.genericrev.telcel.net.ve (161.255.130.68)
Nmap scan report for 161-255-130-69.genericrev.telcel.net.ve (161.255.130.69)
Nmap scan report for 161-255-130-70.genericrev.telcel.net.ve (161.255.130.70)
Nmap scan report for 161-255-130-71.genericrev.telcel.net.ve (161.255.130.71)
Nmap scan report for 161-255-130-72.genericrev.telcel.net.ve (161.255.130.72)
Nmap scan report for 161-255-130-73.genericrev.telcel.net.ve (161.255.130.73)
Nmap scan report for 161-255-130-74.genericrev.telcel.net.ve (161.255.130.74)
Nmap scan report for 161.255.130.75
Nmap scan report for 161-255-130-76.genericrev.telcel.net.ve (161.255.130.76)
Nmap scan report for 161-255-130-77.genericrev.telcel.net.ve (161.255.130.77)
Nmap scan report for 161-255-130-78.genericrev.telcel.net.ve (161.255.130.78)
Nmap scan report for 161-255-130-79.genericrev.telcel.net.ve (161.255.130.79)
Nmap scan report for 161-255-130-80.genericrev.telcel.net.ve (161.255.130.80)
Nmap scan report for 161-255-130-81.genericrev.telcel.net.ve (161.255.130.81)
Nmap scan report for 161-255-130-82.genericrev.telcel.net.ve (161.255.130.82)
Nmap scan report for 161-255-130-83.genericrev.telcel.net.ve (161.255.130.83)
Nmap scan report for 161-255-130-84.genericrev.telcel.net.ve (161.255.130.84)
Nmap scan report for 161-255-130-85.genericrev.telcel.net.ve (161.255.130.85)
Nmap scan report for 161-255-130-86.genericrev.telcel.net.ve (161.255.130.86)
Nmap scan report for 161-255-130-87.genericrev.telcel.net.ve (161.255.130.87)
Nmap scan report for 161-255-130-88.genericrev.telcel.net.ve (161.255.130.88)
Nmap scan report for 161-255-130-89.genericrev.telcel.net.ve (161.255.130.89)
Nmap scan report for 161-255-130-90.genericrev.telcel.net.ve (161.255.130.90)
Nmap scan report for 161-255-130-91.genericrev.telcel.net.ve (161.255.130.91)
Nmap scan report for 161-255-130-92.genericrev.telcel.net.ve (161.255.130.92)
Nmap scan report for 161-255-130-93.genericrev.telcel.net.ve (161.255.130.93)
Nmap scan report for 161-255-130-94.genericrev.telcel.net.ve (161.255.130.94)
Nmap scan report for 161-255-130-95.genericrev.telcel.net.ve (161.255.130.95)
Nmap scan report for 161-255-130-96.genericrev.telcel.net.ve (161.255.130.96)
Nmap scan report for 161-255-130-97.genericrev.telcel.net.ve (161.255.130.97)
Nmap scan report for 161.255.130.98
Nmap scan report for 161-255-130-99.genericrev.telcel.net.ve (161.255.130.99)
Nmap scan report for 161-255-130-100.genericrev.telcel.net.ve (161.255.130.100)
Nmap scan report for 161-255-130-101.genericrev.telcel.net.ve (161.255.130.101)
Nmap scan report for 161-255-130-102.genericrev.telcel.net.ve (161.255.130.102)
Nmap scan report for 161-255-130-103.genericrev.telcel.net.ve (161.255.130.103)
Nmap scan report for 161-255-130-104.genericrev.telcel.net.ve (161.255.130.104)
Nmap scan report for 161-255-130-105.genericrev.telcel.net.ve (161.255.130.105)
Nmap scan report for 161-255-130-106.genericrev.telcel.net.ve (161.255.130.106)
Nmap scan report for 161-255-130-107.genericrev.telcel.net.ve (161.255.130.107)
Nmap scan report for 161-255-130-108.genericrev.telcel.net.ve (161.255.130.108)
Nmap scan report for 161-255-130-109.genericrev.telcel.net.ve (161.255.130.109)
Nmap scan report for 161-255-130-110.genericrev.telcel.net.ve (161.255.130.110)
Nmap scan report for 161-255-130-111.genericrev.telcel.net.ve (161.255.130.111)
Nmap scan report for 161-255-130-112.genericrev.telcel.net.ve (161.255.130.112)
Nmap scan report for 161-255-130-113.genericrev.telcel.net.ve (161.255.130.113)
Nmap scan report for 161-255-130-114.genericrev.telcel.net.ve (161.255.130.114)
Nmap scan report for 161-255-130-115.genericrev.telcel.net.ve (161.255.130.115)
Nmap scan report for 161-255-130-116.genericrev.telcel.net.ve (161.255.130.116)
Nmap scan report for 161-255-130-117.genericrev.telcel.net.ve (161.255.130.117)
Nmap scan report for 161-255-130-118.genericrev.telcel.net.ve (161.255.130.118)
Nmap scan report for 161-255-130-119.genericrev.telcel.net.ve (161.255.130.119)
Nmap scan report for 161-255-130-120.genericrev.telcel.net.ve (161.255.130.120)
Nmap scan report for 161-255-130-121.genericrev.telcel.net.ve (161.255.130.121)
Nmap scan report for 161-255-130-122.genericrev.telcel.net.ve (161.255.130.122)
Nmap scan report for 161-255-130-123.genericrev.telcel.net.ve (161.255.130.123)
Nmap scan report for 161-255-130-124.genericrev.telcel.net.ve (161.255.130.124)
Nmap scan report for 161-255-130-125.genericrev.telcel.net.ve (161.255.130.125)
Nmap scan report for 161-255-130-126.genericrev.telcel.net.ve (161.255.130.126)
Nmap scan report for 161-255-130-127.genericrev.telcel.net.ve (161.255.130.127)
Nmap scan report for 161-255-130-128.genericrev.telcel.net.ve (161.255.130.128)
Nmap scan report for 161-255-130-129.genericrev.telcel.net.ve (161.255.130.129)
Nmap scan report for 161-255-130-130.genericrev.telcel.net.ve (161.255.130.130)
Nmap scan report for 161-255-130-131.genericrev.telcel.net.ve (161.255.130.131)
Nmap scan report for 161-255-130-132.genericrev.telcel.net.ve (161.255.130.132)
Nmap scan report for 161-255-130-133.genericrev.telcel.net.ve (161.255.130.133)
Nmap scan report for 161-255-130-134.genericrev.telcel.net.ve (161.255.130.134)
Nmap scan report for 161-255-130-135.genericrev.telcel.net.ve (161.255.130.135)
Nmap scan report for 161-255-130-136.genericrev.telcel.net.ve (161.255.130.136)
Nmap scan report for 161-255-130-137.genericrev.telcel.net.ve (161.255.130.137)
Nmap scan report for 161-255-130-138.genericrev.telcel.net.ve (161.255.130.138)
Nmap scan report for 161-255-130-139.genericrev.telcel.net.ve (161.255.130.139)
Nmap scan report for 161-255-130-140.genericrev.telcel.net.ve (161.255.130.140)
Nmap scan report for 161-255-130-141.genericrev.telcel.net.ve (161.255.130.141)
Nmap scan report for 161-255-130-142.genericrev.telcel.net.ve (161.255.130.142)
Nmap scan report for 161-255-130-143.genericrev.telcel.net.ve (161.255.130.143)
Nmap scan report for 161-255-130-144.genericrev.telcel.net.ve (161.255.130.144)
Nmap scan report for 161-255-130-145.genericrev.telcel.net.ve (161.255.130.145)
Nmap scan report for 161-255-130-146.genericrev.telcel.net.ve (161.255.130.146)
Nmap scan report for 161-255-130-147.genericrev.telcel.net.ve (161.255.130.147)
Nmap scan report for 161-255-130-148.genericrev.telcel.net.ve (161.255.130.148)
Nmap scan report for 161-255-130-149.genericrev.telcel.net.ve (161.255.130.149)
Nmap scan report for 161-255-130-150.genericrev.telcel.net.ve (161.255.130.150)
Nmap scan report for 161-255-130-151.genericrev.telcel.net.ve (161.255.130.151)
Nmap scan report for 161-255-130-152.genericrev.telcel.net.ve (161.255.130.152)
Nmap scan report for 161-255-130-153.genericrev.telcel.net.ve (161.255.130.153)
Nmap scan report for 161-255-130-154.genericrev.telcel.net.ve (161.255.130.154)
Nmap scan report for 161-255-130-155.genericrev.telcel.net.ve (161.255.130.155)
Nmap scan report for 161-255-130-156.genericrev.telcel.net.ve (161.255.130.156)
Nmap scan report for 161-255-130-157.genericrev.telcel.net.ve (161.255.130.157)
Nmap scan report for 161-255-130-158.genericrev.telcel.net.ve (161.255.130.158)
Nmap scan report for 161-255-130-159.genericrev.telcel.net.ve (161.255.130.159)
Nmap scan report for 161-255-130-160.genericrev.telcel.net.ve (161.255.130.160)
Nmap scan report for 161-255-130-161.genericrev.telcel.net.ve (161.255.130.161)
Nmap scan report for 161-255-130-162.genericrev.telcel.net.ve (161.255.130.162)
Nmap scan report for 161-255-130-163.genericrev.telcel.net.ve (161.255.130.163)
Nmap scan report for 161-255-130-164.genericrev.telcel.net.ve (161.255.130.164)
Nmap scan report for 161-255-130-165.genericrev.telcel.net.ve (161.255.130.165)
Nmap scan report for 161-255-130-166.genericrev.telcel.net.ve (161.255.130.166)
Nmap scan report for 161-255-130-167.genericrev.telcel.net.ve (161.255.130.167)
Nmap scan report for 161-255-130-168.genericrev.telcel.net.ve (161.255.130.168)
Nmap scan report for 161-255-130-169.genericrev.telcel.net.ve (161.255.130.169)
Nmap scan report for 161-255-130-170.genericrev.telcel.net.ve (161.255.130.170)
Nmap scan report for 161-255-130-171.genericrev.telcel.net.ve (161.255.130.171)
Nmap scan report for 161-255-130-172.genericrev.telcel.net.ve (161.255.130.172)
Nmap scan report for 161-255-130-173.genericrev.telcel.net.ve (161.255.130.173)
Nmap scan report for 161-255-130-174.genericrev.telcel.net.ve (161.255.130.174)
Nmap scan report for 161-255-130-175.genericrev.telcel.net.ve (161.255.130.175)
Nmap scan report for 161-255-130-176.genericrev.telcel.net.ve (161.255.130.176)
Nmap scan report for 161-255-130-177.genericrev.telcel.net.ve (161.255.130.177)
Nmap scan report for 161-255-130-178.genericrev.telcel.net.ve (161.255.130.178)
Nmap scan report for 161-255-130-179.genericrev.telcel.net.ve (161.255.130.179)
Nmap scan report for 161-255-130-180.genericrev.telcel.net.ve (161.255.130.180)
Nmap scan report for 161-255-130-181.genericrev.telcel.net.ve (161.255.130.181)
Nmap scan report for 161-255-130-182.genericrev.telcel.net.ve (161.255.130.182)
Nmap scan report for 161-255-130-183.genericrev.telcel.net.ve (161.255.130.183)
Nmap scan report for 161-255-130-184.genericrev.telcel.net.ve (161.255.130.184)
Nmap scan report for 161-255-130-185.genericrev.telcel.net.ve (161.255.130.185)
Nmap scan report for 161-255-130-186.genericrev.telcel.net.ve (161.255.130.186)
Nmap scan report for 161-255-130-187.genericrev.telcel.net.ve (161.255.130.187)
Nmap scan report for 161-255-130-188.genericrev.telcel.net.ve (161.255.130.188)
Nmap scan report for 161-255-130-189.genericrev.telcel.net.ve (161.255.130.189)
Nmap scan report for 161-255-130-190.genericrev.telcel.net.ve (161.255.130.190)
Nmap scan report for 161-255-130-191.genericrev.telcel.net.ve (161.255.130.191)
Nmap scan report for 161-255-130-192.genericrev.telcel.net.ve (161.255.130.192)
Nmap scan report for 161-255-130-193.genericrev.telcel.net.ve (161.255.130.193)
Nmap scan report for 161-255-130-194.genericrev.telcel.net.ve (161.255.130.194)
Nmap scan report for 161-255-130-195.genericrev.telcel.net.ve (161.255.130.195)
Nmap scan report for 161-255-130-196.genericrev.telcel.net.ve (161.255.130.196)
Nmap scan report for 161-255-130-197.genericrev.telcel.net.ve (161.255.130.197)
Nmap scan report for 161-255-130-198.genericrev.telcel.net.ve (161.255.130.198)
Nmap scan report for 161-255-130-199.genericrev.telcel.net.ve (161.255.130.199)
Nmap scan report for 161-255-130-200.genericrev.telcel.net.ve (161.255.130.200)
Nmap scan report for 161-255-130-201.genericrev.telcel.net.ve (161.255.130.201)
Nmap scan report for 161-255-130-202.genericrev.telcel.net.ve (161.255.130.202)
Nmap scan report for 161-255-130-203.genericrev.telcel.net.ve (161.255.130.203)
Nmap scan report for 161-255-130-204.genericrev.telcel.net.ve (161.255.130.204)
Nmap scan report for 161-255-130-205.genericrev.telcel.net.ve (161.255.130.205)
Nmap scan report for 161-255-130-206.genericrev.telcel.net.ve (161.255.130.206)

Nmap scan report for 161-255-130-207.genericrev.telcel.net.ve (161.255.130.207)
Nmap scan report for 161-255-130-208.genericrev.telcel.net.ve (161.255.130.208)
Nmap scan report for 161-255-130-209.genericrev.telcel.net.ve (161.255.130.209)
Nmap scan report for 161-255-130-210.genericrev.telcel.net.ve (161.255.130.210)
Nmap scan report for 161-255-130-211.genericrev.telcel.net.ve (161.255.130.211)
Nmap scan report for 161-255-130-212.genericrev.telcel.net.ve (161.255.130.212)
Nmap scan report for 161-255-130-213.genericrev.telcel.net.ve (161.255.130.213)
Nmap scan report for 161-255-130-214.genericrev.telcel.net.ve (161.255.130.214)
Nmap scan report for 161-255-130-215.genericrev.telcel.net.ve (161.255.130.215)
Nmap scan report for 161-255-130-216.genericrev.telcel.net.ve (161.255.130.216)
Nmap scan report for 161-255-130-217.genericrev.telcel.net.ve (161.255.130.217)
Nmap scan report for 161-255-130-218.genericrev.telcel.net.ve (161.255.130.218)
Nmap scan report for 161-255-130-219.genericrev.telcel.net.ve (161.255.130.219)
Nmap scan report for 161-255-130-220.genericrev.telcel.net.ve (161.255.130.220)
Nmap scan report for 161-255-130-221.genericrev.telcel.net.ve (161.255.130.221)
Nmap scan report for 161-255-130-222.genericrev.telcel.net.ve (161.255.130.222)
Nmap scan report for 161-255-130-223.genericrev.telcel.net.ve (161.255.130.223)
Nmap scan report for 161-255-130-224.genericrev.telcel.net.ve (161.255.130.224)
Nmap scan report for 161-255-130-225.genericrev.telcel.net.ve (161.255.130.225)
Nmap scan report for 161.255.130.226
Nmap scan report for 161-255-130-227.genericrev.telcel.net.ve (161.255.130.227)
Nmap scan report for 161-255-130-228.genericrev.telcel.net.ve (161.255.130.228)
Nmap scan report for 161-255-130-229.genericrev.telcel.net.ve (161.255.130.229)
Nmap scan report for 161-255-130-230.genericrev.telcel.net.ve (161.255.130.230)
Nmap scan report for 161-255-130-231.genericrev.telcel.net.ve (161.255.130.231)
Nmap scan report for 161-255-130-232.genericrev.telcel.net.ve (161.255.130.232)
Nmap scan report for 161-255-130-233.genericrev.telcel.net.ve (161.255.130.233)
Nmap scan report for 161-255-130-234.genericrev.telcel.net.ve (161.255.130.234)
Nmap scan report for 161-255-130-235.genericrev.telcel.net.ve (161.255.130.235)
Nmap scan report for 161.255.130.236
Nmap scan report for 161.255.130.237
Nmap scan report for 161.255.130.238
Nmap scan report for 161.255.130.239
Nmap scan report for 161.255.130.240
Nmap scan report for 161.255.130.241
Nmap scan report for 161.255.130.242
Nmap scan report for 161.255.130.243
Nmap scan report for 161-255-130-244.genericrev.telcel.net.ve (161.255.130.244)
Nmap scan report for 161.255.130.245
Nmap scan report for 161.255.130.246
Nmap scan report for 161-255-130-247.genericrev.telcel.net.ve (161.255.130.247)
Nmap scan report for 161.255.130.248
Nmap scan report for 161.255.130.249
Nmap scan report for 161.255.130.250
Nmap scan report for 161-255-130-251.genericrev.telcel.net.ve (161.255.130.251)
Nmap scan report for 161-255-130-252.genericrev.telcel.net.ve (161.255.130.252)
Nmap scan report for 161-255-130-253.genericrev.telcel.net.ve (161.255.130.253)
Nmap scan report for 161-255-130-254.genericrev.telcel.net.ve (161.255.130.254)
Nmap scan report for 161.255.130.255
Nmap done: 256 IP addresses (0 hosts up) scanned in 37.39 seconds

```

#Automating the Process
Instead of running these tasks step by step manually it is good form to attempt to automate parts of the OSINT process.  I've included below a python script that runs each of the steps and parses the results preparing them for the next auomtation step in the process.
