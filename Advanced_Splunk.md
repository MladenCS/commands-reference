# Advanced Splunk SPL

> Commands are chained together using the pipe symbol `|`. Each pipe passes the output of one command into the next, allowing you to refine results step by step.

## Search Operators

### Relational Operators
| Operator | Example | Explanation |
|---|---|---|
| `=` | `UserName = Mark` | Field equals value |
| `!=` | `UserName != Mark` | Field does not equal value |
| `<` | `Age < 10` | Field value less than |
| `<=` | `Age <= 10` | Field value less than or equal to |
| `>` | `Outbound_Traffic > 50` | Field value greater than |
| `>=` | `Outbound_Traffic >= 50` | Field value greater than or equal to |

### Logical Operators
| Operator | Example | Explanation |
|---|---|---|
| `NOT` | `UserName NOT David` | Ignore events where field contains value |
| `AND` | `UserName = David AND IPAddress = 10.10.10.10` | Both conditions must be true |
| `OR` | `UserName = David OR UserName = John` | Either condition must be true |

> Note: `AND` is implied between search terms — `AccountName != SYSTEM AccountName = James` is the same as `AccountName != SYSTEM AND AccountName = James`

### Wildcards
| Symbol | Example | Explanation |
|---|---|---|
| `*` | `status = fail*` | Matches any characters — returns `failed`, `failure`, etc. |

### Quotes & Parentheses
* Exact phrase match: `"failed login"` — matches the full phrase, not individual words
* Group conditions: `(UserName = David OR UserName = John) AND Status = failed`
* Evaluation order: `()` → `NOT` → `OR` → `AND`

---

## Filtering Commands

### search
* Filter by keyword: `index=windowslogs | search PowerShell`
* Useful when chaining commands together in a pipeline

### fields
* Include specific fields: `index=windowslogs | fields host User SourceIp`
* Exclude a field: `index=windowslogs | fields - _raw`
* Include explicitly: `index=windowslogs | fields + User`

### dedup
* Remove duplicate field values: `index=windowslogs | dedup SourceIp`
* Dedup with multiple fields: `index=windowslogs | fields EventID User Image Hostname SourceIp | dedup SourceIp`
* Keep most recent duplicate: `index=windowslogs | dedup SourceIp sortby -_time`

### rename
* Rename a field: `index=windowslogs | fields EventID User Image Hostname SourceIp | rename User as Employee`
* Rename multiple fields: `index=windowslogs | rename src_ip as "Source IP", dest_ip as "Destination IP"`

### regex
* Filter by regex pattern: `index=windowslogs | regex Image = "\.exe$"`
* Match start of string: `index=windowslogs | regex uri = "^/admin"`
* Match IP format: `index=windowslogs | regex SourceIp = "^\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}$"`

---

## Structuring & Display Commands

### table
* Display selected fields cleanly: `index=windowslogs | table _time EventID Hostname SourceName`
* Build investigation timeline: `index=windowslogs | table _time Hostname LogonType LogonProcessName RecordNumber`

### head / tail
* First N events (newest): `index=windowslogs | head 20`
* Last N events (oldest): `index=windowslogs | tail 20`

### sort
* Sort alphabetically: `index=windowslogs | sort User`
* Sort descending: `index=windowslogs | sort - count`
* Sort by multiple fields: `index=windowslogs | sort - count, + User`

### reverse
* Reverse event order: `index=windowslogs | reverse`

---

## Transforming Commands

### top / rare
* Most frequent values: `index=windowslogs | top User limit=5`
* Least frequent values: `index=windowslogs | rare User limit=5`

### highlight
* Highlight fields in raw view: `index=windowslogs | highlight User EventID Image "Process accessed"`
> Switch view to **Raw** mode to see highlights

### stats
* Count by field: `index=windowslogs | stats count by EventID | sort EventID`
* Average: `index=windowslogs | stats avg(ProcessCount)`
* Max: `index=windowslogs | stats max(Price)`
* Min: `index=windowslogs | stats min(UserAge)`
* Sum: `index=windowslogs | stats sum(Cost)`
* Multiple functions: `index=windowslogs | stats count, avg(bytes), max(bytes) by host`

### chart
* Visualize count by field: `index=windowslogs | chart count by User`
* Chart over time: `index=windowslogs | chart count by User span=1h`

### timechart
* Events over time: `index=windowslogs | timechart count`
* Top N processes over time: `index=windowslogs Image != "" | timechart span=30m count by Image limit=5`
* Average over time: `index=windowslogs | timechart avg(bytes) by host`

---

## Data Enrichment & Field Manipulation

### iplocation
* Add geo data to IP addresses: `index=windowslogs | iplocation SourceIp`
* Count events by country: `index=windowslogs | iplocation SourceIp | stats count by Country`
* Get city and region: `index=windowslogs | iplocation SourceIp | table SourceIp City Region Country`

### lookup
* Enrich with external CSV: `index=windowslogs | lookup user_roles Hostname OUTPUT UserRole`
* Count by enriched field: `index=windowslogs | lookup user_roles Hostname OUTPUT UserRole | stats count by Hostname UserRole`

### eval
* Create descriptive field from numeric: `index=windowslogs | eval LogonTypeDesc = case(LogonType == 3, "Network Logon", LogonType == 5, "Service") | stats count by LogonType LogonTypeDesc`
* Create calculated field: `index=windowslogs | eval size_mb = round(bytes/1024/1024, 2)`
* Conditional field: `index=windowslogs | eval result = if(status >= 400, "error", "ok")`

---

## Timeline & Correlation Queries

### Authentication Timeline
```
index=windowslogs EventID=4624
| table _time Hostname LogonType LogonProcessName RecordNumber
| dedup _time
| reverse
```

### Correlate User Activity Across Log Sources
```
index=windowslogs Hostname=Salena.Adam
| table _time Hostname EventID Category
| reverse
```

### Failed Login Timeline
```
index=windowslogs EventID=4625
| table _time Hostname User SourceIp LogonType
| reverse
```

### Account Not Equal to System
```
index=windowslogs AccountName != SYSTEM
```

### Account Not System but Include Specific User
```
index=windowslogs AccountName != SYSTEM AND AccountName = James
```

### Filter IPs by Subnet
```
index=windowslogs DestinationIp = 172.*
```

### Find Executable Activity
```
index=windowslogs | regex Image = "\.exe$"
| table _time Hostname User Image
| reverse
```

### PowerShell Activity
```
index=windowslogs | search PowerShell
| table _time Hostname User Image CommandLine
| reverse
```
