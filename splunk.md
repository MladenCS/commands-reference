# Splunk SPL (Search Processing Language)

## Basic Search
* Search all events: `index=*`
* Search specific index: `index=main`
* Search by sourcetype: `sourcetype=syslog`
* Search by host: `host=webserver01`
* Search by source: `source="/var/log/auth.log"`
* Combine filters: `index=main sourcetype=syslog host=webserver01`
* Search for keyword: `index=main "failed password"`
* Search multiple keywords (AND): `index=main "failed" "password"`
* Search multiple keywords (OR): `index=main ("error" OR "warning")`
* Exclude keyword: `index=main NOT "debug"`
* Wildcard search: `index=main user=admin*`
* Search time range (last 24h): `index=main earliest=-24h latest=now`
* Search specific time range: `index=main earliest="01/01/2024:00:00:00" latest="01/02/2024:00:00:00"`

## Fields
* Show specific field: `index=main | fields username`
* Show multiple fields: `index=main | fields username, src_ip, action`
* Remove field from results: `index=main | fields - _raw`
* Rename field: `index=main | rename src_ip AS "Source IP"`
* Check if field exists: `index=main | where isnotnull(username)`
* Search where field equals value: `index=main status=404`
* Search where field contains value: `index=main | where like(uri, "%admin%")`
* Search where field is not equal: `index=main status!=200`
* Search where field is greater than: `index=main bytes>1000`
* Search where field is in list: `index=main status IN (400, 401, 403, 404)`

## Filtering & Logic
* Filter with where: `index=main | where status=404`
* Filter with search: `index=main | search status=404`
* Multiple conditions (AND): `index=main | where status=404 AND method="POST"`
* Multiple conditions (OR): `index=main | where status=404 OR status=403`
* NOT condition: `index=main | where NOT status=200`
* Match regex: `index=main | where match(uri, "^/admin")`
* Case-insensitive search: `index=main | where lower(username)="admin"`

## Stats & Aggregation
* Count all events: `index=main | stats count`
* Count by field: `index=main | stats count by src_ip`
* Count by multiple fields: `index=main | stats count by src_ip, username`
* Count distinct values: `index=main | stats dc(username) by src_ip`
* Sum a field: `index=main | stats sum(bytes) by host`
* Average: `index=main | stats avg(response_time) by uri`
* Min and max: `index=main | stats min(bytes), max(bytes) by host`
* Multiple stats: `index=main | stats count, avg(bytes), max(bytes) by host`
* Earliest and latest event: `index=main | stats earliest(_time), latest(_time) by username`
* Values of a field: `index=main | stats values(src_ip) by username`
* List of field values: `index=main | stats list(uri) by src_ip`

## Sorting & Limiting
* Sort ascending: `index=main | sort src_ip`
* Sort descending: `index=main | sort - count`
* Sort by multiple fields: `index=main | sort - count, + src_ip`
* Limit results: `index=main | head 10`
* Last N results: `index=main | tail 10`
* Skip first N results: `index=main | stats count by src_ip | sort - count | head 20`

## Top & Rare
* Top values of a field: `index=main | top src_ip`
* Top N values: `index=main | top limit=20 src_ip`
* Top by multiple fields: `index=main | top src_ip, username`
* Least common values: `index=main | rare src_ip`
* Rare by multiple fields: `index=main | rare src_ip, username`

## Eval & Calculated Fields
* Create new field: `index=main | eval new_field = bytes / 1024`
* Concatenate fields: `index=main | eval full_name = first_name . " " . last_name`
* If condition: `index=main | eval status_type = if(status>=400, "error", "ok")`
* Case condition: `index=main | eval severity = case(status>=500, "critical", status>=400, "warning", true(), "ok")`
* Convert to uppercase: `index=main | eval username = upper(username)`
* Convert to lowercase: `index=main | eval username = lower(username)`
* String length: `index=main | eval uri_length = len(uri)`
* Round a number: `index=main | eval mb = round(bytes/1024/1024, 2)`
* Convert epoch to readable time: `index=main | eval readable_time = strftime(_time, "%Y-%m-%d %H:%M:%S")`
* Convert readable time to epoch: `index=main | eval epoch = strptime(date_string, "%Y-%m-%d")`
* Coalesce (first non-null): `index=main | eval user = coalesce(username, src_user, "unknown")`

## Timechart
* Events over time: `index=main | timechart count`
* Events over time by field: `index=main | timechart count by src_ip`
* Span by hour: `index=main | timechart span=1h count`
* Span by day: `index=main | timechart span=1d count`
* Average over time: `index=main | timechart avg(bytes) by host`
* Limit series: `index=main | timechart count by src_ip limit=5`
* index=your_index_name earliest="04/15/2022:08:05:00" latest="04/15/2022:08:06:00"

