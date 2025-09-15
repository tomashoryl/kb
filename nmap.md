```
nmap 192.168.1.1 -> Scan single IP
nmap 192.168.1.10-20 -> Scan a range
nmap 192.168.1.0/24 -> CIDR scan
nmap -iL targets.txt -> Scan from file

-sL -> List targets only
-sn -> Disable port scanning
-Pn -> Disable host discovery (port scan only)
-PR -> ARP discovery on local network
-n -> No DNS resolution

-sS -> TCP SYN scan (default)
-sT -> TCP connect scan
-sU -> UDP port scan
-sA -> TCP ACK scan
-sW -> TCP Window scan

-p 21 -> Specific port
-p 21-100 -> Port range
-p- -> All ports
-F -> Fast scan (100 ports)
--top-ports 2000 -> Top ports

-sV -> Detect service versions
--version-intensity <0-9> -> Adjust accuracy/speed
--version-light -> Faster, less accurate
--version-all -> Full intensity
-A -> OS, version, scripts, traceroute
-O -> OS detection
--osscan-limit -> Requires open & closed ports
--osscan-guess -> Aggressive guessing

-T0 -> Paranoid (IDS detection)
-T3 -> Normal (default)
-T4 -> Aggressive (fast neworks)
-T5 -> Insane (very fast networks)
--min-rate <num> -> Minimum packet rate
--max-rate <num> -> Maximum packet rate

-f -> Fragment packets
-D <decoys> -> Decoy scans
-S <IP> -> Spoof source IP
--proxies <proxylist> -> Use proxies
--data-length <num> -> Append random data

-sC -> Default safe scripts
--script <name> -> Run specific script
--script http*. -> Run script category
--script-args <args> -> Pass arguments
Examples: http-sitemap-generator,smb-enum*,dns-brute

-oN file -> Normal output
-oX file -> XML output
-oG file -> Grepable output
-oA prefix -> All formats
-v/-vv -> Verbosity levels
```
