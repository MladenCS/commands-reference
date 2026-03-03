# NMAP

## Scan Types
* TCP SYN Scan (Default/Stealth): `nmap -sS [target]`
* TCP Connect Scan: `nmap -sT [target]`
* UDP Scan: `nmap -sU [target]`
* ACK Scan (Firewall detection): `nmap -sA [target]`
* Ping Scan (Host discovery only): `nmap -sn [target]`

## Port Specification
* Specific port: `nmap -p 80 [target]`
* Port range: `nmap -p 1-1000 [target]`
* Multiple ports: `nmap -p 22,80,443 [target]`
* All ports: `nmap -p- [target]`
* Top 100 ports: `nmap --top-ports 100 [target]`

## Service & Version Detection
* Service/version detection: `nmap -sV [target]`
* OS detection: `nmap -O [target]`
* Aggressive scan (OS + version + scripts + traceroute): `nmap -A [target]`

## Speed & Timing
* Paranoid (IDS evasion): `nmap -T0 [target]`
* Sneaky: `nmap -T1 [target]`
* Polite: `nmap -T2 [target]`
* Normal (default): `nmap -T3 [target]`
* Aggressive: `nmap -T4 [target]`
* Insane: `nmap -T5 [target]`

## Scripting Engine (NSE)
* Default scripts: `nmap -sC [target]`
* Specific script: `nmap --script [script-name] [target]`
* Vulnerability scan: `nmap --script vuln [target]`
* Auth scripts: `nmap --script auth [target]`

## Output
* Normal output to file: `nmap -oN output.txt [target]`
* XML output: `nmap -oX output.xml [target]`
* Grepable output: `nmap -oG output.gnmap [target]`
* All formats: `nmap -oA output [target]`

## Evasion & Spoofing
* Fragment packets: `nmap -f [target]`
* Decoy scan: `nmap -D RND:10 [target]`
* Spoof source IP: `nmap -S [spoof-ip] [target]`
* Idle/zombie scan: `nmap -sI [zombie] [target]`
* Randomize host order: `nmap --randomize-hosts [target]`

## Common Combinations
* Full port scan + service detection: `nmap -p- -sV -T4 [target]`
* Stealth + version + default scripts: `nmap -sS -sV -sC [target]`
* Full aggressive scan: `nmap -A -p- -T4 [target]`
* Quick scan with OS detection: `nmap -O --top-ports 100 -T4 [target]`
