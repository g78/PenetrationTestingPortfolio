# Task 4: Port Scanning

## SYN Scan
nmap -sS <ip address>

## ACK Scan
nmap -sA <ip address>

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
NMAP -sS -T4 <ip address>

NMAP -sS -T Polite <ip address>

NMAP -sS --max-rtt-timeout 1250ms <ip address>