## Transaction
* Group events into transactions: `index=main | transaction session_id`
* Transaction with time gap: `index=main | transaction session_id maxpause=30s`
* Transaction max duration: `index=main | transaction session_id maxspan=1h`
* Count events per transaction: `index=main | transaction session_id | stats avg(eventcount)`
* Transaction by multiple fields: `index=main | transaction src_ip, username`

## Dedup & Unique
* Remove duplicates: `index=main | dedup src_ip`
* Dedup by multiple fields: `index=main | dedup src_ip, username`
* Keep last duplicate: `index=main | dedup src_ip sortby -_time`
* Count unique values: `index=main | stats dc(src_ip)`

## Rex & Regex Extraction
* Extract field with regex: `index=main | rex field=_raw "user=(?P<username>\w+)"`
* Extract multiple fields: `index=main | rex field=_raw "src=(?P<src_ip>[\d.]+) dst=(?P<dst_ip>[\d.]+)"`
* Replace with rex: `index=main | rex mode=sed field=uri "s/\?.*//g"`

## Lookup
* Use a lookup table: `index=main | lookup ip_lookup src_ip OUTPUT country`
* Lookup with output rename: `index=main | lookup user_lookup username OUTPUT department AS dept`
* Inputlookup (view lookup table): `| inputlookup ip_lookup.csv`
* Outputlookup (save to lookup): `index=main | stats count by src_ip | outputlookup ip_counts.csv`

## Joins & Subsearches
* Join two searches: `index=main sourcetype=access_log | join src_ip [search index=main sourcetype=firewall]`
* Left join: `index=main | join type=left src_ip [search index=firewall]`
* Subsearch: `index=main [search index=blacklist | fields src_ip]`
* Append results: `index=main sourcetype=linux_secure | append [search index=main sourcetype=wineventlog]`

## Alerts & Thresholds
* Count exceeds threshold: `index=main | stats count by src_ip | where count > 100`
* Failed logins per user: `index=main "failed password" | stats count by username | where count > 5`
* Spike detection: `index=main | timechart count | where count > (avg(count) * 2)`

## Security Use Cases

### Brute Force Detection
```
index=main "failed password" OR "authentication failure"
| stats count by src_ip, username
| where count > 10
| sort - count
```

### Top Talkers (Network)
```
index=network
| stats sum(bytes) as total_bytes by src_ip
| sort - total_bytes
| head 20
| eval total_mb = round(total_bytes/1024/1024, 2)
```

### HTTP Error Analysis
```
index=web sourcetype=access_log
| stats count by status, uri
| where status >= 400
| sort - count
```

### Rare User Agents (Suspicious Scanning)
```
index=web
| rare limit=20 http_user_agent
```

### Login Outside Business Hours
```
index=main sourcetype=linux_secure "Accepted password"
| eval hour = strftime(_time, "%H")
| where hour < 8 OR hour > 18
| stats count by username, src_ip
```

### New User Created
```
index=main sourcetype=wineventlog EventCode=4720
| stats count by Account_Name, Subject_Account_Name
```

### Privilege Escalation (sudo)
```
index=main sourcetype=syslog "sudo"
| rex "sudo:\s+(?P<user>\w+)"
| stats count by user, host
| sort - count
```

### Detect Port Scanning
```
index=network
| stats dc(dest_port) as port_count by src_ip
| where port_count > 50
| sort - port_count
```

### Data Exfiltration (Large Outbound)
```
index=network direction=outbound
| stats sum(bytes) as total by src_ip, dest_ip
| where total > 100000000
| eval total_mb = round(total/1024/1024, 2)
| sort - total_mb
```

### DNS Tunneling Detection
```
index=dns
| stats count, avg(query_length) as avg_len by src_ip
| where avg_len > 50 AND count > 100
```

## Useful SPL Functions Reference
| Function | Usage |
|---|---|
| `count` | Count events |
| `dc(field)` | Count distinct values |
| `sum(field)` | Sum numeric field |
| `avg(field)` | Average numeric field |
| `min(field)` | Minimum value |
| `max(field)` | Maximum value |
| `values(field)` | List all unique values |
| `list(field)` | List all values (with duplicates) |
| `earliest(field)` | Earliest value |
| `latest(field)` | Latest value |
| `strftime(_time, format)` | Format timestamp |
| `strptime(string, format)` | Parse timestamp string |
| `if(condition, true, false)` | Conditional evaluation |
| `case(cond1, val1, ...)` | Multi-condition evaluation |
| `coalesce(f1, f2, ...)` | First non-null value |
| `len(field)` | String length |
| `upper(field)` | Uppercase string |
| `lower(field)` | Lowercase string |
| `match(field, regex)` | Regex match (true/false) |
| `like(field, pattern)` | SQL-style pattern match |
| `round(num, decimals)` | Round number |
| `mvcount(field)` | Count multivalue field entries |
| `mvindex(field, index)` | Get value at index from multivalue |
| `split(field, delim)` | Split string into multivalue |
