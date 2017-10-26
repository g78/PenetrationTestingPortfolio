# Task 2: Detecting Defenses
There are numerous different types of defenses that are deployed to either detect, slow down, confuse or probhibit an adversary from gaining information and access to resources.  I will demonstrate a few techniques I use to discover these entities.

## Load Balancers
Lets see if we can discover if a target is behind a load balancer by making serveral requests to the same target name and cross reference the returned data for changes in date, timestamp, server name and session id.

An excellent tool for this is halberd (https://github.com/jmbr/halberd)

**Command:** *halberd domainName*
```
root@kali:~/toolz/halberd-master# halberd target.com
halberd 0.2.4 (14-Aug-2010)

INFO looking up host target.com... 
INFO host lookup done.
INFO target.com resolves to 23.205.220.27
INFO target.com resolves to 23.205.220.64
23.205.220.27    [#######   ]  clues:   2 | replies: 196 | missed:   0

*** finished (Connection refused) ***

======================================================================
http://target.com (23.205.220.27): 1 real server(s)
======================================================================

server 1: BigIP
----------------------------------------------------------------------

difference: 0 seconds
successful requests: 196 hits (100.00%)
header fingerprint: d6db500e2c67e7ed9fd3aeca80249e0b923c9c3d
23.205.220.64    [##        ]  clues:   2 | replies:  30 | missed:   0

*** finished (Connection refused) ***

======================================================================
http://target.com (23.205.220.64): 1 real server(s)
======================================================================

server 1: BigIP
----------------------------------------------------------------------

difference: 0 seconds
successful requests: 30 hits (100.00%)
header fingerprint: d6db500e2c67e7ed9fd3aeca80249e0b923c9c3d
```

Compare this with Microsoft.com
```
root@kali:~/toolz/halberd-master# halberd microsoft.com
halberd 0.2.4 (14-Aug-2010)

INFO looking up host microsoft.com... 
INFO host lookup done.
INFO microsoft.com resolves to 104.40.211.35
INFO microsoft.com resolves to 104.43.195.251
INFO microsoft.com resolves to 191.239.213.197
INFO microsoft.com resolves to 23.100.122.175
INFO microsoft.com resolves to 23.96.52.53
104.40.211.35    [##        ]  clues:   3 | replies:  77 | missed:   0

*** finished (Connection refused) ***

======================================================================
http://microsoft.com (104.40.211.35): 1 real server(s)
======================================================================

server 1: Microsoft-IIS/8.5
----------------------------------------------------------------------

difference: 0 seconds
successful requests: 77 hits (100.00%)
header fingerprint: 3e97ffecc62fad9b99726f62959cd119a8c57da3
104.43.195.251   [#######   ]  clues:   3 | replies: 149 | missed:   0

*** finished (Connection refused) ***

======================================================================
http://microsoft.com (104.43.195.251): 1 real server(s)
======================================================================

server 1: Microsoft-IIS/8.5
----------------------------------------------------------------------

difference: 0 seconds
successful requests: 149 hits (100.00%)
header fingerprint: 3e97ffecc62fad9b99726f62959cd119a8c57da3
191.239.213.197  [##        ]  clues:   1 | replies:   9 | missed:   0

*** finished (Connection refused) ***

======================================================================
http://microsoft.com (191.239.213.197): 1 real server(s)
======================================================================

server 1: Microsoft-IIS/8.5
----------------------------------------------------------------------

difference: 0 seconds
successful requests: 9 hits (100.00%)
header fingerprint: 3e97ffecc62fad9b99726f62959cd119a8c57da3
23.100.122.175   [###       ]  clues:   3 | replies:  48 | missed:   0

*** finished (Connection refused) ***

======================================================================
http://microsoft.com (23.100.122.175): 1 real server(s)
======================================================================

server 1: Microsoft-IIS/8.5
----------------------------------------------------------------------

difference: 0 seconds
successful requests: 48 hits (100.00%)
header fingerprint: 3e97ffecc62fad9b99726f62959cd119a8c57da3
23.96.52.53      [##        ]  clues:   2 | replies:  23 | missed:   0

*** finished (Connection refused) ***

======================================================================
http://microsoft.com (23.96.52.53): 1 real server(s)
======================================================================

server 1: Microsoft-IIS/8.5
----------------------------------------------------------------------

difference: 0 seconds
successful requests: 23 hits (100.00%)
header fingerprint: 3e97ffecc62fad9b99726f62959cd119a8c57da3

```

Or Amazon.com
```
root@kali:~/toolz/halberd-master# halberd amazon.com
halberd 0.2.4 (14-Aug-2010)

INFO looking up host amazon.com... 
INFO host lookup done.
INFO amazon.com resolves to 54.239.17.6
INFO amazon.com resolves to 54.239.17.7
INFO amazon.com resolves to 54.239.25.192
INFO amazon.com resolves to 54.239.25.200
INFO amazon.com resolves to 54.239.25.208
INFO amazon.com resolves to 54.239.26.128
54.239.17.6      [####      ]  clues:   2 | replies:  70 | missed:   0

*** finished (Connection refused) ***

======================================================================
http://amazon.com (54.239.17.6): 1 real server(s)
======================================================================

server 1: Server
----------------------------------------------------------------------

difference: 0 seconds
successful requests: 70 hits (100.00%)
header fingerprint: bc9d8597424d60bfd45ce8bd193ab64c5ffaabc9
54.239.17.7      [##        ]  clues:   2 | replies:   5 | missed:   0

*** finished (Connection refused) ***

======================================================================
http://amazon.com (54.239.17.7): 1 real server(s)
======================================================================

server 1: Server
----------------------------------------------------------------------

difference: 0 seconds
successful requests: 5 hits (100.00%)
header fingerprint: bc9d8597424d60bfd45ce8bd193ab64c5ffaabc9
54.239.25.192    [##        ]  clues:   2 | replies:   6 | missed:   0

*** finished (Connection refused) ***

======================================================================
http://amazon.com (54.239.25.192): 1 real server(s)
======================================================================

server 1: Server
----------------------------------------------------------------------

difference: 0 seconds
successful requests: 6 hits (100.00%)
header fingerprint: bc9d8597424d60bfd45ce8bd193ab64c5ffaabc9
54.239.25.200    [##        ]  clues:   2 | replies:  29 | missed:   0

*** finished (Connection refused) ***

======================================================================
http://amazon.com (54.239.25.200): 1 real server(s)
======================================================================

server 1: Server
----------------------------------------------------------------------

difference: 0 seconds
successful requests: 29 hits (100.00%)
header fingerprint: bc9d8597424d60bfd45ce8bd193ab64c5ffaabc9
54.239.25.208    [########  ]  clues:   2 | replies: 195 | missed:   0

*** finished (Connection refused) ***

======================================================================
http://amazon.com (54.239.25.208): 1 real server(s)
======================================================================

server 1: Server
----------------------------------------------------------------------

difference: 0 seconds
successful requests: 195 hits (100.00%)
header fingerprint: bc9d8597424d60bfd45ce8bd193ab64c5ffaabc9
54.239.26.128    [##        ]  clues:   2 | replies:  31 | missed:   0

*** finished (Connection refused) ***

======================================================================
http://amazon.com (54.239.26.128): 1 real server(s)
======================================================================

server 1: Server
----------------------------------------------------------------------

difference: 0 seconds
successful requests: 31 hits (100.00%)
header fingerprint: bc9d8597424d60bfd45ce8bd193ab64c5ffaabc9

```
Another good tool to identify Load Balancers is ldb.  The tool looks at IP addresses, date / timestamps and the length of returned http headers to determine its results.

**Command:** *lbd domaninName*

```
root@kali:~# /usr/bin/lbd target.com

lbd - load balancing detector 0.4 - Checks if a given domain uses load-balancing.
                                    Written by Stefan Behte (http://ge.mine.nu)
                                    Proof-of-concept! Might give false positives.

Checking for DNS-Loadbalancing: FOUND
target.com has address 23.205.220.27
target.com has address 23.205.220.64

Checking for HTTP-Loadbalancing [Server]: 
 AkamaiGHost
 NOT FOUND
`
Checking for HTTP-Loadbalancing [Date]: 18:59:43, 18:59:43, 18:59:44, 18:59:44, 18:59:44, 18:59:44, 18:59:44, 18:59:44, 18:59:44, 18:59:45, 18:59:45, 18:59:45, 18:59:45, 18:59:45, 18:59:45, 18:59:45, 18:59:46, 18:59:46, 18:59:46, 18:59:46, 18:59:46, 18:59:46, 18:59:46, 18:59:47, 18:59:47, 18:59:47, 18:59:47, 18:59:47, 18:59:47, 18:59:47, 18:59:47, 18:59:48, 18:59:48, 18:59:48, 18:59:48, 18:59:48, 18:59:48, 18:59:48, 18:59:49, 18:59:49, 18:59:49, 18:59:49, 18:59:49, 18:59:49, 18:59:49, 18:59:50, 18:59:50, 18:59:50, 18:59:50, 18:59:50, NOT FOUND

Checking for HTTP-Loadbalancing [Diff]: FOUND
< Content-Length: 254
> Content-Length: 255

target.com does Load-balancing. Found via Methods: DNS HTTP[Diff]

```

### IPS: Active Filter Detection
Active filter detection is achieved by sending packets that contain the strings found in real life IDS virus signatures.  The packet being sent to the target doesn't contain malicious content instead in contains the string found in the signature.  What we are looking to evaluate is the response, should the target respond with a RESET packet or blocks the source IP address then it is a strong indication that the host is being protected by an IPS.

One of the many really good features of w3af is that is evaluates the target for active filter detection using its Infrastructure Plugin afd.  After configuring the scan the results are show below

```
w3af>>> start
The remote network has an active filter. IMPORTANT: The result of all the other plugins will be inaccurate, web applications could be vulnerable but "protected" by the active filter.This information was found in the request with id 1.
The following URLs were filtered:
- https://intl.target.com/?YXcGUQl=id;uname -a
- https://intl.target.com/?YXcGUQl=<? passthru("id");?>
- https://intl.target.com/?YXcGUQl=../../../../etc/passwd
- https://intl.target.com/?YXcGUQl=./../../../etc/motdhtml
- https://intl.target.com/?YXcGUQl=../../WINNT/system32/cmd.exe?dir+c:\
- https://intl.target.com/?YXcGUQl=../../../../bin/chgrp nobody /etc/shadow|
The following URLs passed undetected by the filter:
- https://intl.target.com/?YXcGUQl=SELECT TOP 1 name FROM sysusers
- https://intl.target.com/?YXcGUQl=exec xp_cmdshell dir
- https://intl.target.com/?YXcGUQl=exec master..xp_cmdshell dir
- https://intl.target.com/?YXcGUQl=ps -aux;
- https://intl.target.com/?YXcGUQl=type+c:\winnt\repair\sam._
Found 1 URLs and 1 different injections points.
The URL list is:
- https://intl.target.com/
The list of fuzzable requests is:
- Method: GET | https://intl.target.com/
Scan finished in 6 seconds.
Stopping the core...

```
## WAF: Web Application Firewalls
To test if the target is behind a web application firewall wafwoof.  Not only does detection take place but wafwoof on occassion is able to identify the WAF being used
**Command:** *wafw00f -a domainName -v*
```
root@kali:~# wafw00f -a https://intl.target.com

                                 ^     ^
        _   __  _   ____ _   __  _    _   ____
       ///7/ /.' \ / __////7/ /,' \ ,' \ / __/
      | V V // o // _/ | V V // 0 // 0 // _/
      |_n_,'/_n_//_/   |_n_,' \_,' \_,'/_/
                                <
                                 ...'

    WAFW00F - Web Application Firewall Detection Tool

    By Sandro Gauci && Wendel G. Henrique

Checking https://intl.target.com
Generic Detection results:
No WAF detected by the generic detection
Number of requests: 13
```

```
root@kali:~# wafw00f -a http://microsoft.com

                                 ^     ^
        _   __  _   ____ _   __  _    _   ____
       ///7/ /.' \ / __////7/ /,' \ ,' \ / __/
      | V V // o // _/ | V V // 0 // 0 // _/
      |_n_,'/_n_//_/   |_n_,' \_,' \_,'/_/
                                <
                                 ...'

    WAFW00F - Web Application Firewall Detection Tool

    By Sandro Gauci && Wendel G. Henrique

Checking http://microsoft.com
Generic Detection results:
The site http://microsoft.com seems to be behind a WAF or some sort of security solution
Reason: The server returned a different response code when a string trigged the blacklist.
Normal response code is "400", while the response code to an attack is "301"
Number of requests: 13

```

```
root@kali:~# wafw00f -a http://target.com

                                 ^     ^
        _   __  _   ____ _   __  _    _   ____
       ///7/ /.' \ / __////7/ /,' \ ,' \ / __/
      | V V // o // _/ | V V // 0 // 0 // _/
      |_n_,'/_n_//_/   |_n_,' \_,' \_,'/_/
                                <
                                 ...'

    WAFW00F - Web Application Firewall Detection Tool

    By Sandro Gauci && Wendel G. Henrique

Checking http://target.com
The site http://target.com is behind a F5 BIG-IP APM
Generic Detection results:
The site http://target.com seems to be behind a WAF or some sort of security solution
Reason: The server header is different when an attack is detected.
The server header for a normal response is "BigIP", while the server header a response to an attack is "AkamaiGHost.",
Number of requests: 12

```

