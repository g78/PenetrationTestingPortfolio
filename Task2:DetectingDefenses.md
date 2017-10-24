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
### IPS: Active Filter Detection
Active filter detection is achieved by sending packets that contain the strings found in real life IDS virus signatures.  The packet being sent to the target doesn't contain malicious content instead in contains the string found in the signature.  What we are looking to evaluate is the response, should the target respond with a RESET packet or blocks the source IP address then it is a strong indication that the host is being protected by an IPS.

```

```
