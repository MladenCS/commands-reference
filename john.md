# John the Ripper

## Basic Usage
* Crack a hash file: `john [hashfile]`
* Crack with a wordlist: `john --wordlist=[wordlist] [hashfile]`
* Show cracked passwords: `john --show [hashfile]`
* Show cracked with username: `john --show --format=[format] [hashfile]`

## Preparing Files
* Combine passwd + shadow: `unshadow passwd shadow > hashes`
* Combine with custom filenames: `unshadow passwd.txt shadow.txt > hashes`

## Hash Formats
* Auto-detect format: `john [hashfile]` (John tries to detect automatically)
* Specify format: `john --format=[format] [hashfile]`
* List all supported formats: `john --list=formats`
* Common formats:
  * Linux shadow: `--format=sha512crypt`
  * MD5: `--format=md5`
  * NTLM (Windows): `--format=nt`
  * bcrypt: `--format=bcrypt`
  * SHA1: `--format=raw-sha1`
  * ZIP password: `--format=zip`
  * SSH key: `--format=ssh`

## Wordlist & Rules
* Basic wordlist attack: `john --wordlist=/usr/share/wordlists/rockyou.txt [hashfile]`
* Wordlist + mangling rules: `john --wordlist=[wordlist] --rules [hashfile]`
* Specific ruleset: `john --wordlist=[wordlist] --rules=Jumbo [hashfile]`
* Incremental (brute force): `john --incremental [hashfile]`
* Incremental digits only: `john --incremental=Digits [hashfile]`

## Mask Attack
* Custom mask: `john --mask=[mask] [hashfile]`
* 6 char lowercase: `john --mask=?l?l?l?l?l?l [hashfile]`
* 8 char alphanumeric: `john --mask=?a?a?a?a?a?a?a?a [hashfile]`
* Mask placeholders:
  * `?l` — lowercase letters
  * `?u` — uppercase letters
  * `?d` — digits
  * `?s` — symbols
  * `?a` — all (letters + digits + symbols)

## Session Management
* Start named session: `john --session=[name] [hashfile]`
* Restore interrupted session: `john --restore=[name]`
* Restore default session: `john --restore`

## Output & Status
* Show cracked passwords: `john --show [hashfile]`
* Check status (while running): press `Enter` or `q`
* Pot file (stores cracked): `~/.john/john.pot`
* Clear pot file (re-crack): `john --pot=/dev/null [hashfile]`

## Common Wordlist Locations (Kali Linux)
* rockyou: `/usr/share/wordlists/rockyou.txt`
* rockyou (gzipped): `/usr/share/wordlists/rockyou.txt.gz`
* Unzip rockyou: `gzip -d /usr/share/wordlists/rockyou.txt.gz`
* SecLists passwords: `/usr/share/seclists/Passwords/`

## Common Combinations
* Crack Linux shadow: `unshadow passwd shadow > hashes && john --wordlist=/usr/share/wordlists/rockyou.txt hashes`
* Crack NTLM hashes: `john --format=nt --wordlist=/usr/share/wordlists/rockyou.txt [hashfile]`
* Crack ZIP password: `zip2john file.zip > zip.hash && john --wordlist=/usr/share/wordlists/rockyou.txt zip.hash`
* Crack SSH private key: `ssh2john id_rsa > ssh.hash && john --wordlist=/usr/share/wordlists/rockyou.txt ssh.hash`
* Crack PDF password: `pdf2john file.pdf > pdf.hash && john --wordlist=/usr/share/wordlists/rockyou.txt pdf.hash`

## Helper Scripts (john2 utilities)
* ZIP to hash: `zip2john [file.zip] > hash`
* SSH key to hash: `ssh2john [id_rsa] > hash`
* PDF to hash: `pdf2john [file.pdf] > hash`
* RAR to hash: `rar2john [file.rar] > hash`
* KeePass to hash: `keepass2john [file.kdbx] > hash`
