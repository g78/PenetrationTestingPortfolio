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

**Command:** *lbd domaninName

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
The page is written in: "en".
The remote web server sent the HTTP header: "x-m-t" with value: "000011000011000000000000000000", which is quite uncommon and requires manual analysis.This information was found in the request with id 19.
The remote web server sent the HTTP header: "x-m-v" with value: "mps 6.3.66", which is quite uncommon and requires manual analysis.This information was found in the request with id 19.
The remote web server sent the HTTP header: "x-m-p" with value: "true", which is quite uncommon and requires manual analysis.This information was found in the request with id 19.
The remote web server sent the HTTP header: "x-s-c" with value: "05-25-003", which is quite uncommon and requires manual analysis.This information was found in the request with id 19.
The remote web server sent the HTTP header: "x-q-s" with value: "200000029000020000004", which is quite uncommon and requires manual analysis.This information was found in the request with id 19.
The remote web server sent the HTTP header: "x-s-v" with value: "intl.target.com/default v170 (f68103c13c6fd137884e59a678d0cc0bd48f9188)", which is quite uncommon and requires manual analysis.This information was found in the request with id 19.
The remote web server sent the HTTP header: "x-varnish" with value: "cl05vc05.c2.moovweb.net 430739534", which is quite uncommon and requires manual analysis.This information was found in the request with id 19.
The remote web server sent the HTTP header: "key" with value: "Cookie;s="moov_=dd38624a-ce1a-49f4-b70c-0ee51eea5a01"", which is quite uncommon and requires manual analysis.This information was found in the request with id 19.
The remote web server sent the HTTP header: "x-i-v" with value: "04043567-be99-42a9-932d-cde30c94c4b7/dd38624a-ce1a-49f4-b70c-0ee51eea5a01", which is quite uncommon and requires manual analysis.This information was found in the request with id 19.
The URL: "https://intl.target.com/" sent the cookie: "akavpau_Adaptive_Experience_VPA=1508963747~id=aa13a3dd25cd29e1eefdb49e5ad112ff; Domain=.target.com; Path=/".This information was found in the request with id 19.
A cookie without the secure flag was sent in an HTTPS response at "https://intl.target.com/". The secure flag prevents the browser from sending a "secure" cookie over an insecure HTTP channel, thus preventing potential session hijacking attacks.This vulnerability was found in the request with id 19.
A cookie without the HttpOnly flag was sent when  requesting "https://intl.target.com/". The HttpOnly flag prevents potential intruders from accessing the cookie value through Cross-Site Scripting attacks.This vulnerability was found in the request with id 19.
New URL found by web_spider plugin: "https://intl.target.com/watchtower/js/v3_0/watchtower.js"
New URL found by web_spider plugin: "https://intl.target.com/"
The URL: "https://intl.target.com/watchtower/js/v3_0/watchtower.js" sent the cookie: "GuestLocation=null|51.50|-0.12|EN|GB; Domain=.target.com,; expires=Thu, 26-Oct-2017 20:30:53 GMT; Path=/
 akavpau_Adaptive_Experience_VPA=1508963753~id=34936ede3f2f5a3098d5d05467949987; Domain=.target.com,; Path=/
 moov_=dd38624a-ce1a-49f4-b70c-0ee51eea5a01; Path=/".This information was found in the request with id 54.
