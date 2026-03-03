# Linux Commands

## File Viewing
* Print file contents: `cat [file]`
* Print with line numbers: `cat -n [file]`
* View file interactively: `less [file]`
* First 10 lines: `head [file]`
* First N lines: `head -n [N] [file]`
* Last 10 lines: `tail [file]`
* Last N lines: `tail -n [N] [file]`
* Follow file in real time: `tail -f [file]`
* Follow file, show last N lines: `tail -f -n [N] [file]`

## File Operations
* Copy file: `cp [source] [dest]`
* Move/rename file: `mv [source] [dest]`
* Delete file: `rm [file]`
* Delete directory recursively: `rm -rf [dir]`
* Create empty file: `touch [file]`
* Create directory: `mkdir [dir]`
* Create nested directories: `mkdir -p [dir/subdir]`

## Search & Filter
* Search for pattern in file: `grep "[pattern]" [file]`
* Case-insensitive search: `grep -i "[pattern]" [file]`
* Show line numbers: `grep -n "[pattern]" [file]`
* Invert match (exclude pattern): `grep -v "[pattern]" [file]`
* Recursive search in directory: `grep -r "[pattern]" [dir]`
* Count matching lines: `grep -c "[pattern]" [file]`
* Search multiple files: `grep "[pattern]" *.log`

## Cut & Extract
* Cut by delimiter, get field N: `cut -d '[delim]' -f [N] [file]`
* Extract first field (space-delimited): `cut -d ' ' -f 1 [file]`
* Extract multiple fields: `cut -d ' ' -f 1,2 [file]`
* Extract field range: `cut -d ' ' -f 1-3 [file]`
* Cut by character position: `cut -c 1-10 [file]`

## Sort & Count
* Sort alphabetically: `sort [file]`
* Sort in reverse: `sort -r [file]`
* Sort numerically: `sort -n [file]`
* Sort numerically in reverse: `sort -n -r [file]`
* Sort by specific field: `sort -t '[delim]' -k [N] [file]`
* Remove duplicate lines: `sort -u [file]`
* Count occurrences (with sort + uniq): `sort [file] | uniq -c`
* Count & sort by most frequent: `sort [file] | uniq -c | sort -n -r`

## Pipelines (Log Analysis)
* Top IPs from access log: `cut -d ' ' -f 1 apache.log | sort | uniq -c | sort -n -r`
* Top N IPs: `cut -d ' ' -f 1 apache.log | sort | uniq -c | sort -n -r | head -n [N]`
* Count requests per IP: `awk '{print $1}' apache.log | sort | uniq -c | sort -n -r`
* Filter log by keyword: `grep "ERROR" app.log | tail -n 50`
* Count errors in log: `grep -c "ERROR" app.log`
* Extract unique HTTP status codes: `awk '{print $9}' apache.log | sort -u`

## AWK
* Print specific column: `awk '{print $[N]}' [file]`
* Print first and third column: `awk '{print $1, $3}' [file]`
* Filter rows by pattern: `awk '/[pattern]/' [file]`
* Filter and print column: `awk '/[pattern]/ {print $1}' [file]`
* Sum a column: `awk '{sum += $[N]} END {print sum}' [file]`

## SED
* Replace first occurrence per line: `sed 's/[old]/[new]/' [file]`
* Replace all occurrences: `sed 's/[old]/[new]/g' [file]`
* Replace in-place: `sed -i 's/[old]/[new]/g' [file]`
* Delete lines matching pattern: `sed '/[pattern]/d' [file]`
* Print specific line number: `sed -n '[N]p' [file]`

## Permissions
* Change file permissions: `chmod [permissions] [file]`
* Common permission sets: `chmod 644 [file]` / `chmod 755 [dir]`
* Change file owner: `chown [user]:[group] [file]`
* Recursive permission change: `chmod -R [permissions] [dir]`

## Disk & System
* Disk usage of directory: `du -sh [dir]`
* Disk usage, all subdirs: `du -h [dir]`
* Free disk space: `df -h`
* Running processes: `ps aux`
* Search running process: `ps aux | grep [name]`
* Kill process by PID: `kill [PID]`
* Force kill: `kill -9 [PID]`
* Kill by name: `pkill [name]`

## Networking
* Show network interfaces: `ip a`
* Show routing table: `ip r`
* Active connections: `ss -tuln`
* Active connections (with PIDs): `ss -tulnp`
* DNS lookup: `nslookup [domain]`
* Trace route: `traceroute [host]`
* Test connectivity: `ping -c 4 [host]`
* Download file: `curl -O [url]`
* Download with output name: `curl -o [filename] [url]`

## Compression
* Create tar archive: `tar -cvf archive.tar [dir]`
* Create gzip archive: `tar -czvf archive.tar.gz [dir]`
* Extract tar: `tar -xvf archive.tar`
* Extract tar.gz: `tar -xzvf archive.tar.gz`
* Unzip file: `unzip [file.zip]`
