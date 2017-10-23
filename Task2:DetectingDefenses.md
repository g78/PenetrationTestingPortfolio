# Task 2: Detecting Defenses
There are numerous different types of defenses that are deployed to either detect, slow down, confuse or probhibit an adversary from gaining information and access to resources.  I will demonstrate a few techniques I use to discover these entities.

## Load Balancers
Lets see if we can discover if a target is behind a load balancer by making serveral requests to the same target name and cross reference the returned data for changes in date, timestamp, server name and session id.

#