A cookie without the secure flag was sent in an HTTPS response at "https://intl.target.com/watchtower/js/v3_0/watchtower.js". The secure flag prevents the browser from sending a "secure" cookie over an insecure HTTP channel, thus preventing potential session hijacking attacks.This vulnerability was found in the request with id 54.
A cookie without the HttpOnly flag was sent when  requesting "https://intl.target.com/watchtower/js/v3_0/watchtower.js". The HttpOnly flag prevents potential intruders from accessing the cookie value through Cross-Site Scripting attacks.This vulnerability was found in the request with id 54.
The following is a list of broken links that were found by the web_spider plugin:
- https://intl.target.com/watchtower/ [ referenced from: https://intl.target.com/watchtower/js/v3_0/watchtower.js ]
- https://intl.target.com/watchtower/js/ [ referenced from: https://intl.target.com/watchtower/js/v3_0/watchtower.js ]
- https://intl.target.com/watchtower/js/v3_0/ [ referenced from: https://intl.target.com/watchtower/js/v3_0/watchtower.js ]
Found 2 URLs and 3 different injections points.
The URL list is:
- https://intl.target.com/
- https://intl.target.com/watchtower/js/v3_0/watchtower.js
The list of fuzzable requests is:
- Method: GET | https://intl.target.com/
- Method: GET | https://intl.target.com/
- Method: GET | https://intl.target.com/watchtower/js/v3_0/watchtower.js
A comment containing "Added for Enable SHAPE for A2C" was found on these URL(s):
- https://intl.target.com/ (request with id: 19)
The whole target has no protection (X-Frame-Options header) against Click-Jacking attacks. This vulnerability was found in the requests with ids 19 and 54.
Password profiling TOP 100:
- [1] true with 197 repetitions.
- [2] function with 91 repetitions.
- [3] false with 79 repetitions.
- [4] return with 38 repetitions.
- [5] secure with 29 repetitions.
- [6] data with 20 repetitions.
- [7] length with 16 repetitions.
- [8] redsky with 16 repetitions.
- [9] marketing with 15 repetitions.
- [10] window with 15 repetitions.
- [11] content with 15 repetitions.
- [12] wcsstore with 15 repetitions.
- [13] img3 with 14 repetitions.
- [14] targetimg3 with 14 repetitions.
- [15] document with 14 repetitions.
- [16] mobile with 12 repetitions.
- [17] pageId with 12 repetitions.
- [18] path with 11 repetitions.
- [19] visitorId with 11 repetitions.
- [20] getCookieValueByName with 11 repetitions.
- [21] seoApi with 11 repetitions.
- [22] seoHttpRequest with 11 repetitions.
- [23] source with 11 repetitions.
- [24] sessionId with 10 repetitions.
- [25] else with 10 repetitions.
- [26] domain with 9 repetitions.
- [27] cookie with 9 repetitions.
- [28] ssoe with 9 repetitions.
- [29] byteArray with 9 repetitions.
- [30] tablet with 9 repetitions.
- [31] test with 9 repetitions.
- [32] replace with 8 repetitions.
- [33] setAttribute with 8 repetitions.
- [34] class with 8 repetitions.
- [35] uimod with 8 repetitions.
- [36] viewId with 8 repetitions.
- [37] force with 7 repetitions.
- [38] loadMode with 7 repetitions.
- [39] configJSON with 7 repetitions.
- [40] searchTerm with 7 repetitions.
- [41] zipcode with 7 repetitions.
- [42] Target with 7 repetitions.
- [43] indexOf with 7 repetitions.
- [44] static with 7 repetitions.
- [45] redoak with 7 repetitions.
- [46] forEach with 6 repetitions.
- [47] targetimg1 with 6 repetitions.
- [48] publish with 6 repetitions.
- [49] expires with 6 repetitions.
- [50] lazySizesConfig with 6 repetitions.
- [51] location with 6 repetitions.
- [52] experimentData with 6 repetitions.
- [53] Less with 5 repetitions.
- [54] pass with 5 repetitions.
- [55] Treatment with 5 repetitions.
- [56] routeId with 5 repetitions.
- [57] sapphirePreviewTreatment with 5 repetitions.
- [58] randomValue with 5 repetitions.
- [59] links with 5 repetitions.
- [60] Expect with 5 repetitions.
- [61] help with 5 repetitions.
- [62] navigator with 5 repetitions.
- [63] Date with 5 repetitions.
- [64] parentNode with 5 repetitions.
- [65] session with 5 repetitions.
- [66] userAgent with 5 repetitions.
- [67] setRequestHeader with 5 repetitions.
- [68] html with 5 repetitions.
- [69] JSON with 5 repetitions.
- [70] sizes with 5 repetitions.
- [71] gads with 5 repetitions.
- [72] appPath with 5 repetitions.
- [73] registry with 5 repetitions.
- [74] readCookie with 5 repetitions.
- [75] nodeName with 4 repetitions.
- [76] init with 4 repetitions.
- [77] crush with 4 repetitions.
- [78] template with 4 repetitions.
- [79] DPCI with 4 repetitions.
- [80] hexCharArray with 4 repetitions.
- [81] getElementsByTagName with 4 repetitions.
- [82] substring with 4 repetitions.
- [83] cname with 4 repetitions.
- [84] config with 4 repetitions.
- [85] footer with 4 repetitions.
- [86] taxonomy with 4 repetitions.
- [87] appPathStrings with 4 repetitions.
- [88] hidden with 4 repetitions.
- [89] split with 4 repetitions.
- [90] views with 4 repetitions.
- [91] category with 4 repetitions.
- [92] byteValue with 4 repetitions.
- [93] lazyClass with 4 repetitions.
- [94] mobilex with 4 repetitions.
- [95] getByteArrayFromInteger with 4 repetitions.
- [96] getUrlParameter with 4 repetitions.
- [97] Math with 4 repetitions.
- [98] hardRefresh with 4 repetitions.
- [99] view with 4 repetitions.
- [100] page with 4 repetitions.
Scan finished in 13 seconds.
Stopping the core...

```
### Web Application Firewalls

