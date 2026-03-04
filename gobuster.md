# Gobuster

## Modes
* Directory/file brute force: `gobuster dir -u [url] -w [wordlist]`
* DNS subdomain enumeration: `gobuster dns -d [domain] -w [wordlist]`
* Virtual host enumeration: `gobuster vhost -u [url] -w [wordlist]`
* S3 bucket enumeration: `gobuster s3 -w [wordlist]`
* Fuzzing mode: `gobuster fuzz -u [url]?FUZZ=test -w [wordlist]`

## Dir Mode Options
* Specify file extensions: `gobuster dir -u [url] -w [wordlist] -x php,html,txt`
* Set threads: `gobuster dir -u [url] -w [wordlist] -t 50`
* Follow redirects: `gobuster dir -u [url] -w [wordlist] -r`
* Show response length: `gobuster dir -u [url] -w [wordlist] -l`
* Add custom header: `gobuster dir -u [url] -w [wordlist] -H "Authorization: Bearer [token]"`
* Use cookies: `gobuster dir -u [url] -w [wordlist] -c "session=[value]"`
* Use a proxy: `gobuster dir -u [url] -w [wordlist] --proxy http://127.0.0.1:8080`
* Skip SSL verification: `gobuster dir -u [url] -w [wordlist] -k`
* Filter by status code: `gobuster dir -u [url] -w [wordlist] -s 200,301,302`
* Exclude status codes: `gobuster dir -u [url] -w [wordlist] -b 404,403`
* Basic auth: `gobuster dir -u [url] -w [wordlist] -U [user] -P [password]`

## DNS Mode Options
* Basic subdomain scan: `gobuster dns -d [domain] -w [wordlist]`
* Show IP addresses: `gobuster dns -d [domain] -w [wordlist] -i`
* Use custom DNS resolver: `gobuster dns -d [domain] -w [wordlist] -r [dns-server]`

## Output
* Save results to file: `gobuster dir -u [url] -w [wordlist] -o output.txt`
* Verbose output: `gobuster dir -u [url] -w [wordlist] -v`
* No progress/no color (clean output): `gobuster dir -u [url] -w [wordlist] -q --no-color`

## Common Wordlist Locations (Kali Linux)
* Small: `/usr/share/wordlists/dirbuster/directory-list-2.3-small.txt`
* Medium: `/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`
* Large: `/usr/share/wordlists/dirbuster/directory-list-2.3-big.txt`
* SecLists common: `/usr/share/seclists/Discovery/Web-Content/common.txt`
* SecLists big: `/usr/share/seclists/Discovery/Web-Content/big.txt`
* SecLists subdomains: `/usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt`

## Common Combinations
* Quick dir scan: `gobuster dir -u http://[target] -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 50`
* Dir scan with extensions: `gobuster dir -u http://[target] -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt,bak -t 50`
* Scan & save output: `gobuster dir -u http://[target] -w /usr/share/seclists/Discovery/Web-Content/common.txt -o results.txt`
* Subdomain enumeration: `gobuster dns -d [domain] -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -i`
* Scan through Burp proxy: `gobuster dir -u http://[target] -w [wordlist] --proxy http://127.0.0.1:8080 -k`
