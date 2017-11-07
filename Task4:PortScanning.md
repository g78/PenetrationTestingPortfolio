# Task 4: Port Scanning

## SYN Scan
nmap -sS ipAddress

## ACK Scan
nmap -sA ipAddress

## Adding in Timing Options
NMAP offers six timing templates which are specified with -T (0-5).  The template names are 0: Paranoid, 1: Sneaky, 2: Polite, 3: Normal, 4: Aggressive, 5: Insane.

0 and 1: IDS Evasion
2: Use less bandwidth and target machine resources
3: Default to technically doesn't do anything
4: Speeds up the scan as it assumes the network the scan is originating on has high bandwidth
5: Willing to sacrifice accuracy for speed, assumption is made that the scan is originating on a network with mega fast network bandwidth

From a personal point of view I use T4, however if there is a requirement for a very slow scan I will use T2 but that's a rare occassion. 
For a cautious scanners user the default.
Examples:
NMAP -sS -T4 ipAddress

NMAP -sS -T Polite ipAddress

NMAP -sS --max-rtt-timeout 1250ms ipAddress

## Service and OS Detection

*Command:* **nmap -sS -O -A -T4 192.168.107.146**

```
nmap -sS -O -A -T4 192.168.107.146

Starting Nmap 7.60 ( https://nmap.org ) at 2017-11-07 21:44 GMT
Nmap scan report for 192.168.107.146
Host is up (0.00019s latency).
Not shown: 989 closed ports
PORT      STATE SERVICE     VERSION
25/tcp    open  ftp         vsftpd 3.0.2
|_smtp-commands: SMTP: EHLO 530 Please login with USER and PASS.\x0D
80/tcp    open  http        Apache httpd 2.4.7 ((Ubuntu))
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Kevgir VM
111/tcp   open  rpcbind     2-4 (RPC #100000)
| rpcinfo: 
|   program version   port/proto  service
|   100000  2,3,4        111/tcp  rpcbind
|   100000  2,3,4        111/udp  rpcbind
|   100003  2,3,4       2049/tcp  nfs
|   100003  2,3,4       2049/udp  nfs
|   100005  1,2,3      32769/tcp  mountd
|   100005  1,2,3      40137/udp  mountd
|   100021  1,3,4      38105/tcp  nlockmgr
|   100021  1,3,4      48801/udp  nlockmgr
|   100024  1          46936/tcp  status
|   100024  1          49875/udp  status
|   100227  2,3         2049/tcp  nfs_acl
|_  100227  2,3         2049/udp  nfs_acl
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp   open  netbios-ssn Samba smbd 4.1.6-Ubuntu (workgroup: WORKGROUP)
1322/tcp  open  ssh         OpenSSH 6.6.1p1 Ubuntu 2ubuntu2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 17:32:b4:85:06:20:b6:90:5b:75:1c:6e:fe:0f:f8:e2 (DSA)
|   2048 53:49:03:32:86:0b:15:b8:a5:f1:2b:8e:75:1b:5a:06 (RSA)
|   256 3b:03:cd:29:7b:5e:9f:3b:62:79:ed:dc:82:c7:48:8a (ECDSA)
|_  256 11:99:87:52:15:c8:ae:96:64:73:d6:49:8c:d7:d7:9f (EdDSA)
2049/tcp  open  nfs_acl     2-3 (RPC #100227)
8080/tcp  open  http        Apache Tomcat/Coyote JSP engine 1.1
| http-methods: 
|_  Potentially risky methods: PUT DELETE
|_http-open-proxy: Proxy might be redirecting requests
|_http-server-header: Apache-Coyote/1.1
|_http-title: Apache Tomcat
8081/tcp  open  http        Apache httpd 2.4.7 ((Ubuntu))
|_http-generator: Joomla! 1.5 - Open Source Content Management
| http-robots.txt: 14 disallowed entries 
| /administrator/ /cache/ /components/ /images/ 
| /includes/ /installation/ /language/ /libraries/ /media/ 
|_/modules/ /plugins/ /templates/ /tmp/ /xmlrpc/
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Welcome to the Frontpage
9000/tcp  open  http        Jetty winstone-2.9
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: Jetty(winstone-2.9)
|_http-title: Dashboard [Jenkins]
32769/tcp open  mountd      1-3 (RPC #100005)
MAC Address: 00:0C:29:B7:19:7A (VMware)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.8
Network Distance: 1 hop
Service Info: Host: CANYOUPWNME; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_nbstat: NetBIOS name: CANYOUPWNME, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Unix (Samba 4.1.6-Ubuntu)
|   Computer name: canyoupwnme
|   NetBIOS computer name: CANYOUPWNME\x00
|   Domain name: 
|   FQDN: canyoupwnme
|_  System time: 2017-11-07T23:44:31+02:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2017-11-07 21:44:31
|_  start_date: 1600-12-31 23:58:45

TRACEROUTE
HOP RTT     ADDRESS
1   0.19 ms 192.168.107.146

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.50 seconds

```

Adding in a port range:
*Command:* **nmap -sS -O -A -T4 -p1-65535 192.168.107.146**
