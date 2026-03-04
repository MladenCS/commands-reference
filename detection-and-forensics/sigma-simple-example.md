# Simple Detection Rule with Sigma

### Context
* **Log Source**: Sysmon Event ID 1 (Process Creation)
* **Image**: `C:\Windows\System32\notepad.exe`
* **CommandLine**: `“C:\Windows\System32\NOTEPAD.EXE” C:\Users\John\Desktop\password.txt`

### Sigma Rule
```yaml
title: Potential Password Exposure (via cmdline)
author: Adam Swan
tags:
  - attack.t1552.001
  - attack.credential_access
logsource:
  product: windows
  category: process_creation
detection:
  selection:
    Image|endswith:
      - '\notepad.exe'
      - '\word.exe'
      - '\excel.exe'
      - '\wordpad.exe'
      - '\notepad++.exe'
    CommandLine|contains:
      - 'pass' # pass will match on password
      - 'pwd'
      - 'pw.' # pw.txt, etc.
      - 'account'
      - 'secret'
      - 'details'
  condition: selection
falsepositives:
  - Administrative scripts
level: low
