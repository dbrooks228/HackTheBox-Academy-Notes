# Identifying Hashes

This document serves as a reference for identifying hash types using formats, tools like `hashid`, and the importance of context during analysis.

---

## Overview

Hash lengths and formats can often indicate the algorithm used:
- Example: A 32-character hash could be MD5 or NTLM.
- Example: Hash formats like `hash:salt` or `$id$salt$hash` provide additional clues.

### Common Hash Identifiers
- **$1$**: MD5  
- **$2a$**: Blowfish  
- **$2y$**: Blowfish with proper 8-bit character handling  
- **$5$**: SHA-256  
- **$6$**: SHA-512  

Example:
```bash
$6$vb1tLY1qiY$M.1ZCqKtJBxBtZm1gRi8Bbkn39KU0YJW1cuMFzTRANcNKFKR4RmAQVk4rqQQCkaJT6wXqjUkFcA/qNxLyqW.U/
```
- **$6$**: SHA-512
- **vb1tLY1qiY**: Salt
- Remaining: Hash value

---

## Using `hashid` to Identify Hashes

### Installation
```bash
pip install hashid
```

### Analyzing Single Hashes
```bash
hashid '$apr1$71850310$gh9m4xcAn3MGxogwX/ztb.'
```
Output:
```
[+] MD5(APR)
[+] Apache MD5
```

### Analyzing Multiple Hashes
```bash
hashid hashes.txt
```

### Including Hashcat Modes
```bash
hashid 'a2d1f7b7a1862d0d4a52644e72d59df5:500:lp@trash-mail.com' -m
```
Output includes possible Hashcat modes:
```
[+] Lastpass [Hashcat Mode: 6800]
```

---

## Importance of Context

Hash identification tools like `hashid` rely on regex and may provide multiple possibilities. Context helps narrow down options:
- **Source Example**: Was the hash obtained via Active Directory, SQL injection, or a Windows host?
- **Reference**: Use Hashcat’s example hashes to match the format and determine the correct mode.

### Example
```bash
hashid '2fc5a684737ce1bf7b3b239df432416e0dd07357:2014'
```
Output:
```
[+] SHA-1
[+] Double SHA-1
[+] Redmine Project Management Web App
```
Using the context of a salt (`:2014`), identify the correct hash type as SHA-1.

---

## Summary

1. Hash length and format provide clues to the algorithm.
2. Use tools like `hashid` to identify potential hash types.
3. Contextualize the source of the hash for accurate identification.
4. Reference Hashcat’s example hashes to confirm hash type and mode.
